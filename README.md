# Pando

[![Build Status](https://travis-ci.org/iliakolev/pando.svg?branch=master)](https://travis-ci.org/iliakolev/pando)

Easy to use bash scripts in order to provision a Vagrant server.

* This targets Ubuntu LTS releases, currently 14.04.

## Dependencies

* Vagrant `1.5.0`+
    * Use `vagrant -v` to check your version
* Vitualbox

## Instructions

**First**, Copy the Vagrantfile from this repo. You may wish to use curl or wget
to do this instead of cloning the repository.

**Second**, edit the `Vagrantfile` and uncomment which scripts you'd like to
run. You can uncomment them by removing the `#` character before the
`config.vm.provision` line.

> You can indeed have [multiple provisioning](http://docs.vagrantup.com/v2/provisioning/basic_usage.html) scripts when provisioning Vagrant.

**Third** and finally, run:

```bash
$ vagrant up
```

> <strong>Windows Users:</strong>
>
> By default, NFS won't work on Windows. I suggest deleting the NFS block so Vagrant defaults back to its default file sync behavior.
>
> However, you can also try the "vagrant-winnfsd" plugin. Just run `vagrant plugin install vagrant-winnfsd` to try it out!
>
> Vagrant version 1.5 will have [more file sharing options](https://www.vagrantup.com/blog/feature-preview-vagrant-1-5-rsync.html) to explore as well!

## What You Can Install

* Base Packages
    * Base Items (Git and more!)
    * Vim
* Web Servers
    * Nginx
* Additional Languages
    * NodeJS via NVM
    * Ruby via RVM

## The Vagrantfile

The vagrant file does three things you should take note of:

1. **Gives the virtual machine a static IP address of 192.168.22.10.** This
static IP allows us to use [xip.io](http://xip.io) for the virtual host setups
while avoiding having to edit our computers' `hosts` file.
2. **Uses NFS instead of the default file syncing.** NFS is reportedly faster
than the default syncing for large files. If, however, you experience issues
with the files actually syncing between your host and virtual machine, you can
change this to the default syncing by deleting the lines setting up NFS:

  ```ruby
  config.vm.synced_folder ".", "/vagrant",
            id: "core",
            :nfs => true,
            :mount_options => ['nolock,vers=3,udp,noatime']
  ```
3. **Offers an option to prevent the virtual machine from losing internet
connection when running on Ubuntu.** If your virtual machine can't access the
internet, you can solve this problem by uncommenting the two lines below:

  ```ruby
    #vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    #vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  ```

  Don't forget to reload your Vagrantfile running
  `vagrant reload --no-provision`, in case your virtual machine already exists.

## Credits

* **[Vaprobash](https://github.com/fideloper/Vaprobash)** Vagrant Provisioning
  Bash Scripts
