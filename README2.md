1) Create droplet 
2) SSH into ip.droplet
3) copy-paste password from `email`
4) copy-paste password from `email` again
5) create new unix password
6) repeat your new unix password 
7) install zsh 

    apt-get update && apt-get install zsh && apt-get install git-core &&  wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh && chsh -s `which zsh` && sudo shutdown -r 0 

8) install `nano`

    apt-get install nano

9) install `git`

    apt-get install git 

10) config `git`

    nano ~/.gitconfig 
    # add in file
    name = Jenkins
    email = jenkins@referralsolutionsgroup.com
    editor = nano

11) 
        