1) Create droplet 
2) SSH into ip.droplet
3) copy-paste password from `email`
4) copy-paste password from `email` again
5) create new `unix` password
6) repeat your new `unix` password 
7) install `zsh` 

    apt-get update && apt-get install zsh && apt-get install git-core &&  wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh && chsh -s `which zsh` && sudo shutdown -r 0 

8) install `nano`

    apt-get install nano

9) config `~/.zshrc`

    # change these lines
    # theme name you prefer 
    ZSH_THEME="fino"

    # plugins you prefer 
    plugins=(git z last-working-dir history-substring-search history extract zsh-syntax-highlighting)

10) install `git`

    apt-get install git 

11) config `git`

    nano ~/.gitconfig 
    # add in file
    name = Jenkins
    email = jenkins@referralsolutionsgroup.com
    editor = nano

12) Create new `linux` user

    adduser jenkins

13) Add user to `sudoers` group

    usermod -a -G sudo jenkins

14) Create password for new user 

15) Install `Apache`

    apt-get install apache2

16) Update the system 

    apt-get update && apt-get upgrade

17) Install `java` development kit 

    apt-get install openjdk-7-jdk

18) Add `jenkins` key and source list to apt

    wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -

19) Create a sources list for `jenkins`

    sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'

20) Update cache before installing `jenkins`

    apt-get update

21) Install `jenkins`

    apt-get install jenkins

22) 