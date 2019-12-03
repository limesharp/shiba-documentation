# Creating Sections Content

Create a file `banner.php` under `pub/wp/wp-content/themes/limesharp/metaboxes/blog/post/banner.php` with the following content:

```php
<?php
// Preconditions
// id: about_banner
?>
<div class="content-block about_banner">
    <div class="meta-row">
        <div class="meta-wrap wrap-3">
            <h3>Banner Section</h3>
            <!-- Style -->
            <div class="meta-field">
                <p>
                    <?php $mb->the_field("banner__style", WPALCHEMY_FIELD_HINT_RADIO) ?>
                    <label>
                    <input type="radio" name="<?php $mb->the_name(); ?>" value="full" <?php $mb->the_radio_state('full') ?> />Banner full width</label>
                    <label>
                    <input type="radio" name="<?php $mb->the_name(); ?>" value="half" <?php $mb->the_radio_state('half') ?> />Banner contained</label>
                </p>
            </div>
            <!-- Content -->
            <div class="meta-field">
                <label>Main Description</label>
                <?php $mb->the_field('banner__content'); ?>
                <?php $mb->get_the_wp_editor($mb->get_the_value(), $mb->get_the_name()); ?>
            </div>
            <!-- Title -->
            <div class="meta-field">
                <?php $mb->the_field('banner__title'); ?>
                <label>Label</label>
                <input type="text" name="<?php $mb->the_name(); ?>" value="<?php $mb->the_value(); ?>">
            </div>
            <!-- Image -->
            <div class="meta-field">
                <?php $fieldName = 'banner__image'; ?>
                <?php require get_template_directory() . '/metaboxes/components/sirv-image.php' ?>
            </div>
        </div>
    </div>
</div>

```

**Explanation**

-   The ID `about_banner` is really important. It is added as one of the classes in the div `<div class="content-block about_banner">`
-   To add a new element use `<?php $mb->the_field('element'); ?>`. For instance, in this example we are adding a style, title, content, and image.
-   Please note the image is a reusable component that will add an image from Sirv, this component (`sirv-image`) might not be available if the project does not use Sirv.
