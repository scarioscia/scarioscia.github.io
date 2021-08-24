This repo was forked from the [AP Jekyll theme](https://github.com/kssim/ap), which was adapted from the [Tale](https://github.com/chesterhow/tale) theme. 

## Pages

* `index.md` - home/about
* `service/index.md` - separate page on academic service

## Formatting

### `_sass`

* `main.scss` - basically only contains symlinks

#### `base`

* `_base.scss` - default page layout
* `_code.scss` - code block formatting
* `_pagination.scss` - something with the page links on the header bar?
* `_syntax.scss` - weird
* `_variables.scss` - font and color variables

#### `layouts`

* `_footer.scss` - icons for social media
* `_home.scss` - layout for home/about page specifically
* `_layout.scss` - more general page layout? Incorporating header and footer?
* `_portfolio.scss` - for the portfolio page that I deleted
* `_post.scss` - layout for post format pages specifically

### `_includes`

* `head.html` - website logo
* `icons.html` - social media icons for footer of about page

## Other

* `_site` - weird HTML pages that auto-update

## Tabs
To add new tabs to your side, `mkdir tab_topic`, then add a line in `_layouts/base.html` below the existing tab lines with `tab_topic` as the directory and the words you'd like to click on your site.  
