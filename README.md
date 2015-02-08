# Continuous integration 

This project is an example of Continuous Integration. 
It is [Jenkins CI](http://jenkins-ci.org/) running [Mocha](http://mochajs.org/) tests with cross-browser [SauceLabs](saucelabs.com) integrations on [Selenium RC](http://www.seleniumhq.org/projects/remote-control/) via [Webdriver.io](http://webdriver.io/) javascript bindings and [WebdriverCSS](https://github.com/webdriverio/webdrivercss) visual regression tests via [Applitools](applitools.com).

[CI diagram flow](http://goo.gl/qQlYYy)

# Workflow Overview

1. Developer pushes changed code base to `production` branch
2. Git commit triggers `jenkins` build script
3. When build script is successfully executed, `jenkins` runs Mocha tests.  
4. When all tests pass, than `jenkins` triggers production deployment.

# Project configuration

Project configuration consists of setting up Jenkins remote server, install dependencies it will need for re-creating production server environment and mocha-webdriverio-webdrivercss tests, setup ssh keys for remote accessing and git hooks for triggering Jenkins.  

* Setup ssh keys 
* Install `oh-my-zsh`
* Install `git` and `z` plugins for zsh
* Install `git`
* Install Node.js
* Install npm 
* Install SeleniumRC
* Install Webdriver.io
* Install WebdriverCSS
* Install dependencies for WebdriverCSS
* Install Jenkins
* Install plugins for Jenkins
* Configure Jenkins
* Setup git hooks

## Creating Web Server 

In this example I'll be using Digital Ocean SSD cloud hosting, but you can use whatever hosting you like. Let's setup __2 GB/2 CPUs__ [droplet](http://monosnap.com/image/oLidA02XDTRd5atmWg8pcXi8NjhdzD) with [node.js on Ubuntu 14.04 image](http://monosnap.com/image/ceBTWoVJd6P7nBmUkN23G2O40F9PT5). We can also add [ssh key](http://monosnap.com/image/Qo6yzQg9oK4FDIftpEMQHiciReZqWS) now, so we won't worry about it later. 

## Setting up `ssh` access 

Once the Droplet was succefully created we need to [reset root password](http://monosnap.com/image/ObqUKlSOoOBiWcNxuS5YNvegYtgNpc) on it. Now let's pool up `iTerm` or `Terminal` and login in our droplet. 

    ssh root@droplet.ip.number

After a few seconds you will be prompted for a password, type it in and we are in our droplet shell.

__Note:__ 

When a domain has been moved from one server to another an issue with SSH logins may occur. The warning dialog that most SSH programs give looks something like this:

    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that the RSA host key has just been changed.
    The fingerprint for the RSA key sent by the remote host is
    06:ea:f1:f8:db:75:5c:0c:af:15:d7:99:2d:ef:08:2a.
    Please contact your system administrator.
    Add correct host key in /home/user/.ssh/known_hosts to get rid of this message.
    Offending key in /home/user/.ssh/known_hosts:4
    RSA host key for domain.com has changed and you have requested strict checking.
    Host key verification failed.

The SSH program will print this message and often exit, prohibiting the user from connecting to the suspicious site. This problem arises when a site has changed servers, and the new server RSA key which is transmitted when authenticating is different from the old server.

## Installing and configuring `oh-my-zsh`

[Installing oh-my-zsh on Ubuntu](https://gist.github.com/tsabat/1498393)









# Glossary 

* [Selenium RC(Remote Control)](http://www.seleniumhq.org/projects/remote-control/) - is an API accessible from multiple languages. Rather than recording tests, you write programs that call into the API to control the browser
* [WebdriverIO](http://webdriver.io/) - Selenium 2.0 bindings for NodeJS
* [WebdriverCSS](https://github.com/webdriverio/webdrivercss) - is a plugin for an automatic visual regression for WebdriverIO. After initialization it enhances a WebdriverIO instance with an additional command called webdrivercss and enables the possibility to save screenshots of specific parts of your application
* [SauceLabs Integration](https://github.com/webdriverio/webdriverio/blob/master/examples/webdriverio.browserstack.js) - Automated Cross-Browser testing with wide collection of plattforms, devices and browser combinations.
* [Applitools Eyes](https://applitools.com/) - is a comprehensive automated UI validation solution with really smart image matching algorithms that are unique in this area. As a cloud service it makes your regression tests available everywhere and accessible to everyone in your team, and its automated maintenance features simplify baseline maintenance.
* [Jenkins CI](jenkins-ci.org) - is an open source continuous integration tool written in Java. It provides continuous integration services for software development. It is a server-based system running in a servlet container.
* [Mocha](http://mochajs.org/) - Mocha is a feature-rich JavaScript test framework running on node.js and the browser, making asynchronous testing simple and fun


# Articles

[Jenkins and Node](https://blog.dylants.com/2013/06/21/jenkins-and-node/)

[Trigger Jenkins on git push](http://stackoverflow.com/questions/5784329/how-can-i-make-jenkins-ci-with-git-trigger-on-pushes-to-master)

[Setting up continious integration deployment with jenkins](http://code.tutsplus.com/tutorials/setting-up-continuous-integration-continuous-deployment-with-jenkins--cms-21511)

[Video Tutorial on Jenkins](https://www.youtube.com/watch?v=4sANX9AhM8c)

[How to install and use Jenkins](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-jenkins-on-ubuntu-12-04)


# Resources

[Webdriver.io google group](https://groups.google.com/forum/#!forum/webdriverio)

[How to configure git commit hook](http://stackoverflow.com/questions/12794568/how-to-configure-git-post-commit-hook)
