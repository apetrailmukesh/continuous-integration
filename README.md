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

## Creating Web Server 

In this example I'll be using Digital Ocean SSD cloud hosting, but you can use whatever hosting you like. Let's setup __2 GB/2 CPUs__ [droplet](http://monosnap.com/image/oLidA02XDTRd5atmWg8pcXi8NjhdzD) with [node.js on Ubuntu 14.04 image](http://monosnap.com/image/ceBTWoVJd6P7nBmUkN23G2O40F9PT5). We can also add [ssh key](http://monosnap.com/image/Qo6yzQg9oK4FDIftpEMQHiciReZqWS) now, so we won't worry about it later. 

## Setting up `ssh` access 

Once the Droplet was succefully created we need to [reset root password](http://monosnap.com/image/ObqUKlSOoOBiWcNxuS5YNvegYtgNpc) on it. Now let's pool up `iTerm` or `Terminal` and login in our droplet. 

    ssh root@droplet.ip.number

After a few seconds you will be prompted for a password, type it in and you are in Jenkins Server Shell.

Now we want to restrict root login. 

    nano /etc/ssh/sshd_config

we need to find the line that looks like this:

    PermitRootLogin yes

Here, we have the option to disable root login through SSH. This is generally a more secure setting since we can now access our server through our normal user account and escalate privileges when necessary.

    PermitRootLogin no

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


### Installing `zsh`

    // Update package manager 
    apt-get update 
    // Install pre requirements
    apt-get install zsh && apt-get install git-core
    // Install zsh
    wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh
    // Change shell to zsh
    chsh -s `which zsh`
    // Restart droplet 
    sudo shutdown -r 0

Or a nice one-liner:

    apt-get update && apt-get install zsh && apt-get install git-core &&  wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh && chsh -s `which zsh` && sudo shutdown -r 0

Source: [Installing `oh-my-zsh` on Ubuntu](https://gist.github.com/tsabat/1498393)


### Configuring `oh-my-zsh`

`oh-my-zsh` has a great variety of awesome themes, you can choose the one you like here - [http://zshthem.es/all/](http://zshthem.es/all/). I prefer `fino-time`, because it has useful output with a server name you are in, so you won't get lost in your ssh tonnels. 

Install `nano` text editor or any other, because we are going to edit `~.zshrc`

    apt-get install nano 

Open `.zshrc` file:

    nano ~/.zshrc

And change `ZSH_THEME` on the one you like: 

    ZSH_THEME="fino-time"

Than hit <kbd>Ctrl</kbd>+<kbd>O</kbd> and <kbd>Enter</kbd>. 

Restart your droplet for `zsh` to apply the changes: 

    sudo shutdown -r 0



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
