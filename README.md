![Build Status](https://travis-ci.org/thinkshout/crm-training.svg)

ThinkShout's virtual environment for CRM Trainings based on [Drupal VM](http://www.drupalvm.com/), a VM for local Drupal development, built with Vagrant + Ansible. It should take 5-10 minutes to build or rebuild the VM from scratch on a decent broadband connection.

## Installation

This guide will help you quickly build your Drupal site with all of the CRM modules installed and ready to go.

### 1 - Install dependencies (VirtualBox and Vagrant)

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Drupal VM also works with Parallels or VMware, if you have the [Vagrant VMware integration plugin](http://www.vagrantup.com/vmware)). If you are using Homebrew and cask, thank simply execute `brew cask install virtualbox`.
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html). If are you using Homebrew, `brew cask install vagrant`.
  3. For Vagrant to automatically configure your hosts file, install the `hostsupdater` plugin (`vagrant plugin install vagrant-hostsupdater`). All hosts defined in `apache_vhosts` or `nginx_hosts` will be automatically managed.
  4. For Vagrant to automatically assign an available IP address to your VM, install the `auto_network` plugin (`vagrant plugin install vagrant-auto_network`), and set `vagrant_ip` to `0.0.0.0` inside `config.yml`.
  5. Ensure that GitHub.com and drupal.org are added to your `known_hosts` file. The setup process will copy over your host OS known hosts to the guest OS in order to avoid the interactive prompt when doing a git checkout that will hang the provisioning process.

Note for Faster Provisioning (Mac/Linux only): *[Install Ansible](http://docs.ansible.com/intro_installation.html) on your host machine, so Drupal VM can run the provisioning steps locally instead of inside the VM.* For Homebrewers, `brew install ansible`.

Note for Linux users: *If NFS is not already installed on your host, you will need to install it to use the default NFS synced folder configuration. See guides for [Debian/Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-14-04), [Arch](https://wiki.archlinux.org/index.php/NFS#Installation), and [RHEL/CentOS](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-centos-6).*

Note on versions: *Please make sure you're running the latest stable version of Vagrant, VirtualBox, and Ansible, as the current version of Drupal VM is tested with the latest releases. As of August 2015: Vagrant 1.7.4, VirtualBox 5.0.2, and Ansible 1.9.2.*

### 2 - Build the Virtual Machine

  1. Download this project and put it wherever you want.
  3. By default, your local files will be located at `~/Sites/drupal-crm`. If you want to use a different location, create the local directory and configure the path to that directory in `config.yml` (`local_path`, inside `vagrant_synced_folders`).
  4. Open Terminal, cd to this directory (containing the `Vagrantfile` and this README file).
  5. **IMPORTANT:** If you have Ansible installed on your host machine: Run `$ sudo ansible-galaxy install -r provisioning/requirements.yml --force` prior to step 5 (`vagrant up`), otherwise Ansible will complain about missing roles.
  5. Type in `vagrant up`, and let Vagrant do its magic.

Note: *If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, post an issue to this project's issue queue on GitHub with the error.*

### 3 - Access your VM

  1. Open your browser and access [https://drupal-crm.dev/](https://drupal-crm.dev/). The default login for the admin account is `admin` for both the username and password.

## Other Notes

  - To shut down the virtual machine, enter `vagrant halt` in the Terminal in the same folder that has the `Vagrantfile`. To destroy it completely (if you want to save a little disk space, or want to rebuild it from scratch with `vagrant up` again), type in `vagrant destroy`.
  - When you rebuild the VM (e.g. `vagrant destroy` and then another `vagrant up`), make sure you clear out the contents of the `drupal` folder on your host machine, or Drupal will return some errors when the VM is rebuilt (it won't reinstall Drupal cleanly).
  - You can change the installed version of Drupal or drush, or any other configuration options, by editing the variables within `config.yml`.
  - Access your virtual machine and manage your site direclty by executing `vagrant ssh` from the project root.
  - You can also use Drush aliases to directly interact with your site. Aliases are automatically configured. So just using `drush @drupalvm.drupalvm.dev [command]`.
