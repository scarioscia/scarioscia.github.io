---
layout: post
title: Extracting a small p-value from R
---

So often, I see that dreaded `p-value < 2.2e-16` in the output of my statistical test or model in R. It turns out that's the automatic rounding. I find myself trying to extract the exact value often enough that I thought I'd document a couple of the common use-cases here. 

A really simple option is the binomial test, `binom.test()`. Here we have simulated data that will give us the rounded value:
```
aneu_gain_mat <- 1090
aneu_gain_pat <- 1
aneu_loss_mat  <- 69
aneu_loss_pat  <- 220

tab <- matrix(c(aneu_gain_mat, aneu_gain_pat,
                aneu_loss_mat,  aneu_loss_pat),
              nrow = 2, byrow = TRUE,
              dimnames = list(ploidy = c("Chr Gain", "Chr Loss"),
                              origin = c("Maternal", "Paternal")))

# aneuploidy gain: maternal > 0.5?
bt_mat <- binom.test(aneu_gain_mat, aneu_gain_mat + aneu_gain_pat, p = 0.5, alternative = "greater")

# aneuploidy_loss: paternal > 0.5?
bt_pat <- binom.test(aneu_loss_pat, aneu_loss_mat + aneu_loss_pat, p = 0.5, alternative = "greater")
```

For either, to extract the p-value, you can grab the object `bt_mat$p.value`. 

For a generalized linear model it's a bit more involved. We can set our `glm` and then use the `summary()` function to extract the p-value without rounding. First we simulate a glm:
```
set.seed(1)

n <- 100                           # rows
num_genes  <- rnorm(n)             # predictor 1
chr_length <- rnorm(n)             # predictor 2

# coefficients 
beta0 <- 0
beta1 <- -0.1                      # num_genes has a negative effect
beta2 <-  0.1                      # chr_length has a positive effect

# generate probabilities on the logit scale, then binomial counts
N <- rep(500, n)                   # trials per row 
eta <- beta0 + beta1*num_genes + beta2*chr_length
p   <- plogis(eta)                 # convert to 0–1

monosomy <- rbinom(n, size = N, prob = p)
trisomy  <- N - monosomy

summary_table <- data.frame(num_genes, chr_length, monosomy, trisomy)

# fit model 
m1 <- glm(cbind(monosomy, trisomy) ~ num_genes + chr_length,
          family = binomial, data = summary_table)

summary(m1)

# exact numeric p-values (no "<2e-16" clipping)
format.pval(summary(m1)$coefficients[, "Pr(>|z|)"], digits = 22, eps = 1e-300)
```

Stepwise, we can extract the p-value via the summary: 
```
sm <- summary(m1)

# numeric p-values as stored (two-sided)
pvals <- sm$coefficients[, "Pr(>|z|)"]

# print without the "<2e-16" clipping
format.pval(pvals, digits = 22, eps = 1e-300)
```

I hope this helps! 