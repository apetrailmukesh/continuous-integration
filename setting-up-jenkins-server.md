#### 1) Create droplet 

#### 2) SSH into ip.droplet

#### 3) copy-paste password from `email`

#### 4) copy-paste password from `email` again

#### 5) create new `unix` password

#### 6) repeat your new `unix` password 

#### 7) install `zsh` 

    apt-get update && apt-get install zsh && apt-get install git-core &&  wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh && chsh -s `which zsh` && sudo shutdown -r 0 

#### 8) install `nano`

    apt-get install nano

#### 9) config `~/.zshrc`

    # change these lines
    # theme name you prefer 
    ZSH_THEME="fino"

    # plugins you prefer 
    plugins=(git z last-working-dir history-substring-search history extract zsh-syntax-highlighting)

#### 10) install `git`

    apt-get install git 

#### 11) config `git`

    nano ~/.gitconfig 
    # add in file
    name = Your Name
    email = your@mail.com
    editor = nano

#### 12) Create new `linux` user

    adduser jenkins

#### 13) Add user to `sudoers` group

    usermod -a -G sudo jenkins

#### 14) Create password for new user 

#### 15) Install `Apache`

    apt-get install apache2

#### 16) Update the system 

    apt-get update && apt-get upgrade

#### 17) Install `java` development kit 

    apt-get install openjdk-7-jdk

#### 18) Add `jenkins` key and source list to apt

    wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -

#### 19) Create a sources list for `jenkins`

    sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'

#### 20) Update cache before installing `jenkins`

    apt-get update

#### 21) Install `jenkins`

    apt-get install jenkins

#### 22) Go to `~/.zshrc`

    cd ~/.zshrc

#### 23) Generate new `ssh` keys

    ssh-keygen -t rsa -C "your@mail.com"

#### 24) Add your `ssh` public key to your [springloops.io](sringloops.com) account

    cat id_rsa.pub

#### 25) Add springloops to `~/.ssh/known_hosts`. 
    There is probably a better way to do it ...

    git clone ssh://sls@slsapp.com:1234/name/project.git

#### 26) Browse to droplet.ip:8080

#### 27) Install `jenkins` plugins

Go to __Manage Jenkins__ => __Manage Plugin__ => __Avaliable__

Check `git-client-plugin`, `git-plugin` and __install without restart__

#### 28) Update `jenkins` plugins 

Go to __Manage Jenkins__ => __Manage Plugin__ => __Updates__

Check all avaliable plugins and __download and install after restart__

#### 29) Restart `jenkins`

    sudo service jenkins restart 

#### 30) Setup credentials

Go to __Manage Jenkins__ => __Configure Global Security__

Check __Enable security__

Check __Matrix-based security__

Check __Overall__ => __Read Only__ for _Anonymous_

Add _User_

Check all marks for _New user_



31)




