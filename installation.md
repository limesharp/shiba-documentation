# Installation

## URL Definition

Since Shiba Commerce is intended to be separated from Magento, two URLs need to be defined to point to the actual Magento framework and to the Shiba Commerce frontend, so we have a way to access the Magento Admin Panel and browse the actual Store.

A common approach is to have two different subdomains, as `https://www.store.com/` for the Store and `https://admin.store.com/` for the Admin.

## Virtual Host Configuration

Having the two domains in mind, two Virtual Hosts need to be configured to point to `/path/to/magento-root/middleman/public` (Shiba Commerce frontend) and to `/path/to/magento-root/pub` (Magento framework).

Below is an example of how to have them configured.

```
<VirtualHost *:80>
    DocumentRoot "/path/to/magento-root/middleman/public"
    ServerName www.store.com

    <Directory /path/to/magento-root/middleman/public>
        Options Indexes FollowSymlinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
</VirtualHost>

<VirtualHost *:80>
    DocumentRoot "/path/to/magento-root/pub"
    ServerName admin.store.com

    <Directory /path/to/magento-root/pub>
        Options Indexes FollowSymlinks MultiViews
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
</VirtualHost>
```

The second one is the usual Virtual Host you would have to set when working on a classic Magento project without Shiba.

## Middleman Configuration

### .env file

Create a `magento-root/middleman/.env` file based on the `magento-root/middleman/.env.example` existing one, replacing the URLs with the correct ones, and configuring the right credentials for both MySql and Redis.

### Packages

Run `composer install` and then `npm install` in the `magento-root/middleman/` folder.

## Magento Configuration

### Web URLs

Under "_Stores -> Settings -> Configuration -> General -> Web_", at "_Default Config_" level, configure "_Base URL_", "_Base Link URL_", "_Secure Base URL_" and "_Secure Base Link URL_" with the Admin URL (`https://admin.store.com/`).

Configure the "_Base URL for Static View Files_" and "_Secure Base URL for Static View Files_" with the Admin URL as well (`https://admin.store.com/static/`).

Finally, configure the "_Base URL for User Media Files_" and "_Secure Base URL for User Media Files_" with the Admin URL as well (`https://admin.store.com/media/`).

Remember to set "_Use Secure URLs on Storefront_" and "_Use Secure URLs in Admin_" to "_Yes_", and double check the use of `http` or `https` on the fields above.

### Headless URL

Under "_Stores -> Settings -> Configuration -> Limesharp -> Headless Configuration_" configure the "_Site URL to be used_" with the Store URL (`https://www.store.com/`).

On the database, this is the `limesharp_shiba/general/site_url` path.