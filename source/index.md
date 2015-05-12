---
layout: default
currentMenu: home
---

# Question2Answer documentation

See the menu to the left to get started!



Test code block:

```language-php
define('QA_VERSION', '1.7.0-beta-2'); // also used as suffix for .js and .css requests
define('QA_BUILD_DATE', '2014-12-06');


/**
 * Autoloads some Q2A classes so it's possible to use them without adding a require_once first. From version 1.7 onwards.
 * These loosely follow PHP-FIG's PSR-0 standard where faux namespaces are separated by underscores. This is being done
 * slowly and carefully to maintain backwards compatibility, and does not apply to plugins, themes, nor most of the core
 * for that matter.
 *
 * Classes are stored in the qa-include/Q2A folder, and then in subfolders depending on their categorization.
 * Class names should be of the form Q2A_Namespace_Class, e.g. Q2A_Util_Debug. There may be multiple "namespaces".
 * Classes are mapped to PHP files with the underscores converted to directory separators. The Q2A_Util_Debug class is in
 * the file qa-include/Q2A/Util/Debug.php. A class named Q2A_Db_User_Messages would be in a file qa-include/Q2A/Db/User/Messages.php.
 */
function qa_autoload($class)
{
	if (strpos($class, 'Q2A_') === 0)
		require QA_INCLUDE_DIR.strtr($class, '_', '/') . '.php';
}
spl_autoload_register('qa_autoload');
```

Test HTML block

```language-markup
<!DOCTYPE html>
<html>
	<!-- Powered by Question2Answer - http://www.question2answer.org/ -->
	<head>
		<meta charset="utf-8">
		<title>Recent questions - Question2Answer</title>
		<meta name="viewport" content="width=device-width, initial-scale=1">
		<link rel="stylesheet" href="./qa-theme/SnowFlat/qa-styles.css?1.7.0-beta-2">
		<script>
		var qa_root = '.\/';
		var qa_request = 'questions';
		</script>
		<script src="./qa-content/jquery-1.11.1.min.js"></script>
		<script src="./qa-content/qa-page.js?1.7.0-beta-2"></script>
		<script src="./qa-theme/SnowFlat/js/snow-core.js?1.7.0-beta-2"></script>
	</head>

	<body class="qa-template-questions qa-body-js-off">
		<div class="qa-body-wrapper">
			<div class="qa-main-wrapper">
				<div class="qa-main">
					<h1>Recent questions</h1>
				</div>
			</div>
		</div>
	</body>
</html>
```
