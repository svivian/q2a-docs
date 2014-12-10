---
currentMenu: install
---

# Installation

Question2Answer takes 5 minutes (or less!) to install. In most cases, installing Question2Answer for your website should be dead easy. Please follow the steps below.

## Before you install Question2Answer

Make sure you have:

- A web server which you can access via FTP/SFTP. You might find [Cloudlook](http://www.cloudlook.com/) helpful to evaluate cloud hosting providers.
- A text editor.
- A web browser.

And make sure your web server is running:

- Web serving software such as Apache or Nginx.
- PHP 5.1.6 or later.
- MySQL 4.1 or later, MySQL 5.x for best performance.

If you are not sure about this, please check with your web hosting provider.


	<?php
	define('QA_VERSION', '1.7.0-beta-2'); // also used as suffix for .js and .css requests
	define('QA_BUILD_DATE', '2014-12-06');


	/**
	 * Autoloads some Q2A classes so it's possible to use them without adding a require_once first. From version 1.7 onwards.
	 * These loosely follow PHP-FIG's PSR-0 standard where faux namespaces are separated by underscores. This is being done
	 * slowly and carefully to maintain backwards compatibility, and does not apply to plugins, themes, nor most of the core
	 * for that matter.
	 *
	 * Classes are stored in the qa-include/Q2A folder, and then in subfolders depending on their categorization.
	 * Class names should be of the form Q2A_<Namespace>_<Class>, e.g. Q2A_Util_Debug. There may be multiple "namespaces".
	 * Classes are mapped to PHP files with the underscores converted to directory separators. The Q2A_Util_Debug class is in
	 * the file qa-include/Q2A/Util/Debug.php. A class named Q2A_Db_User_Messages would be in a file qa-include/Q2A/Db/User/Messages.php.
	 */
	function qa_autoload($class)
	{
		if (strpos($class, 'Q2A_') === 0)
			require QA_INCLUDE_DIR.strtr($class, '_', '/') . '.php';
	}
	spl_autoload_register('qa_autoload');

