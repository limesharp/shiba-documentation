# CMS Pages - Vue

Once the WordPress templates are created and some content is added in WordPress start creating the Vue files to get the information from WordPress

## Registering a new template

-   Go to `middleman/resources/assets/js/shiba/router/component-map.js`

```js
import aboutPage from "$shiba/pages/content/about.vue"

export default (route, type, payload) => {
    switch (type) {
        ...
        case "page":
            switch (payload.template) {
                case "template-about":
                    return aboutPage
                    break
                default:
                    return cmsPage
                    break
            }
        ...
        default:
            break
    }
}
```

**Explanation**

The important things here are to import the `aboutPage` (it will be created in the next step)
and return it if it is the case.

## Registering a new template

-   Create the about us Vue template under `middleman/resources/assets/js/shiba/pages/content/about-page.vue`

```vue
<template>
    <main class="default-page about-page" :class="post.post_name" v-if="post">
        <section v-for="(block, j) in post.meta_about.blocks" v-if="block.type" :key="j">
            <template v-if="block.type === 'about_banner'">
                <!-- Add html and read information here -->
            </template>
            <template v-if="block.type === 'about_message'">
                <!-- Add html and read information here -->
            </template>
        </section>
    </main>
</template>

<script>
import cmsPage from '$meem-theme/src/cms-page'

export default {
    props: [],
    extends: cmsPage,
    mounted() {
        this.$prerender.init()
        console.log('Mounted!')
    }
}
</script>
```

**Explanation**

-   Check what is in `post.meta_about.blocks` you will have an object with all the content from the page in WordPress
