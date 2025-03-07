---
title: Launch Check - WordPress Performance and Configuration Analysis
description: Learn more about the checks we automatically run on your Pantheon WordPress site.
cms: "WordPress"
categories: [go-live]
tags: [webops, launch]
reviewed: "2020-05-27"
---

Pantheon provides static site analysis as a service for WordPress sites to make best practice recommendations on site configurations. These reports are found in the Site Dashboard under the **Status** tab, and are accessible by site team members.

![status tab on live environment](../images/dashboard/status-tab.png)

## Overview

Every site is unique, with its own individual configuration, content, audience, and so forth. On Pantheon they're all built with one of two CMS frameworks, Drupal or WordPress, and have the same architectural requirements. Therefore, it's possible to provide recommendations that fit the vast majority of use cases using a technique known as static program analysis by gathering performance & behavior patterns to see how a site works.

This mechanism does not actually perform requests on your site, and in doing so avoids the observer effect. It's non-intrusive, so no installation or configuration is required. Finally, it's completely automated for consistent reports and results.

In short, you get a fast, repeatable report that can help detect common problems and provide introspection into your site.

## How Does it Work?

WP Launch Check is a site audit extension for WP-CLI designed for Pantheon customers. While designed initially for the Pantheon Dashboard it is intended to be fully usable outside of Pantheon.

## Run Launch Check Manually

To manually perform a site audit using WP Launch Check from the command line (using [Terminus](/terminus)), run:

```php{promptUser: user}
terminus wp <site>.<env> -- launchcheck <subcommand>
```

For more information about WP-CLI, visit their [GitHub page](https://github.com/wp-cli/wp-cli). For more information on WordPress Launch Check, go to the [GitHub repo](https://github.com/pantheon-systems/wp_launch_check/).

## What Does Launch Check Evaluate?

### Cron

This check verifies that Cron is enabled and what jobs are scheduled. It is enabled by default, but it if has been disabled you'll receive the following message:

`Cron appears to be disabled, make sure DISABLE_WP_CRON is not defined in your wp-config.php.`

### Database

The database stores your site's data including:

- pages and other content
- user data
- plugins and themes
- categories, tags, and system-wide settings
- tables 

Launch Check displays database stats, the number of rows in a given table, which tables are using InnoDB storage engine (suggests a query to run if not), transients, and expired transients. [Transients](https://developer.wordpress.org/apis/handbook/transients/) are cached data temporarily stored in the `wp_options` table.  

 The `wp_options` table stores several types of data for your site, including:

- settings for your plugins, widgets, and themes
- temporarily cached data
- site URL and home URL
- category settings
- autoloaded data

If your website is running slow and you receive the following message in the database stats: `consider autoloading only necessary options`, review [Optimize Your wp-options Table and Autoloaded Data](/optimize-wp-options-table-autoloaded-data).

#### What issues will I experience if I don't use InnoDB?

InnoDB has row level locking; MYISAM has table level locking. If a query is being performed on a table with MYISAM storage engine, no other query can modify the data until the first has given up its lock, which can result in tremendous performance issues for web applications.
To learn how to move your tables to InnoDB, see [Moving MySQL tables from MyISAM to InnoDB](/myisam-to-innodb).

### Probable Exploits

This check will display a list of exploited patterns in code, the file name that has the exploit, line number, and match.

### Object Cache

This tells you if Object Caching and Redis are enabled.

If you receive an error message similar to the example below, you'll need to move the `object-cache.php` from the plugin directory to `wp-content/object-cache.php`. For more information, see [Object Cache (formerly Redis) for Drupal or WordPress](/object-cache).
    
`Cannot redeclare class WP_Object_Cache in/srv/bindings0fef773f42984256a4f6feec2556a5ed/code/wp-content/plugins/wp-redis/object-cache.php`
    
### Plugins

This check lists all your enabled plugins and alerts you when they need to be updated. It also checks for any vulnerabilities.

- **Green:** All of your plugins are up-to-date
- **Yellow:** Highlighted plugins need to be updated
- **Red:** Displays all vulnerabilities and unsupported plugins

#### Unsupported Plugins

- [WP Super Cache](https://wordpress.org/plugins/wp-super-cache/)
- [W3 Total Cache](https://wordpress.org/plugins/w3-total-cache/)
- [batcache](https://wordpress.org/plugins/batcache/)

### PHP Sessions

Displays the files that references sessions. If any are found, you'll be prompted to install the [WordPress Native PHP Sessions](https://wordpress.org/plugins/wp-native-php-sessions) plugin.

## Support

If you have a feature request, message enhancements, or found a bug, please look at the [project's issues](https://github.com/pantheon-systems/wp_launch_check/issues) and submit a new issue if someone else has not already posted it. Pull requests are always welcome!

## See Also

If you have a Drupal site, see [Launch Check - Drupal Performance and Configuration Analysis](/drupal-launch-check).
