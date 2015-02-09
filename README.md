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

In this example I'll be using Digital Ocean SSD cloud hosting, but you can use whatever hosting you like. Let's setup __2 GB/2 CPUs__ [droplet](http://monosnap.com/image/oLidA02XDTRd5atmWg8pcXi8NjhdzD) with [node.js on Ubuntu 14.04 image](http://monosnap.com/image/ceBTWoVJd6P7nBmUkN23G2O40F9PT5). 

## Setting up `ssh` access 

Once the Droplet was succefully created we need to [reset root password](http://monosnap.com/image/ObqUKlSOoOBiWcNxuS5YNvegYtgNpc). Copy the password from the recovery email and pool up `iTerm` or `Terminal`.
To add a SSH key after the creation of the droplet, you need to add the contents of the public key to the file `~/.ssh/authorized_keys`. You can do that by running this on your local computer: 

    cat ~/.ssh/id_rsa.pub | ssh root@your.ip.address "cat >> ~/.ssh/authorized_keys"

After a few seconds you will be prompted for a password, type it in and BOOM, the next time ssh, you won't be prompted for a password! 

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

and let's also add some usefull plugins we might need later: 

    plugins=(git z last-working-dir history-substring-search history extract zsh-syntax-highlighting)

Than hit <kbd>Ctrl</kbd>+<kbd>O</kbd> and <kbd>Enter</kbd>. 

Restart your droplet for `zsh` to apply the changes: 

    sudo shutdown -r 0

## Installing and configuring `git`

Install git 

    apt-get install git 

Setup `git` global configuration. 

Create `~/.gitconfig` file and copy this into it:

    [user]
        name = Your Name
        email = your-mail@mail.com
    [core]
    excludesfile = .gitignore_global
    editor = nano
    [color]
        diff = auto
        status = auto
        branch = auto
        interactive = auto
        ui = auto
    [color "branch"]
        current = green bold
        local = green
        remote = red bold
    [color "sh"]
        branch = yellow
    [color "diff"]
        meta = blue bold
        frag = magenta bold
        old = red
        new = green
    [color "status"]
        added = green
        changed = blue bold
        untracked = red
    [alias]
        st = status -sb
        sb = status -sb
        aa = add -A
        br = branch
        ci = commit
        co = checkout
        df = diff
        ui = update-index
        unchange = update-index --assume-unchanged
        change = update-index --no-assume-unchanged
        dc = diff --cached
        last = log -1 HEAD
        lg = log --graph --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
        lg10 = log --graph --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -10
        lg20 = log --graph --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -20
        lgf = log --graph --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -40
        lr = log --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative --reverse
        lr10 = log --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -10 --reverse
        lr20 = log --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -10 --reverse
        tree = log --graph --decorate --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
        lgq = !git --no-pager log --pretty=format:'%C(magenta)%h%Creset -%C(red)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative -20
        today = diff --stat @{12am}..HEAD
        total = !git log --pretty=oneline | wc -l
        oh = !git push origin && git push heroku
        tl = tag -l -n100
        rehead = reset HEAD
        reci = reset --soft HEAD^
        back = !git reci && git rehead
        rehoh = reset --hard ORIG_HEAD
        # http://blog.blindgaenger.net/advanced_git_aliases.html
        ignore = !([ ! -e .gitignore ] && touch .gitignore) | echo $1 >>.gitignore
        r  = !git ls-files -z --deleted | xargs -0 git rm
        x  = commit -m
        xa = commit -a -m
        a  = add
    [github]
        user = YourUsename
    [filter "media"]
        required = true
        clean = git media clean %f
        smudge = git media smudge %f
    [fetch]
        prune = true

Create `~/.gitignore_global` file and copy this into it:

    # Compiled source 
    ####################
    *.com
    *.class
    *.dll
    *.exe
    *.o
    *.so
    # Packages #
    ############
    # it's better to unpack these files and commit the raw source
    # git has its own built in compression methods
    *.7z
    *.dmg
    *.gz
    *.iso
    *.jar
    *.rar
    *.tar
    *.zip
    # Logs and databases #
    ######################
    *.log
    *.sql
    *.sqlite
    # OS generated files #
    ######################
    .DS_Store
    .DS_Store?
    ._*
    .Spotlight-V100
    .Trashes
    ehthumbs.db
    Thumbs.db
    # Random VM Files/Folders
    bad-proxies.txt

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
