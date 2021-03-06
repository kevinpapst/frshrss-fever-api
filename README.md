# FreshRSS - Fever API extension

This FreshRSS extension allows you to access FreshRSS with RSS readers that support the Fever API.

## Installation

To use it, upload the ```fever.php``` file to the FreshRSS location `/p/api/fever.php` on your server and enable API access in FreshRSS.

### RSS clients

There are many RSS clients existing, and they all seem to understand the Fever API a bit differently. 
If your favorite client doesn't work properly with this API, create an issue and I will have a look. 
But I can ONLY do that for free clients, as I am not willing to buy any RSS client. If you want to gift them to me, let me know and sent you my Google Play / App Store account ... but be aware that I can't promise anything, I don't know if I am able to fix their broken Fever API implementation. 

Special client implementation:
- The Press Android client needs (tested with 1.5.4) needs the additional file `fever-press.php` (use that file as endpoint in the Fever account setting)  

### Authentication

There is a drawback when using this plugin, which is the username and password combination that you have to use.
You have to set the API password as lowercase md5 sum from the string 'username:password' (this limitation exists, because the Fever API was designed in a way which is incompatible with the authentication system used by FreshRSS).

So if you use **kevin** as username and **freshrss** as password you have to set your FreshRSS API password to **4a6911fb47a87a77f4de285f4fac856d** (it MUST be lowercased, some tools seem to create it in uppercase which will not work!).

You can create the hash like this:
```bash
$ md5 -s "kevin:freshrss"
MD5 ("kevin:freshrss") = 4a6911fb47a87a77f4de285f4fac856d
```
You can also use online tools for calculating ypu API password hash if you don't have a md5 binary available.

In your favorite RSS reader you configure **fever.php** as endpoint, **kevin** as username and **freshrss** as password.  

## Compatibility

Tested with:

- PHP 7.1
- FreshRSS 1.8.1-dev
- iOS apps: Fiery Feeds, Unread

## Features
Following features are implemented:

- fetching categories
- fetching feeds
- fetching RSS items (new, favorites, unread, by_id, by_feed, by_category, since)
- fetching favicons
- setting read marker for item(s)
- setting starred marker for item(s)
- **hot** is not supported as there is nothing in FreshRSS that is similar
- setting read marker for feed
- setting read marker for category

## Testing and error search
If this API doesn't work as expected in your RSS reader, please test it manually with a tool like [Postman](https://www.getpostman.com/). 
Configure a POST request to the URL http://your-freshrss-url/api/fever.php?api which  should give you the result:
```
{
    "api_version": 3,
    "auth": 0
}
```
Great, the base setup seems to work!

Now lets try an authenticated call, so add a body to your POST request encoded as `form-data` and one key named `api_key` with the value `your-password-hash`, that should give you:
```
{
    "api_version": 3,
    "auth": 1,                               <= 1 means you were successfully authenticated
    "last_refreshed_on_time": "1520013061"   <= depends on your installation
}
```
Perfect, you are authenticated and can now start testing the more advanced features. Therefor change the URL and append the possible API actions to your request parameters. Check the [original Fever documentation](https://feedafever.com/api) for more infos. 

Some basic calls are:

- http://your-freshrss-url/api/fever.php?api&items
- http://your-freshrss-url/api/fever.php?api&feeds
- http://your-freshrss-url/api/fever.php?api&groups
- http://your-freshrss-url/api/fever.php?api&unread_item_ids
- http://your-freshrss-url/api/fever.php?api&saved_item_ids
- http://your-freshrss-url/api/fever.php?api&items&since_id=some_id
- http://your-freshrss-url/api/fever.php?api&items&max_id=some_id
- http://your-freshrss-url/api/fever.php?api&mark=item&as=read&id=some_id
- http://your-freshrss-url/api/fever.php?api&mark=item&as=unread&id=some_id

Replace `some_id` with a real ID from your `freshrss_username_entry` database.

### Debugging

If nothing helps and your clients still misbehaves, add these lines to the start of `fever.api``

```php
file_put_contents(__DIR__ . '/fever.log', $_SERVER['HTTP_USER_AGENT'] . ': ' . json_encode($_REQUEST) . PHP_EOL, FILE_APPEND);
```

Then use your RSS client to query the API and afterwards check the file `fever.log`.

## About FreshRSS
[FreshRSS](https://freshrss.org/) is a great self-hosted RSS Reader written in PHP, which is can also be found here at [GitHub](https://github.com/FreshRSS/FreshRSS).

More extensions can be found at [FreshRSS/Extensions](https://github.com/FreshRSS/Extensions).

## Changelog

* Initial version

## Credits and license

This plugin was inspired by the [tinytinyrss-fever-plugin](https://github.com/dasmurphy/tinytinyrss-fever-plugin).
Thanks to @dasmurphy for sharing it!

The original plugin was released under GNU GPL version 2. I have no clue what that means for this plugin ... 
