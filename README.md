# Vagrant ColdFire Dev

This is a [librarian-chef](https://github.com/applicationsonline/librarian) managed [Vagrant](http://vagrantup.com) box for [ColdFire](https://github.com/nmische/ColdFire) development. To use you must supply the Git repo information (repository url, revision, SSH deployment key) for your copy of ColdFire. This assumed workflow is one where you have forked ColdFire to your own GitHub account where you will make changes (preferably on a feature branch) and will then submit pull requests back to the main project. For more on the ColdFire repo setup see the [coldfire-dev cookbook README](https://github.com/nmische/chef-coldfire-dev/blob/master/README.md).

Once you have your Git repo information squared away, to use this repo you would:

    git clone git@github.com:{your_user_name}/vagrant-coldfire-dev  coldfire
    cd coldfire
    librarian-chef install
    # Now go off and download the 32-bit Linux installer for ColdFusion 10
    # Once you have it, place it in:
    # cookbooks/coldfusion10/files/default
    vagrant up

Once Chef has done it's thing you should be able to browse to http://192.168.33.33/coldfire/tests/ to see the ColdFire test pages.

## Developing ColdFire ##

To install the ColdFire Firefox plugin for development first run an incremental build:

    vagrant ssh
    cd /vagrant/wwwroot/coldfire
    sudo ant incremental-all

This will install the latest coldfire debugging templates to the ColdFuison and Railo servers and build a copy of the Firebug extension in the `wwwroot/incremental/firefox/extensions` directory of the Vagrant project.

Next configure Firefox to use this development version of the ColdFire extension[1]:

* Find your [Firefox profile folder](http://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data). NOTE: You may want to create a new profile for development purposes.
* Find or create the extensions sub-folder.
* In the extension subfolder create a file name `coldfire@riaforge.org`
* Populate the file with the path to the incremental extension. Assuming you cloned this repo to `/Projects/ColdFireDev` the path would be `/Projects/ColdFireDev/wwwroot/coldfire/incremental/firefox/extensions/coldfire@riaforge.org`
* Restart Firefox.

## Building A Release ##

To run a full build of the project do the following:

    vagrant ssh
    cd /vagrant/wwwroot/coldfire
    sudo ant


[1] For more on development setup for Firebug see [the Firebug wiki](http://getfirebug.com/wiki/index.php/Firebug_Internals#Development_Setup).



