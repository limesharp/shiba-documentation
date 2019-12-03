# Creating the template

In this example we are going to create a template for the **about us page**

1. Create a file called `template-about.php` under `pub/wp/wp-content/thems/limesharp/` with the following content:

```php
<?php
// Template Name: About us
?>
```

2. Go to `pub/wp/wp-content/themes/limesharp/metaboxes/main-spec.php` and register a new MetaBox by adding `new WPAlchemy_MetaBox` as follows

```php
$about_mb = new WPAlchemy_MetaBox([
    'id' => '_about_mb',
    'title' => 'Page Content',
    'context' => 'normal',
    'priority' => 'high',
    'hide_editor' => 'true',
    'include_template' => ['template-about.php'],
    'template' => get_stylesheet_directory() . '/metaboxes/about-meta.php'
]);
```

At the end of the file, add the new box in the array of boxes:

```php
$mb_array = [
    'home' => $home_mb,
    'about' => $about_mb
];

```
