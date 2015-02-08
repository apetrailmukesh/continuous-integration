# Continuous integration 

This project is an example of Continuous Integration. 
It is based on running Mocha tests with cross-browser SauceLabs integrations on Selenium Server with Webdriver.io javascript bindings, Webdrivercss visual regression tests and configured with Jenkins Continious Integration.

[CI diagram flow](http://goo.gl/qQlYYy)

# Workflow steps

1. Developer pushes changed code base to `development` branch
2. `Development` branch automatically deploys itself to the `testing server`, via git hooks.
3. Deployment to `testing server` triggers `jenkins`
4. If all test pass, than `jenkins` triggers production deployment.

# Setting up Jenkins Server 

* install node.js  

* Download selenium server

`curl -O http://selenium-release.storage.googleapis.com/2.43/selenium-server-standalone-2.43.1.jar`


* Install Java JDK

`http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html`


* Start selenium standalone server

`java -jar selenium-server-standalone-2.43.1.jar`


* Download Webdriver.io Javascript binding

`npm install webdriverio`

* Install WebdriverCSS dependencies

`brew install graphicsmagick cairo`


* Install WebdriverCSS `npm` module

`npm install webdrivercss`


# Articles

[Jenkins and Node](https://blog.dylants.com/2013/06/21/jenkins-and-node/)

[Trigger Jenkins on git push](http://stackoverflow.com/questions/5784329/how-can-i-make-jenkins-ci-with-git-trigger-on-pushes-to-master)

[Setting up continious integration deployment with jenkins](http://code.tutsplus.com/tutorials/setting-up-continuous-integration-continuous-deployment-with-jenkins--cms-21511)

[Video](https://www.youtube.com/watch?v=4sANX9AhM8c)


# Resources

[Webdriver.io google group](https://groups.google.com/forum/#!forum/webdriverio)

[How to configure git commit hook](http://stackoverflow.com/questions/12794568/how-to-configure-git-post-commit-hook)
