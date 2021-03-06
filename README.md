# GetSimple SimpleDir Plugin
Provides a CSS style-able directory listing. The plugin includes a bunch of filetype icons.

Now with a configuration page to specify the directory path and URL, as well as any file-types to be ignored in the directory listing.

Updated from [RobA](http://get-simple.info/extend/plugin/simpledir/254/)'s original plugin.

# Installation
* Unzip the file to your plugin directory.
* Navigate to the **Plugins** Tab, and select the **SimpleDir Settings** menu items.
* Complete the **Full Server Path to Directory** and the **Base URL for Directory** and save the settings to set the properties for all SimpleDir instances globally. Leave them empty to default to your `data/upload/` path.
* Optionally, add a comma separated list (no spaces) of the extensions to not display.

There is also a css file in the `simpledir` subdirectory that can be used as a template for styling the table.

# Usage
* Create a page and include `(% simpledir %)` on that page - this will be replaced with a table. Parameters given below.

    ```
    (% simpledir %)
    ```
* Outside of pages, use function `get_simpledir_display` - API details given below.

    ```php
    <?php get_simpledir_display(); ?>
    ```

## Parameters
Additional parameters for the `(% simpledir %)` shortcode:

* `dirpath`: path relative to global path given in the admin panel.
* `urlpath`: path relative to the global URL path for the subdirectories.
* `ignore`: comma separated list (no spaces) of extensions to ignore, e.g. `php,htaccess`.  Leave empty to use value given in admin panel.
* `key`: alphanumeric identifier (used in the URL query string) to distinguish this instance of SimpleDir, e.g. `subdir2`. Needed if you have multiple instances of SimpleDir on the same page.

### Example
```
// Shows all files in /data/uploads/
(% simpledir key="subdir1" %)

// Shows all files in /data/uploads/images
(% simpledir key="subdir2" dirpath="images/" %)

// Shows all non-png files in data/uploads/
(% simpledir key="subdir3" ignore="png" %)
```

# API
## Display functions
* `get_simpledir_display($dirpath = null, $urlpath = null, $ignore = array(), $key = 'subdir')`: prints out a table of the contents of `$dirpath`.
  * `$dirpath`: same as `dirpath` above.
  * `$urlpath`: same as `urlpath` above.
  * `$ignore`: array of extensions to ignore, e.g. `array('php', 'htaccess')` 
  * `$key`: same as `key` above.

## Public functions
* `return_simpledir_display($dirpath = null, $urlpath = null, $ignore = array(), $key = 'subdir')`: returns a string of the table.
* `return_simpledir_results($dirpath = null, $urlpath = null, $ignore = array())`: returns an array of the contents of `$dirpath` with the following keys:
  * `subdirs` : array of subdirectories; each an associative array with keys `name`, `date`
  * `files`: array of files, each an associative array with keys `name`, `date`, `size`, `type`
  * `total`: total size of the files in this directory