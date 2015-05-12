# Question2Answer documentation

This repo contains documentation for [Question2Answer][Q2A], the free open source Q&A platform for PHP/MySQL.

The docs are written using Markdown files and compiled into a website using the static site generator, [Sculpin].

## Installing

If you want to contribute, you should set up the docs locally so you can edit the files and preview them. Here's how:

1. Fork this repo on Github.

2. Clone your copy of the repo to your system.

3. Install Sculpin. On Linux/Mac systems this can be done easily via the command line:

		curl -O https://download.sculpin.io/sculpin.phar
		chmod +x sculpin.phar
		mv sculpin.phar ~/bin/sculpin

4. Navigate to your local repo and run the command:

		sculpin generate --watch --server

5. Hit up `http://localhost:8000/` in your browser to view the documentation. Changes you make will be automatically recompiled back into the local site.

For a production site, use this command:

    sculpin generate --env=prod


## Editing

The documentation itself is comprised of Markdown files - plaintext files with simple additional markup like `**text**` for bold text, `# Title` for headings and so on.

The Markdown files are located in the `source` directory, organised in subfolders. The folders `_static` and `_layouts` contain CSS/JS and Twig templates respectively.

### Front matter

Each Markdown file begins with something called "front matter" - a small block of configuration. For example:

	---
	layout: default
	currentMenu: home
	---

This sets the template to use (default) and states which menu item should be highlighted as the current one.

### Code blocks

Code examples should be surrounded by "fenced code blocks" - three backticks on the line before and line after the code. The language can also be specified to enable syntax highlighting:

	```language-php
	<?php
	// some PHP code
	qa_opt('site_title');
	```



[Q2A]: http://www.question2answer.org/
[Sculpin]: https://sculpin.io/
