# FreshRSS - Fever API extension

This FreshRSS extension allows you to access FreshRSS with RSS readers that support the Fever API.

## Installation

To use it, upload the ```fever.php``` file to the FreshRSS location `/p/api/fever.php` on your server and enable API access in FreshRSS.

There is a major drawback when using this plugin, which is the username and password combination that you have to use.

## Features
Following Features are implemented:

- fetching categories
- fetching feeds
- fetching RSS items (new, favorites, unread, by_id, by_feed, by_category, since)
- fetching favicons
- **hot** is not supported as there is nothing in FreshRSS that is similar

## Roadmap
Support will follow soon:

- setting read marker for item(s)
- setting starred marker for item(s)

## About FreshRSS
[FreshRSS](https://freshrss.org/) is a great self-hosted RSS Reader written in PHP, which is can also be found here at [GitHub](https://github.com/FreshRSS/FreshRSS).

More extensions can be found at [FreshRSS/Extensions](https://github.com/FreshRSS/Extensions).

## Changelog

* Initial version

## Credits and license

This plugin was higjly inspired by the [tinytinyrss-fever-plugin](https://github.com/dasmurphy/tinytinyrss-fever-plugin).
Thanks to @dasmurphy for sharing it!

The original plugin was released under 