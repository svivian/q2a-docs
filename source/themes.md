---
layout: default
currentMenu: themes
---

# Creating themes for Question2Answer

Question2Answer supports multiple themes, and makes it easy for you to create your own. The HTML in its pages contains no visual formatting, so you can customize the look and feel using CSS only.

## Creating a CSS theme for Question2Answer

To create a new theme, follow the steps below:

1. Choose one of Question2Answer's standard themes as a starting point. Make a **copy** of its directory in Q2A's `qa-theme` directory, and give the copy your chosen theme name. The copied directory should at least contain the `qa-styles.css` file.

2. Log in to your Q2A site as an administrator, open the General section of the Admin panel, choose your new theme and save options.

3. Open the new `qa-styles.css` file in your text editor, and modify the formatting as required. Use the View Source feature in your browser to see how CSS classes are used.

4. As you make changes, refresh the browser page to see how they look. Be sure to test in different browsers.

5. Starting in Q2A 1.5, your theme can contain metadata which identifies you as the theme creator and allows Q2A to check for updates. The following metadata can appear in `qa-styles.css` file - all fields are optional:

	```language-php
	/*
		Theme URI: Web address for your theme
		Theme Version: Your theme version number
		Theme Date: Build date of your theme in YYYY-MM-DD
		Theme Author: Human-readable name of theme author
		Theme Author URI: Web address for plugin author
		Theme License: Short name of plugin license, e.g. GPLv2/3
		Theme Update Check URI: Web address for Q2A to check for updates
	*/
	```

	Some of this metadata will be displayed next to the theme setting in the Q2A admin interface. In addition, `Theme Update Check URI` allows you to inform users about new versions of the theme. Q2A will retrieve the content from `Theme Update Check URI`, and look for metadata in the same format as above. If the `Theme Version` values don't match, a message will show up, along with a link to the `Theme URI` from the online metadata. The simplest way to use this mechanism is to keep the latest version of `qa-styles.css` at a particular location online, and set that location as the `Theme Update Check URI`.


6. Please consider offering your theme online as a .zip file for others to download, then [telling us](./feedback.php) so we can [link to it here](addons.php#themes).

## Creating an advanced theme for Question2Answer

To go beyond CSS-only themes, it is possible to make changes to the HTML that Question2Answer generates for particular elements of content. This requires some PHP programming, and is explained in detail below:

1. Create a new theme directory using the same [steps](#theme) as for CSS-only themes.

2. Create a file named `qa-theme.php` in your new theme directory and insert the PHP code below. If you wish to use non English characters in your theme file, ensure your text editor is using UTF-8 encoding without a BOM (byte order mark).

	```language-php
	<?php

	class qa_html_theme extends qa_html_theme_base
	{
	}
	```

3. Use the View Source feature in your browser to find out which parts of the HTML you would like to change.

4. Look in the `qa-theme-base.php` file in Question2Answer's `qa-include` directory for the function which is responsible for generating that HTML. Most functions in `qa-theme-base.php` are named to match the CSS class of the HTML element that they generate. For example the function `page_links()` outputs `<DIV CLASS="qa-page-links">`...`</DIV>`.

5. For each function in `qa-theme-base.php` that you wish to override, create a function inside your `qa_html_theme` class with the same name and parameters. You can use `echo` or `print` to output HTML, but it is recommended to use `$this->output()`. This will automatically keep track of indentation - see examples of its use in `qa-theme-base.php`.

6. Use of DOM element IDs (e.g. `<DIV ID="...">`) is reserved for the Question2Answer control layer, so IDs should not be used by your theme to target CSS styling. Instead, use classes (e.g. `<DIV CLASS="...">`), which are just as easy to work with.

7. Within your custom function, you can call the default function in the `qa_html_theme_base` class using PHP's double colon (`::`) notation, e.g. `qa_html_theme_base::page_links();` This approach is better than copying and pasting code from `qa-theme-base.php`, since it will maximize the chance of your theme being compatible with future versions of Question2Answer.

8. To change behavior based on the type of page being shown, check `$this->template`. You can also check `$this->request` to see which page the user requested and `$this->context` to find out more about which part of the page you are currently in. The `print_r()` PHP function will make it easy for you to check these values in different situations.

9. In response to an Ajax request, some functions in your theme class may be called outside of the context of a full HTML page. Any modifications to these functions, or functions they call, should be tested accordingly. As of Q2A 1.5, this applies to: `a_list_item()`, `c_list_item()`, `favorite_inner_html()` and `voting_inner_html()`.

Below is an example which makes two changes to the standard theme. First, it reverses the order of the search and user navigation elements. Second, it changes the ellipsis (...) in the page links into an elongated hyphen (—) on the tags page.

```language-php
<?php

class qa_html_theme extends qa_html_theme_base
{
	function nav_user_search() // reverse order from qa_html_theme_base
	{
		$this->search();
		$this->nav('user');
	}

	function page_link_content($page_link) // override ellipsis behavior on tags page only
	{
		if ($page_link['type'] == 'ellipsis' && $this->template == 'tags')
			$this->output('<span class="qa-page-ellipsis">&mdash;</span>');
		else
			qa_html_theme_base::page_link_content($page_link);
	}
}
```
