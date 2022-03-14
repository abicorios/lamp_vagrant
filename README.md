# lamp_vagrant

Lamp server (Linux Ubuntu 20.04, Apache2, MySQL 5.7, PHP 7.3).

It is insecure, for the local development only.

# Warning

It is infinitely slow on Windows 10 at the PhpMyAdmin import. But on the Ubuntu host it is good. So you need another LAMP on the Windows. Maybe I will try to fix it later. The simplest solution is the Docker https://github.com/sprintcube/docker-compose-lamp

# Requirements

VirtualBox [https://www.virtualbox.org/](https://www.virtualbox.org/)

Vagrant [https://www.vagrantup.com/](https://www.vagrantup.com/)

# Web development requirements
Also the web-development can be comfortable with Git [https://git-scm.com/](https://git-scm.com/) and VS Code [https://code.visualstudio.com/](https://code.visualstudio.com/)

# Usage

Clone this LAMP by the Git:

```bash
git clone git@github.com:abicorios/lamp_vagrant.git
```

or load the archive [https://github.com/abicorios/lamp_vagrant/archive/refs/heads/main.zip](https://github.com/abicorios/lamp_vagrant/archive/refs/heads/main.zip) and unzip it.

Copy a zip archive of the web-project to the directory of the LAMP project (let it is `project.zip`).

Then go to the directory of this LAMP project by the command line, and run:

```bash
vagrant up
```

After up of the virtual machine, you can go inside by the ssh:

```bash
vagrant ssh
```

Then become the root user:

```bash
sudo -i
```

Go to the main web directory, copy a project archive to the main web directory, and extract an archive:

```bash
cd /var/www/html
cp /vagrant/project.zip .
unzip project.zip
```

See a result:

```bash
ls
```

If you obtain the `project` directory (`/var/www/html/project`), but you need project files just inside the `/var/www/html` directory, you can simle move it:

```bash
cd /var/www/html
rsync -a project/ ./
rm -rf project
rm project.zip
```

Finally set the owner of the files to the dummy web user:

```bash
chown -R www-data:www-data /var/www/html
```

The website is accessible by the [http://localhost:8080](http://localhost:8080), and the PhpMyAdmin is accessible by the [http://localhost:8080/phpmyadmin](http://localhost:8080/phpmyadmin). MySQL admin user is `root` with the password `123456` (it is not secure, but it is not important, because it is accessible only locally).

You can disable the virtual machine by the `vagrant halt`, and next time you can enable it by the `vagrant up`. You can destroy the virtual machine by the `vagrant destroy`.


# Web development
At the Windows the git-bash tool can be useful, it appears by an installation of the git. At the Linux the usual bash terminal is good.

Go to the LAMP project directory by the command line on the host system (outside the virtual machine). Then you can add the ssh settings of the virtual machine access to the ssh config:

```bash
vagrant ssh-config | sed 's/User vagrant/User root/; s/Host default/Host lamp/; $i\ \ ForwardAgent yes' >> ~/.ssh/config
```

Open the VS Code, install the `Remote - SSH` extension. Connect to the LAMP machine by this extension (left-down green button). If it is not so clear, read the expanded instruction [https://code.visualstudio.com/docs/remote/ssh-tutorial](https://code.visualstudio.com/docs/remote/ssh-tutorial), some begin part about the connection to the host.

So, open the main web directory `/var/www/html` by the VS Code inside the virtual machine.

You can add the new archive from the host system by the mouse moving.

You can open the terminal inside the VS Code connected to the virtual machine by the main menu of the VS Code.

You can extract and move files like on the above `Usage` description by the terminal.

If some access errors, you can fix owner:

```bash
chown -R www-data:www-data /var/www/html
```

Also, it can be need to fix owner on new files. May be I will fix this more globally by the new ftp-user like on a hosting in next updates.

In short, names of VS Code extension for the web development which are comfortable for me: `GitLens`, `PHP Intelephense`, `PHP Debug` (it is good with the `Xdebug helper` browser extension). If you want use Xdebug without the browser extension, you can add the line `xdebug.start_with_request = yes` to the config file `/etc/php/7.3/mods-available/xdebug.ini`, and restart the Apache by the `service apache2 restart` command, but it will start Xdebug by the background scripts too (for example cron tasks), sometime it is not so comfortable.

To work with Github, it needs to set the ssh key by the instructions [https://docs.github.com/en/authentication/connecting-to-github-with-ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh). You can check by the command `ssh-add -L` key in the host and the guest system. If you set a key on the host system, and enable it on the Github settings, so when you ssh to the guest system with the forward agent option, then a key is accessible in the guest system. The agent forwarding is enabled in the `~/.ssh/config` as it is described above. On the Ubuntu host the ssh agent is enabled by default. In some case you need search how to do the autostart of the ssh agent. For example, on the Windows host it is possible by the instruction [https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse). But on the Windows host it is stack on the PhpMyAdmin import, as it warned above. The simplest solution is the Docker https://github.com/sprintcube/docker-compose-lamp
