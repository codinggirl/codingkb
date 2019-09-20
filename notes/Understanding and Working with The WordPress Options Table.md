---
title: Understanding and Working with The WordPress Options Table
created: '2019-09-05T01:17:51.898Z'
modified: '2019-09-05T01:18:04.221Z'
---

# Understanding and Working with The WordPress Options Table

by [Rachel McCollin](https://tutsplus.com/authors/rachel-mccollin)17 Sep 2014

Difficulty:IntermediateLength: Short Languages:English

[WordPress](https://code.tutsplus.com/categories/wordpress)[Web Development](https://code.tutsplus.com/categories/web-development) [Databases](https://code.tutsplus.com/categories/databases)[CMS](https://code.tutsplus.com/categories/cms)

This post is part of a series called [Understanding and Working with Data in WordPress](https://code.tutsplus.com/series/understanding-and-working-with-data-in-wordpress--cms-670) .

[Understanding and Working with Taxonomies and Terms in WordPress](https://code.tutsplus.com/tutorials/understanding-and-working-with-taxonomies-and-terms-in-wordpress--cms-21051)

[Understanding and Working with Data in WordPress \- Multisite](https://code.tutsplus.com/tutorials/understanding-and-working-with-data-in-wordpress-multisite--cms-21214)

In the earlier parts of this series, we looked at the tables in the WordPress database and the relationships between them.

In this part I'll cover a table which is different from the others \- the `wp_options` table. As you can see from the diagram below, this is the only table which sits on its own:

![](https://cms-assets.tutsplus.com/uploads/users/227/posts/21119/image/working-with-data-in-wordpress-introduction-database-tables.jpg)

The options table stores a different kind of data from the other tables: instead of storing data about your site's content, it stores data about the site itself. Data is written to the options table using the [Options API](http://codex.wordpress.org/Options_API) or the [Settings API](http://codex.wordpress.org/Settings_API), both of which consist of a set of functions used to add, update and delete data from this table.

You can add values to existing options and you can also add new records to the table when you want to create new options.

 In this tutorial I'll look at different aspects of the options table and how you interact with it:

*   Access to the `wp_options` table
*   Structure of the `wp_options` table
*   Populating the `wp_options` table
*   The Options API
*   The Settings API

I will just give an overview of the APIs and how they interact with the options table \- if you want to learn more, read Tom McFarlin's [series on the Settings API](https://code.tutsplus.com/tutorials/the-complete-guide-to-the-wordpress-settings-api-part-1-what-it-is-why-it-matters--wp-24060).

## Access to the wp\_options Table

As the `wp_options` table stores data which is related to the setup and administration of the site as a whole, access to it is restricted. To be able to amend settings and options, users will need to have the `[manage_options](http://codex.wordpress.org/Roles_and_Capabilities#manage_options)` capability. The only default user role with this capability is the administrator role (and in Multisite, the network administrator role).

This means that if you need to add options that other user roles have access to, you'll have to assign the `manage_options` capability to them. This carries risks, so only do it if you're sure!

## Structure of the wp\_options Table

The options table has a similar structure to the three metadata tables. It has four fields:

*   `option_ID`
*   `option_name`
*   `option_value`
*   `autoload` \- specifies whether the option is automatically loaded on each page load \- defaults to `yes` in a single site installation and `no` in Multisite.

Each record in the `option_name` field will be a unique value: if you add more than one value to an option, WordPress stores this in an array in the `option_value` field. A good example of this is the `active_plugins` option, which stores an array of plugins activated on your site.

When adding, editing or deleting data in the `wp_options` table, you must always specify the `option_name`, as I'll show later in this tutorial.

Advertisement

## Populating the wp\_options table

The `wp_options` table is populated from one of three sources:

*   the default Settings screens
*   theme options screens
*   settings and options screens which you add via plugins

There are a number of options built in to WordPress \- you can see all of these in the [Option Reference](http://codex.wordpress.org/Option_Reference). But you can also create your own.

To create new options in your theme or plugin, you would use the Options API or the Settings API. I'll cover these in more detail below.

## Using the Options API

The Options API consists of eight functions which allow you to add, get, update or delete options:

| Function | Parameters | Notes |
| --- | --- | --- |
| `[add_option()](http://codex.wordpress.org/Function_Reference/add_option)` | `$option`, `$value`, `$deprecated`, `$autoload`
 | Only `$option` is required. If there's an existing record with your `$option` parameter as the value of its `option_name` field, WordPress will add your `$value` to an array in the `option_value` field for that record, otherwise it will create a new record.
 |
| `[delete_option()](http://codex.wordpress.org/Function_Reference/delete_option)` | `$option` | Deletes all fields for that option |
| `[get_option()](http://codex.wordpress.org/Function_Reference/get_option)` | `$option`, `$default`
 | `$default` (optional) is the default value to return if no value is stored against the option in the database. |
| `[update_option()](http://codex.wordpress.org/Function_Reference/update_option)` | `$option`, `$new_value`
 | `$new_value` is the value which will populate the `option_value` field |
| `[add_site_option()](http://codex.wordpress.org/Function_Reference/add_site_option)` | `$option`, `$value`
 | Similar to `add_site_option()` but adds the option network\-wide in Multisite (meaning that the option is stored in the `wp_options` table and not the `wp_XX_options` table where `XX` is the site ID). `$autoload` is not included as site options do not autoload in Multisite and this cannot be overridden. |
| `[delete_site_option()](http://codex.wordpress.org/Function_Reference/delete_site_option)` | `$option`
 | The same as `delete_option()` but works network\-wide in Multisite. |
| `[get_site_option()](http://codex.wordpress.org/Function_Reference/get_site_option)` | `$option`, `$default` , `$use_cache`
 | Similar to `get_option()` but retrieves the network\-wide option in Multisite. |
| `[update_site_option()](http://codex.wordpress.org/Function_Reference/update_site_option)` | `$option`, `$value`
 | Identical to `update_option()` but works network\-wide in Multisite. |

Note that when creating options, either via the Options API or the Settings API, you can create records with no value in the `option_value` field. This allows site administrators to populate that field at a later time.

## Using the Settings API

As well as the Options API, you can also use the [Settings API](http://codex.wordpress.org/Settings_API) to interact with data in the `wp_options` table. The Settings API lets you create settings which site administrators can use to add or update data in the options table \- it adds a user interface to your options.

The Settings API has more to it than the Options API so I won't cover it in detail here, but essentially it has three elements:

*   the setting (the data in the `wp_options` table)
*   the field (which is used to add and edit data)
*   the settings section, which is a group of related fields.

The two functions in the Settings API which interact directly with the `wp_options` table are as follows:

| Function | Parameters | Notes |
| --- | --- | --- |
| `[register_setting()](http://codex.wordpress.org/Function_Reference/register_setting)` | `$option_group`, `$option_name`, `$sanitize_callback`
 | The `$option_name` parameter refers to the `option_name` field in the `wp_options` table; the other parameters interact with other functions in the Settings API |
| `[unregister_setting()](http://codex.wordpress.org/Function_Reference/unregister_setting)` | `$option_group`, `$option_name`, `$sanitize_callback`
 | Deregisters settings from the `wp_options` table \- normally used with deactivation hooks for themes or plugins.
 |

These functions don't add values to the options in the `wp_options` table, but they create settings which can than have values added to them via other functions in the Settings API.

## Summary

The `wp_options` table is unique among WordPress database tables in that it doesn't share a relationship with any of the other tables. This is because it stores data about the site or network, and not the content. To interact with the options table, you can use the functions in the Options API or the Settings API, and you can also make use of functions which add data network\-wide in a Multisite insulation.

In the final part of this series I'll look at Multisite, as it uses some additional database tables which have not been covered so far in this series, and also creates multiple instances of each of the core tables, one for each site.


[Understanding and Working with The WordPress Options Table](https://code.tutsplus.com/tutorials/understanding-and-working-with-the-wordpress-options-table--cms-21119)
