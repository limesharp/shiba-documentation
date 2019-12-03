# Add sections or blocks to the template

In this example we are adding a block `about_banner` to the template

1. Create the `about-meta.php` file under `pub/wp/wp-content/themes/limesharp/metaboxes/` with the following content:

```php

<?php global $wpalchemy_media_access; ?>

<div class="my_meta_control metabox cms-images">
    <div class="my_meta_control metabox">
        <?php
        // Blocks Declaration ---------
        $block_types = [
            [
                'id' => 'about_banner',
                'name' => 'Banner Section'
            ]
        ];
        ?>

        <div class="meta-loop-wrap wrap-12 block-section">
            <?php while ($mb->have_fields_and_multi('blocks', ['sortable' => true])) : ?>
                <?php $mb->the_group_open(); ?>

                <!-- Start Block initialization -->
                <div class="meta-row">
                    <div class="meta-wrap wrap-12">
                        <div class="meta-field">
                            <label>Block Type</label>
                            <?php $mb->the_field('type'); ?>
                            <select name="<?php $mb->the_name(); ?>" class="block-selector">
                                <option value="">Please select block type</option>
                                <?php foreach ($block_types as $block) : ?>
                                    <option value="<?php echo $block['id'] ?>" <?php $mb->the_select_state($block['id']); ?>><?php echo $block['name'] ?></option>
                                <?php endforeach; ?>
                            </select>
                        </div>
                    </div>
                </div>
                <!-- End Block initialization -->

                <!-- Start Block editing      -->
                <div class="content-block-wrapper" data-block="<?php $mb->the_value(); ?>">
                    <?php include get_theme_file_path('metaboxes/about/banner.php') ?>
                </div>
                <?php echo $mb->get_the_delete_button(null, 'Remove Block from Page'); ?>
                <?php $mb->the_group_close(); ?>
            <?php endwhile; ?>
            <?php echo $mb->get_the_copy_button('blocks', 'Add Block To Page') ?>
        </div>
    </div>
</div>
```

**Explanation**

Important parts on the file:

```php
$block_types = [
    [
        'id' => 'about_banner',
        'name' => 'Banner Section'
    ]
];
```

```php
<?php include get_theme_file_path('metaboxes/about/banner.php') ?>
```

You can add as many sections or blocks to the template, for instance adding a "message" section:

```php
$block_types = [
    [
        'id' => 'about_banner',
        'name' => 'Banner Section'
    ],
    [
        'id' => 'about_message',
        'name' => 'Image Section'
    ]
];
```

```php
<?php include get_theme_file_path('metaboxes/about/banner.php') ?>
<?php include get_theme_file_path('metaboxes/about/message.php') ?>
```

-   Each block_type has an id, this is important because people can add as many sections as they want. This is handled here: `<?php while ($mb->have_fields_and_multi('blocks', ['sortable' => true])) : ?>`
