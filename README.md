# Spicy MoodleBox



Spicy Moodlebox is a port of Nicolas Martignoni's MoodleBox to Ubuntu ARM-64 Architecture.Moodlebox is a Moodle server and Wi-Fi router on Raspberry Pi. 

All credits goes to Nicolas Martignoni for the original Moodlebox. This port is keeping a different identity from the Original Moodlebox only for performance comparison purposes. This port has been done by Dhuny Riyad

## The MoodleBox Documentation

Visit the [MoodleBox web site][website] for more information about the MoodleBox features or any question about the usage of a MoodleBox.

If you just want to use the original Raspbian based version of MoodleBox, a [prepared disk image][download] of the latest released version is [available for downloading][download] and using out of the box on your Raspberry Pi 3A, 3B, 3B+ or 4B. Follow the instructions on the [MoodleBox web site][website].
If you want a ubuntu 64-bit version of Moodlebox, follow the instructions below to install one.

### Asking Support Questions

The original Moodlebox has an active [discussion forum][forum] where users and developers can ask questions. Please don't use the GitHub issue tracker to ask questions.

## Building the MoodleBox disk image from scratch

> If you just want to use a MoodleBox, __you don't need__ to build the MoodleBox disk image yourself. Just [download the MoodleBox image][download] and follow the instructions on the [MoodleBox web site][website]. If you want a spicy Moodlebox then follow the instructions below.

To build a Spicy MoodleBox from scratch with this script, you need a Raspberry Pi 3B, 3B+ or 4B.

1. Clone [Ubuntu ARM-64](https://ubuntu.com/download/raspberry-pi) on your microSD card.
1. Insert the microSD card into your Raspberry Pi.
1. Connect your Raspberry Pi to your Ethernet network and boot it.
1. Login with username `ubuntu` password `ubuntu`. Set new password to `Moodlebox4$`.
1. Create a `ssh.txt` file on the `boot` partition, e.g. using 
        
		sudo touch /boot/ssh.txt
		
1. [Install Ansible](https://docs.ansible.com/intro_installation.html) on your computer.
1. [Install `sshpass`](https://gist.github.com/arunoda/7790979) to enable passing SSH password to the Raspberry Pi. On macOS, use e.g. `brew tap esolitos/ipa; brew install sshpass`.
1. [Clone this repository][git] to your local drive. A quick way is:
	
	sudo apt install sshpass ansible -y
	
1. A server restart may be required for apt update & upgrade to remove locks on tmp files. Perform 
	
	sudo apt update && sudo apt upgrade -y
	
1. Create a `keys` directory in the repository folder and copy your public key into it, under the name `id_rsa.pub`.
1. A quick way  for above is: 

	ssh-keygen -t rsa

	with blanks as defaults. 
	
	sudo cp /home/ubuntu/.ssh/id_rsa.pub /home/ubuntu/spicyMoodlebox/keys/
	
1. Get the IP address of your Raspberry Pi and change it in the `hosts.yml` file. Do not change anything else, unless you know what you're doing. You're on your own.
1. Create a config.yml with `cp default.config.yml config.yml`.
1. Run   `ansible-playbook ubuntu.yml`   from the repository folder to prepare the OS.
1. Wait 1 -2 mins, playbook will change username from ubuntu to moodlebox, script will exit with failure.
1. Playbook will stop after username change, with an error similar to `Unable to create local directories(/home/ubuntu/.ansible/cp’`...
1. Reboot server with sudo init 6 and login with username moodlebox password Moodlebox4$
1. Run `ansible-playbook moodlebox.yml` from the repository folder.
1. Wait 15–50 minutes, depending on your Raspberry Pi model, SD card speed and Internet bandwidth. You're done.
1. On first boot run sudo passwd and change the default password.

### Overriding defaults

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can change the MoodleBox main credentials and the timezone with something like:

    moodlebox_username: 'myusername'
    moodlebox_password: 'secret'
    moodlebox_timezone: 'Australia/Perth'

Any variable can be overridden in `config.yml`; see the file `default.config.yml` for a list of available variables.

## Availability

The Moodlebox code is available at [https://github.com/moodlebox/spicymoodlebox][git].
The Spicy Moodlebox code is available at [https://github.com/dhuny/spicymoodlebox][git].

### Release notes

See Original Moodlebox [Release notes](https://github.com/moodlebox/moodlebox/blob/master/CHANGELOG.md).


## Thanks

The original Moodlebox thanks list

- To Daniel Méthot, for the [idea of a MoodleBox](https://moodle.org/mod/forum/discuss.php?d=278493)
- To Christian Westphal, for the [first POC](https://moodle.org/mod/forum/discuss.php?d=331170) of a MoodleBox
- To the [Raspberry Pi Foundation](https://www.raspberrypi.org/), for a splendid small computer
- To [Martin Dougiamas](https://en.wikipedia.org/wiki/Martin_Dougiamas), for giving us Moodle, and to the [Moodle community](https://moodle.org/)

## License

The original Moodlebox copyright info is as follow: 

Copyright © 2016 onwards, Nicolas Martignoni nicolas@martignoni.net.

All contributions to this repository are licensed under AGPLv3 or any later version.

MoodleBox doesn't require a CLA (Contributor License Agreement). The copyright belongs to all the individual contributors. Therefore we recommend that every contributor adds following line to the header of a file, if they
changed it substantially:

```
@copyright Copyright © <year>, <your name> (<your email address>)
```

  [website]: https://moodlebox.net
  [download]: https://moodlebox.net/download
  [forum]: https://discuss.moodlebox.net/
  [git]: https://github.com/moodlebox/moodlebox
