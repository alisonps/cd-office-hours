WEEK 1
======

1. Download vagrant:
[https://dl.bintray.com/mitchellh/vagrant/vagrant_1.6.1.dmg](https://dl.bintray.com/mitchellh/vagrant/vagrant_1.6.1.dmg)

1. Download VirtualBox
[http://download.virtualbox.org/virtualbox/4.3.8/VirtualBox-4.3.8-92456-OSX.dmg](http://download.virtualbox.org/virtualbox/4.3.8/VirtualBox-4.3.8-92456-OSX.dmg)

     NB: this is specifically an older version as 4.3.10 has a bug.

1. Install Ansible via brew:

   ```
   brew update
   brew install ansible
   ```
1. Ensure your python is 2.7 or higher

   ```
   python --version
   Python 2.7.6
   ```
1. Start the vagrant instance for the Vagrantfile in this directory:

   ```
   vagrant up
   ```
    This will create a new VM with Ubuntu 12.04 (precise pangolin)

	If your default VM provider is set to something other than Virtualbox, you will need to explicitly specify the VM provider.

	```
	vagrant up --provider virtualbox
	```

1. Test SSH both ways
    1. ```vagrant ssh``` # then Ctrl-d to logout
    1. ```ssh vagrant@192.168.33.10``` # password is ```vagrant``` then logout

    This proves that we have set up a static IP address which Ansible will later require.

1. Test Ansible ping (depending on the version of Vagrant you're using, the following's going to look a little different)

  Prior to Vagrant 1.7, the default insecure private key is used for ssh. Test Ansible ping with:

   ```
   ansible -m ping AppServer -i inventory -vvv --user vagrant --private-key=~/.vagrant.d/insecure_private_key
   ```

   Beginning in Vagrant 1.7, Vagrant replaces the default insecure key with a more secure generated one. Test Ansible ping with:

   ```
   ansible -m ping AppServer -i inventory -vvv --user vagrant --private-key=.vagrant/machines/default/virtualbox/private_key
   ```

   Broken down those arguments are:

   ```
   ansible            # ansible binary for running modules directly
   -m ping            # The ansible module to run
   AppServer          # server set to on which to run this command
   -i ./inventory     # server definition file
   -vvv               # some verbosity (a lot, actually)
   --user vagrant     # tell ansible which user
   --private-key=~/.vagrant.d/insecure_private_key # use the vagrant SSH keys already created as part of vagrant up
   ```
   Response should end with:

   ```
   192.168.33.10 | success >> {
    "changed": false,
    "ping": "pong"
}
   ```

1. Destroy your vagrant VM to save space (we'll create another one in next weeks session):

   ```
   vagrant destroy -f
   ```
