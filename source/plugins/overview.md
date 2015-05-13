---
layout: default
currentMenu: plugins
---

# Creating plugins for Question2Answer

Question2Answer plugins (introduced in Q2A 1.3) allow the platform to be modified or extended without changing the Q2A core. Plugins can integrate with Q2A in one of three ways:

- **[Modules](modules.php) extend Q2A in a defined and specific way.** The following types of module are supported from Q2A 1.3+:

	- [Page modules](modules.php?module=page) add a new type of page to a Question2Answer site.

	- [Login modules](modules.php?module=login) allow users to log in to Q2A via an external identity provider such as Facebook.

	- [Editor modules](modules.php?module=editor) provide a web interface for writing and editing posts.

	- [Viewer modules](modules.php?module=viewer) render content as HTML or text, and often work together with an editor module.

	From Q2A 1.4+:

	- [Event modules](modules.php?module=event) are notified when something important happens, such as content being posted or voted on.

	- [Widget modules](modules.php?module=widget) allow extra content to be shown on Q2A pages, as chosen by the site administrator.

	From Q2A 1.5+:

	- [Filter modules](modules.php?module=filter) can validate and/or modify many types of user input, including the content of posts.

	- [Search modules](modules.php?module=search) can implement a custom indexer and/or search engine for a Q2A site.

	- [Captcha modules](modules.php?module=captcha) provide a web interface for human verification, such as reCAPTCHA.

	- [Process modules](modules.php?module=process) have the opportunity to run at specific stages of Q2A's response processing.

	Modules are the preferred method for extending Q2A - [more about modules](modules.php).

- **[Layers](layers.php) modify the HTML output for some elements of a Q2A page**. Layers work similarly to [advanced themes](themes.php#advanced), by overriding some functions in the base theme class in `qa-theme-base.php`. Unlike advanced themes, multiple layers can be installed simultaneously via their respective plugins. Layers require Q2A 1.4+.

- **[Overrides](overrides.php) allow over 150 core Q2A functions to be modified**. Core functions can be replaced by the plugin's code, or they can be wrapped, with the plugin modifying the function's inputs and/or outputs. With wrapping, multiple overrides for the same function in different plugins can be active simultaneously. Overrides require Q2A 1.5+.

The sections below explain the general principles behind plugins. You will also need to read about implementing [modules](modules.php), [layers](layers.php) and/or [overrides](overrides.php), as well as some [functions](functions.php) in Q2A that may be useful in developing your plugin.

An alternative is to work through the [plugin tutorial](plugins-tutorial.php), which introduces all of this material in a more gentle and gradual way.

To see some examples of plugins, modules and layers, take a look at the `qa-plugin` directory in your Q2A installation as well as the modules registered in the function `qa_register_core_modules()` in `qa-base.php` of Q2A 1.5+.



## Directory structure

Plugins for Question2Answer have the following directory structure:


- A plugin is contained inside a directory whose name should use only numbers, lowercase letters and hyphens.

- The directory name should be descriptive and similar to the plugin's name, such as `facebook-login` or `wysiwyg-editor`.

- The plugin directory must contain a PHP file named `qa-plugin.php` whose purpose is described below.

- The plugin directory may also contain any other files and subdirectories, including PHP, HTML, Javascript, CSS, etc...

- A plugin is activated by installing its directory inside Q2A's `qa-plugin` directory.

The `qa-plugin.php` file has two main purposes. First, it registers any modules, layers or overrides provided by the plugin. Second, it contains optional metadata about the plugin and its author. As of Q2A 1.5, it is not recommended for `qa-plugin.php` to perform any other actions - if this seems necessary, please consider using a [process module](modules.php?module=process) instead. If you wish to use non English characters in `qa-plugin.php`, ensure your text editor is using UTF-8 encoding without a BOM (byte order mark).



## Registering modules

Each module is implemented as a PHP `class` with specific member functions, as [detailed here](modules.php). For the sake of speed, the class definition for each module should be in a separate PHP file, since Q2A will only `include` this file when necessary.

To register a module, call `qa_register_plugin_module()` in `qa-plugin.php`, with the following parameters in order:


1. The type of module, e.g. `'page'` or `'login'`. You can also use `'module'` to register a generic module which can display a form in the admin interface, but is not otherwise used.

2. The name of the PHP file in the plugin directory containing the module's class declaration, e.g. `qa-xml-sitemap.php`. Use `null` if the module's class is defined within `qa-plugin.php` itself (not recommended for non-trivial modules).

3. The name of the PHP `class` in this file that implements the module, e.g. `qa_xml_sitemap`.

4. A human-readable module name, which should be descriptive and unique, e.g. `'XML Sitemap'`. This may be displayed in the Admin panel and/or used to refer to the module within Q2A's internals.



## Registering layers

Each layer is implemented as a PHP file containing a fixed `class` declaration, as [described here](layers.php). To register a layer, call `qa_register_plugin_layer()` in `qa-plugin.php`, with the following parameters in order:


1. The name of the PHP file in the plugin directory containing the layer's class declaration, e.g. `qa-mouseover-layer.php`.

2. A human-readable layer name, which should be descriptive and unique, e.g. `'Mouseover Layer'`. This may also be used to refer to the layer within Q2A's internals.



## Registering overrides

Overrides are implemented as PHP files which contain the replacement function definitions, as [described here](overrides.php). Your functions can call the original Q2A functions using the `_base` suffix, e.g. `qa_lang_base()`. To register a file containing overrides, call `qa_register_plugin_overrides()` in `qa-plugin.php` with the name of the overrides PHP file in the plugin directory.



## Plugin localization

Throughout Q2A, the [functions](functions.php#qa_lang) `qa_lang()` and `qa_lang_html()` are used to obtain localized language phrases. An identifier string is passed to these functions, which consists of a file prefix, a slash (`/`) and a phrase key. For example, the identifier `'main/cancel_button'` obtains the element with key `'cancel_button'` from the appropriate `qa-lang-main.php` language file. Each language file is a PHP script which returns an array mapping keys to phrases, e.g. see `qa-lang-main.php`. If you wish to use non English characters in your language files, ensure your text editor is using UTF-8 encoding without a BOM (byte order mark).

From Q2A 1.5+, plugins can add their own language prefixes and phrases by calling `qa_register_plugin_phrases()` from within `qa-plugin.php`, with the following parameters in order:


1. A pattern for the language files within the plugin's directory. The pattern must contain an asterisk (`*`), which is substituted for the appropriate language code or `'default'`, which is used if the desired language cannot be found. For example, the pattern `'qa-phrases-*.php'` maps to `qa-phrases-fr.php` for French, `qa-phrases-de.php` for German and `qa-phrases-default.php` for defaults. The asterisk can also be in a directory name, e.g. the pattern `'lang-*/phrases.php'` would map to `lang-fr/phrases.php`, `lang-de/phrases.php` and `lang-default/phrases.php` respectively.

2. A prefix used to access the phrases, which must be unique to your plugin. For example, the prefix `'whizbang'` means that your phrase with key `'whizzy_title'` would be obtained by calling `qa_lang('whizbang/whizzy_title')`.



## Plugin metadata

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

### Plugin metadata prior to Q2A 1.7

In Q2A 1.5 and 1.6, plugin metadata was registered using metadata in a PHP comment inside the `qa-plugin.php` file. Here is an equivalent example to the above:

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

For backwards compatibility, Q2A 1.7+ will still read this old style of metadata, if no `metadata.json` file is present.
