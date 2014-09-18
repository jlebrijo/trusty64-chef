## Vagrant image creation and share through Vagrantcloud.com

```
mkdir trusty64-chef
cd trusty64-chef/
rbenv local 2.1.2
```

Create Gemfile:

```
source 'https://rubygems.org'
gem 'vagrant', github: 'mitchellh/vagrant', tag: 'v1.6.4'
gem 'knife-solo'
```

Install gems:

```
bundle
bundle install --binstubs .bundle/bin
```

Add to Vagrantfile:

```
config.vm.network :public_network, bridge: "wlan0", ip: "192.168.10.25"
```

Start box: `vagrant up`

Install Chef:

```
knife solo prepare vagrant@192.168.10.25    ## password: vagrant
```

Update Ubuntu:

```
vagrant ssh
sudo apt-get update
```


Package: `vagrant package --base trusty64-chef_default_1409751631640_92694 --output trusty64-chef.box`

Add to local Box list: `vagrant box add lebrijo/trusty64-chef package.box`

Upload the box to a HTTP server: `scp trusty64-chef.box lebrijoc@files.lebrijo.com:www/lebrijo.com/files`

Create an entry in vagrantcloud.com [lebrijo/trusty64-chef](https://vagrantcloud.com/lebrijo/trusty64-chef)