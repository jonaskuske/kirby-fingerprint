# Kirby 3 Fingerprint

A little utility to add cache-busting fingerprints to assets (JS/CSS) in Kirby 3.
Re-uses the [`css()`](https://getkirby.com/docs/reference/templates/helpers/css) and [`js()`](https://getkirby.com/docs/reference/templates/helpers/js)-helpers in Kirby 3 as much as possible.

When the files are updated, new hashes are added to the filenames automatically; so browser cache gets busted.

## Installation

- unzip [master.zip](https://github.com/bvdputte/kirby-fingerprint/archive/master.zip) as folder `site/plugins/kirby-fingerprint` or
- `git submodule add https://github.com/bvdputte/kirby-fingerprint.git site/plugins/kirby-fingerprint`
- `composer require bvdputte/kirby-fingerprint`

<br>

> If you can't change your server setup, use the [query option](#options) which generates URLs in the form of **`?v=<md5_hash>`**.  
> But be aware that [query strings are not perfect](http://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/).


### Apache .htaccess rules

💡 Add the following to your `.htaccess` file:

```
# Bust browsercache on CSS & JS files
# More info: https://github.com/bvdputte/kirby-fingerprint
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.+)\.([0-9a-z]{32})\.(js|css|png|jpe?g|gif|svg|ico)$ $1.$3 [L]
```

Place immediately after the `RewriteBase` definition.

### Nginx rules

Add the following to your virtual host setup:

```
location ~ (.+)\.(?:\d+)\.(js|css)$ {
    try_files $uri $1.$2;
}
```

## Usage

```php
css("assets/css/styles.css");
// Output: <link href="//localhost:3000/assets/css/styles.db5796ea5bf253bb7be3526eb083e068.css" rel="stylesheet">
js("assets/js/scripts.js");
// Output: <script src="//localhost:3000/assets/js/scripts.1e9dd0c95e7b12ce96729501c7585deb.js"></script>
```

## Options

Use query string:

```php
// config.php
return [
    'bvdputte.fingerprint.query' => true
];
```

Disable plugin:

```php
// config.php
return [
    'bvdputte.fingerprint.disabled' => true
];
```

## Advanced features

For more advanced features, such as subresource integrity, please checkout [bnomei's kirby3-fingerprint plugin](https://github.com/bnomei/kirby3-fingerprint).

## Disclaimer

This plugin is provided "as is" with no guarantee. Use it at your own risk and always test it yourself before using it in a production environment. If you find any issues, please [create a new issue](https://github.com/bvdputte/kirby-fingerprint/issues/new).

## License

[MIT](https://opensource.org/licenses/MIT)

It is discouraged to use this plugin in any project that promotes racism, sexism, homophobia, animal abuse, violence or any other form of hate speech.
