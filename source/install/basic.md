---
layout: default
currentMenu: install
---

# Installation

Question2Answer takes 5 minutes (or less!) to install. In most cases, installing Question2Answer for your website should be dead easy. Please follow the steps below.

## Before you install Question2Answer

Make sure you have:

- A web server which you can access via [FTP][1] or [SFTP][2] (or other deployment method). You might find [Cloudlook][3] helpful to evaluate cloud hosting providers.
- A [text editor][4].
- A web browser.

And make sure your web server is running:

- Web serving software such as [Apache][5] or [Nginx][6].
- [PHP][7] 5.1.6 or later.
- [MySQL][8] 5.0 or later.

If you are not sure about this, please check with your web hosting provider.

[1]: http://en.wikipedia.org/wiki/File_Transfer_Protocol
[2]: http://en.wikipedia.org/wiki/SSH_file_transfer_protocol
[3]: http://www.cloudlook.com/
[4]: http://en.wikipedia.org/wiki/Text_editor
[5]: http://httpd.apache.org/
[6]: http://wiki.nginx.org/Main
[7]: http://www.php.net/
[8]: http://www.mysql.com/



## Installing Question2Answer for the first time

The instructions below are for installing Question2Answer where it manages user accounts and logins for you. If you would like Question2Answer to integrate with your existing user database and account system, see the instructions for [single sign-on](http://www.question2answer.org/single-sign-on.php) installation. From version 1.4, Question2Answer also offers easy [integration with a WordPress 3.x](http://www.question2answer.org/wordpress.php) site and user database.

1. Download the [latest version of Question2Answer](http://www.question2answer.org/question2answer-latest.zip) to your computer or web server (also available [on GitHub](https://github.com/q2a/question2answer)).

2. Unzip the download using a tool such as [WinZip](http://www.winzip.com/) (or `unzip` in the Unix shell).

3. If you want to run a non-English site, check if the appropriate [language file](http://www.question2answer.org/addons.php) is available. If so, download and install it in the `qa-lang` folder. If not, it's simple to [translate](http://www.question2answer.org/translate.php) Question2Answer for yourself.

4. Create a MySQL database, and a MySQL user with full permissions for that database.

    <small>If you're interested, the [privileges](http://dev.mysql.com/doc/mysql/en/privilege-system.html) actually needed are: CREATE, ALTER, DELETE, INSERT, SELECT, UPDATE, LOCK TABLES</small>

5. Note down the MySQL details: username, password, database name and server host name. If MySQL is running on the same server as your website, the server host name is likely to be `127.0.0.1` or `localhost`.

6. Find `qa-config-example.php` in the unzipped `question2answer` folder, and rename it to `qa-config.php`.

7. Open `qa-config.php` in your text editor, insert the MySQL details at the top, and save the file. Do not use a word processor such as Microsoft Word for this, but rather Notepad or another appropriate [text editing program](http://en.wikipedia.org/wiki/List_of_text_editors).

8. Place all the Question2Answer files (**including .htaccess**) in the appropriate location on your web server:

    - To serve Question2Answer at the root of a domain (e.g. `http://www.mysite.com/`), move or upload all the contents of the unzipped `question2answer` folder into the root directory for that domain on your web server.

    - To serve Question2Answer in a subdirectory of a site (e.g. `http://www.mysite.com/qa/`), create the subdirectory inside the root directory for the site, then move or upload all the contents of the unzipped `question2answer` folder into this subdirectory.

9. Open the appropriate web page for Question2Answer in your web browser, for example:

    - If you installed Question2Answer at the root of a domain, `http://www.mysite.com/`

    - If you installed Question2Answer in a subdirectory, `http://www.mysite.com/qa/`

10. Follow the on-screen instructions to set up your database and administrator account. That's it!



## Upgrading to the latest version of Question2Answer

The instructions below are for upgrading from a previous version of Question2Answer. Click for a [full version history](http://www.question2answer.org/versions.php).

1. Before upgrading, it is recommended to make a backup of your MySQL database using `mysqldump` or a similar tool.

2. If you have modified any core Q2A files, or installed any extra themes, languages or plugins in the `qa-theme`, `qa-lang` or `qa-plugin` directories, make a copy of your whole Q2A directory, to help reapply your changes after upgrading.

3. Download the [latest version of Question2Answer](http://www.question2answer.org/question2answer-latest.zip) to your computer or web server (also available [on GitHub](https://github.com/q2a/question2answer)).

4. Unzip the download using a tool such as [WinZip](http://www.winzip.com/) (or `unzip` in the Unix shell).

5. Replace all the Question2Answer files and directories (**including .htaccess**) on your web server with the new ones.

    - Your custom `qa-config.php` file will not be affected, nor will your `qa-external` directory.

    - You will need to reapply any other changes and reinstall any themes, languages or plugins as appropriate.

6. Open the page for Question2Answer in your browser, and look around the admin center. If appropriate, you will be prompted to upgrade the database, so follow the on-screen instructions. You've now successfully upgraded - congratulations!



## Add-Ons

See the [add-ons](http://www.question2answer.org/addons.php) page for language files, themes and plugins created by Question2Answer users.
