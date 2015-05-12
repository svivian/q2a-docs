---
layout: default
currentMenu: plugin-metadata
---

# Plugin Metadata

Plugins can list "metadata" - information about the plugin itself - in a JSON file. Currently the metadata is used in two particular places:

1. On the Plugins page in the Admin area, to show the plugin details.
2. When loading each plugin, to check if it can run in the current version of Q2A and PHP. (Any plugins not meeting the minimum requirements will be disabled.)

The data is stored in a file `metadata.json` in each plugin's directory. The JSON format consists of key-value pairs of the form `"key": "value"`, separated by commas and wrapped in curly braces `{ ... }`. Note: JSON requires that both keys and values must be in double quotes, and there be no trailing comma after the last item (before `}`).

All fields are optional (you can omit any whole line). Here's an example with all the possible keys and their descriptions:

```language-javascript
{
	"name": "Human-readable name of your plugin",
	"uri": "Web address for your plugin",
	"description": "Human-readable description of your plugin",
	"version": "Your plugin version number",
	"date": "Build date of your plugin in YYYY-MM-DD",
	"author": "Human-readable name of plugin author",
	"author_uri": "Web address for plugin author",
	"license": "Short name of plugin license, e.g. GPLv2",
	"update_uri": "Web address for Q2A to check for updates",
	"min_q2a": "Numerical part only, e.g. 1.3",
	"min_php": "Numerical part only, e.g. 5.1"
}
```

The `update_uri` key allows you to inform users about new versions of the plugin. Q2A will retrieve the content from the provided URL, and look for metadata in the same format as above. In other words, it should contain a URL to a JSON file. If you are using a public repository such as Github, you can link to the "raw" version of `metadata.json` in your own repo - this way, as soon as you push an updated `metadata.json` to your repo, the admin/plugins page will notify users of the new version.


## Prior to Q2A 1.7

In earlier Q2A versions, plugin metadata was registered using metadata in a PHP comment inside the `qa-plugin.php` file. Here is an equivalent example to above:

```language-php
/*
	Plugin Name: Human-readable name of your plugin
	Plugin URI: Web address for your plugin
	Plugin Description: Human-readable description of your plugin
	Plugin Version: Your plugin version number
	Plugin Date: Build date of your plugin in YYYY-MM-DD
	Plugin Author: Human-readable name of plugin author
	Plugin Author URI: Web address for plugin author
	Plugin License: Short name of plugin license, e.g. GPLv2
	Plugin Update Check URI: Web address for Q2A to check for updates
	Plugin Minimum Question2Answer Version: Numerical part only, e.g. 1.3
	Plugin Minimum PHP Version: Numerical part only, e.g. 5
*/
```
