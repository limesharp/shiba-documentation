# Hide / show sections depending on the selected one

Before we created a template with one section id `about_banner`

-   This id is used to hide / show the section depending on what was selected.
-   If you create new IDs make sure to add the css for them in this file `pub/wp/wp-content/themes/me-em/metaboxes/meta.css`

```css
.content-block-wrapper[data-block='about_banner'] .content-block.about_banner {
    display: block;
}
```
