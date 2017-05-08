I have recently been expanding my knowledge. PHP has been popular choice for webpages for many years now. Probably it's one of the oldest and most mature programming tools I've had to use in a while. PHP webpages are well known for the LAMP stack (Linux, Apache, MySQL, and PHP). For my project, however, I am interested in setting up a WIMP stack (Windows, IIS, MySQL, and PHP... Yes it's an unfortunate accronym...)

Sure enough, when LAMP is the status quo, then going against the grain with WIMP can sometimes feel like exploring undocumented territory. In particular I was creating a WordPress Multi-Site setup and found a small number of pitfalls and gotchas that I'd love to share with you now. We'll see the steps for setting up a WordPress in WIMP (on a local dev environment) and we'll pay special attention to the pot holes and speed bumps. Buckle up!

I am setting up this environment on Windows 10 therefore you might have better (or worse) success with a different Windows environment. Please let me know in the comments.

First step is to setup IIS. Now I assume you would already have IIS setup on your machine, but if not then checkout this [MSDN documentation](https://msdn.microsoft.com/en-us/library/ms181052(v=vs.80).aspx). What we need to add is the ability to create PHP App Pools in IIS. Download the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) and run it.

Click on the `Products` tab and click the `Add` button for the following:
    - PHP 7.1.1 (x64)
	- MySQL Windows 5.1
	
Pitfall #1: I specifically recommend MySQL 5.1 over 5.5. I was receiving error messages that 5.5 would not install successfully.

After you select those two products, press the `Install` button.
The installer will prompt you for a password for the _root_ DB user. Use whatever you're comfortable with, but don't use a weak password if this environment will be accessible from other machines.

Pitfall #2: Accept the resulting dialogs and you will see an "Installation Complete" message, however it's very likely there will be an error message like so:

    > We're sorry. The following products have failed to install.
	> PHP Manager for IIS

The good news is, I haven't found it to matter the least bit. So just ignore that error and let's move on...

Now you will have your MySQL daemon running. You need to manually create your WordPress database before you can do anything with the application so let's take an aside to set that up now. Open your PowerShell (or Command Prompt if you are not cool yet...) The proper way to run the `mysql` cli tool is with the following parameters:

```
> mysql -u root -p
```

This will prompt you for your password and then you can begin executing the commands. We only need to use one command here (Note the semi-colon is required):

```
mysql> create database wordpress;
```

This obviously will initialize a new database. The command to exit MySQL CLI is `\q`.
If you are so inclined, now would be a good time to create a new user. This is how:

```
mysql> GRANT ALL PRIVILEGES ON wordpress.* To 'user'@'hostname' IDENTIFIED BY
 'password';
```


Now you are ready to download the WordPress source code. [Click here](https://wordpress.org/download/) to get the latest download link. Extract the archive to a folder you want as the root for your website.

Now we are ready to create the new site in IIS. Open IIS and first thing is to add a new Application Pool. Right Click `Application Pools` and click `Add Application Pool`. Give it a cool name like "WordPress" and select "No Managed Code" for the *.NET CLR version*. A PHP is not going to user any managed .NET code. Press `OK` to close the dialog.

Next, we can create the new site. Right click on `Sites` and click `Add Website...`. I like to call my "Site name" the same as the "Application Pool". Click the elipsis beside the "Physical path" field and choose the directory you extracted the WordPress source to. I recommend in a shared dev environment to use a "Specific User" that is shared between devs, under `Connect as...` dialog, but if this is for your eyes only, then don't bother.

Pitfall #3: If you are planning on doing a WordPress Multi-Site setup like I was, then you actually are required to use port 80. You can use an Application of an existing site, but then WordPress will only allow you to do sub-directory domains. If you plan on giving each of your children sites a different domain, then you need to create the new site, with port 80, just like I have described.

There is one last thing to do in IIS here. With the "WordPress" site selected, double-click the `Default Document` configurator. Some installations of IIS will only have one or two files here, such as "default.aspx" or the like. What we want is `index.php` so add it if it doesn't already exist. Pitfall #4: If it does exist, you'll see an alert:

> "The file 'index.php' exists in the current directory. It is recommended that you move this file to the top of the list to imporove performance."

Here's a better idea! Remove all the default documents other than index.php. You won't need them!


When your new IIS site is running, you can go to the fun part! Browse to `http://localhost/` in your browser and begin the "Famous five-minute installation".

When it asks for your database connection details, you should know the drill. The first MySQL command above made a new database called "wordpress" so that is the *Database Name* you use now. The *Username* you supply is "root". To be fair, the Web Platform Installer never actually asked you to choose that, so it's ok if you forgot. The *Password* was whatever you used to login to the MySQL daemon earlier. The *Database Host* should be localhost in this case. Only change this if the DB is on a different machine from the webserver. Note, if you were actually setting up a non-localhost environment, you still use "localhost" in this field. It's the host that WordPress will use to find the DB, so it's relative to the server, not the client, capiche? Last but not least it is recommended to change the *Table Prefix* for security reasons. Even though this is your localhost environment, you might want to update this part now so that it's one less step if you might have to migrate the DB in the future. Sensible values would be the acronym of your product name or company. Or the top secret codename for your project. It's ok, I won't tell.

Submit the form and hopefully you see a page that has a `Run the install` button. If you see something else, follow the directions and go back and fix anything that may be amiss.

Now you should find yourself to the welcome page. This form is self-explanatory. Just create a title, username, password, and put your email. The *Search Engine Visibility* field won't have an effect on a non-production environment, but feel free to set it now with the confident that you can change the value through the CMS at anytime later. Submit the form and you will see the "Success!" page.

Pitfall #4: I probably should have mentioned this earlier, but if you were planning on using an alias other than `localhost` you should do it before the installation process. WordPress will take the domain name and consider that as the only valid domain that can login afterwords. Changing it after the fact can be a bit of a pain. WordPress also doesn't give any kind of error message when you are attempting to login with the wrong alias as the domain. Yes, even if they are resolving to the same IP it won't work. It just redirects you back to the Login screen.

Hopefully you are logged in now. This is the CMS, if you haven't seen it before. However, it is not yet the Multi-site CMS. We still have a few more configurations to take care of, but we're almost done.

Go to the web root folder of your WordPress installation and open the file called `wp-config.php`. Scroll down enough and you will see a block comment like so:

```
/* That's all, stop editing! Happy blogging. */
```

Immediately above this line, put the following line of code and save the file:

```
define( 'WP_ALLOW_MULTISITE', true );
```

With that in place, you can refresh the CMS page and there will be a new feature... Can you find it? Just kidding it's hiding pretty good so I'll tell you. Click on `Tools` > `Network Setup` to see the new config page we just enabled. If you like the *Network Title* then go ahead and press the `Install` button. The page will reload and you will see a couple of code snippets. This is were it gets interesting...

First, do the first step that it tells you. Copy the PHP lines of code and paste it again into the `wp-config.php` below the last edit you made.

Pitfall #5: The weird part is that the second step is inadequate for general cases. What you should see in step 2 is the contents of a `web.config` file. There unfortunately is a bug in the rewrite rules preventing users from logging in properly, on all environments including localhost, under the WordPress and IIS configuration that I've told you. Never fear, because I have the preferred `web.config` right here for you to use:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="WordPress Rule 1" stopProcessing="true">
                    <match url="^index\.php$" ignoreCase="false" />
                    <action type="None" />
                </rule>
                <rule name="WordPress Rule 2" stopProcessing="true">
                    <match url="^([_0-9a-zA-Z-]+/)?files/(.+)" ignoreCase="false" />
                    <action type="Rewrite" url="wp-includes/ms-files.php?file={R:2}" appendQueryString="false" />
                </rule>
                <rule name="WordPress Rule 3" stopProcessing="true">
                    <match url="^([_0-9a-zA-Z-]+/)?wp-admin$" ignoreCase="false" />
                    <action type="Redirect" url="{R:1}wp-admin/" redirectType="Permanent" />
                </rule>
                <rule name="WordPress Rule 4" stopProcessing="true">
                    <match url="^" ignoreCase="false" />
                    <conditions logicalGrouping="MatchAny">
                        <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" />
                        <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" />
                    </conditions>
                    <action type="None" />
                </rule>
                <rule name="WordPress Rule 5" stopProcessing="true">
                    <match url="^([_0-9a-zA-Z-]+/)?(wp-(content|admin|includes).*)" ignoreCase="false" />
                    <action type="Rewrite" url="{R:2}" />
                </rule>
                <rule name="WordPress Rule 6" stopProcessing="true">
                    <match url="^([_0-9a-zA-Z-]+/)?(.*\.php)$" ignoreCase="false" />
                    <action type="Rewrite" url="{R:2}" />
                </rule>
                <rule name="WordPress Rule 7" stopProcessing="true">
                    <match url="." ignoreCase="false" />
                    <action type="Rewrite" url="index.php" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
```

Go ahead and put the `web.config` in your root directory, beside the `wp-config.php` file. Note, I  recommend to use this improved `web.config` file for all IIS installations of WordPress. The original config tries to make a best guess at what kind of environment and setup you are running and to be fair it works most of the time. However the one I linked to above is superior. Shout out to user [misaxi](https://gist.github.com/misaxi/67cb1444a7dccf11f7fa) for posting this `web.config` on his GitHub Gist.

Finally, the only remaining thing to do is log out of the CMS and then log back in. Enjoy your WordPress Multi-site and "Happy Blogging!" If you have any questions, feel free to leave a message in the comments below.


