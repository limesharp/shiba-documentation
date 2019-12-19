# Sirv

You can read more about what is Sirv at https://sirv.com/.

## Component

### Usage

```
<sirv-img :image="imageObject" />
```

The `imageObject` should be the entire product image object, not the plain image URL, but an object as follows:

```
let imageObject = {
    label : 'Image label',
    url : 'Image full URL'
}
```

#### Sirv options

Sirv outside Shiba can be configured using the `data-options` data attribute on the `img` element. We can do the same in Shiba by passing those options as Vue.js `props` in the `<sirv-img />` component.

For example, if you want to configure a threshold of 150px and you can do the following:

```
<sirv-img :image="imageObject" :threshold="150px" />
```

Or for example, if you want to prevent Sirv from loading the image automatically so you can control that manually, you can do the following:

```
<sirv-img :image="imageObject" :autostart="false" />
```

If you decide to disable the `autostart`, is up to you to manually init Sirv on that `<img />` element as Sirv indicates in their official documentation.

```
Sirv.start(imgDomElement);
```

You can read more about what options you can pass to Sirv at https://sirv.com/help/resources/responsive-imaging/.

#### Custom options

There's a custom `prop` you can use to show a placeholder image while waiting for Sirv to load the real image, or if you just want to have a placeholder image on display before you initialize Sirv on that element manually in case you disabled the `autostart`.

```
<sirv-img :image="imageObject" :autostart="false" placeholder="https://cdn.example.com/placeholder.png" />
```

In this case, the `placeholder` component `prop` expects a full image URL.

### Definition

```
<template>
    <img
        :src="placeholder"
        :class="{'Sirv' : autostart, 'sirv-image-no-max-width': noMaxWidth}"
        :alt="imageMutated.label"
        :data-src="imageMutated.url"
        :data-options="dataOptions"
    />
</template>
```

The component script logic will take all given `props` and make a `string` with them to pass it to the `<img />` element using the `data-options` data attribute, as this is how Sirv expects it to be.

It will also use the `imageObject` component given `prop` to look for the image `label` and `url`.

The `src` of the `<img />` element will be the given `placeholder`, while the `data-src` will be the given `url` in the `imageObject`.

Sirv logic will take care of the rest after that element is rendered or after you manually init it.

## Back end

The Magento `Limesharp_Sirv` module implements a `preference` for `Magento\CatalogGraphQl\Model\Resolver\Product\ProductImage\Url` in order to return over all GraphQL calls the Sirv URL instead of the Magento URL for all product images.