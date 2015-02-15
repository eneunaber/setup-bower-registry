# setup-bower-registry
Instructions and files needed to setup a private bower repo

## Prerequisite
A linux box of some flavor. For our setup we are using a CentOS 6.5 headless machine. A key requirement is to have the epel repository available. If not already installed see the references section below, the documentation about installing redis has details on how to add the epel repo.

## Setup 
### Install Packages
```
sudo yum install nodejs
sudo yum install npm
sudo yum install redis
sudo service redis start
sudo npm install bower**
```

*Note: For CentOS 6.5, Node.js and npm are separate packages, but in other distros you get both when you install Node.js
**Bower was installed as a convenience to add packages from the server. However, it appears that most packages will be remotely added.

### Add Service User 
```
sudo adduser bower-registry
sudo passwd -l bower-registry
```

### Service Scripts
From this repo, rename and move bower-registry.sh to init.d folder as bower-registry

```
cd /etc/init.d
sudo chmod 755 bower-registry
sudo chkconfig --add bower-registry
sudo service bower-registry start
```

## Library Setup 
Each library that needs to be added to the bower server needs to follow two basic steps. 

* Add a bower.json file to your repository
* Register your product
 
### Add bower.json to the product
Assuming bower is installed, the simplest thing to do is run the bower init command and follow the prompts. Be sure to not mark the repo as private.

### Register
Add a .bowerrc file to the root of your application. A sample .bowerrc is provided in the repo. Take publisher.bowerrc and rename and move to the root of your repo that you are trying to publish as .bowerrc. Change localhost to the correct address of your local bower registry.

You then need to register the package with the bower server.
```
bower register JSSimpleSlideShow git://github.com/eneunaber/JSSimpleSlideShow.git
```

### Consumer Setup 
Each project that needs to consume the assets from the private bower repo will need to add a .bowerrc file to their project. Take consumer.bowerrc and rename and move to the root of your new project as .bowerrc. Change localhost to the correct address of your local bower registry.

This will allow users to use the main bower repo but also the private repo. The private repo will be checked initially, followed by the bower’s main public repo.

## References
* http://fumblesandfriends.com/blog/setting-up-a-private-bower-registry/
* http://sharadchhetri.com/2014/10/04/install-redis-server-centos-7-rhel-7/
* http://bower.io/docs/creating-packages/
* http://bower.io/docs/api/#register
* http://bower.io/docs/config/

