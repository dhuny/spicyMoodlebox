# Spicy MoodleBox



Spicy Moodlebox is a port of Martignoni's MoodleBox to Ubuntu ARM-64 Architecture. Moodlebox is a Moodle server and Wi-Fi router on a Raspberry Pi. 

All credits goes to Nicolas Martignoni for the original Moodlebox. This Spicy port was built and labelled  by Dhuny Riyad  as part of a research project only for performance comparison purposes. 
If you have reached here by accident, then you are surely searching to visit [Moodlebox](https://moodlebox.net) as this git is not regularly maintained.

As part of a  vision  to help education in remote areas, this work tries to upgrade the hardware and software environment of a Pi to compare the performance of Moodle on same.
If this can be helpful to anybody, the comparison results are available on [YouTube](https://www.youtube.com/watch?v=xMDy_Yv33C8).  

Also available, on Amazon is a book titled [Portable & Home hosted Learning Management Systems](https://www.amazon.com/Portable-hosted-Learning-Management-Systems/dp/B091H18XFV) which is linked to the performance comparison work. The book is still in its infancy but aims to disseminate the knowledge required to make portable Moodle LMS more common and accessible.

Contributors and reviewers for all the contents are most welcomed. A polite mail to the [author](mailto:riyad@dhuny.org) would be highly appreciated.


## The Spicy MoodleBox Documentation


If you want a ubuntu 64-bit version of Moodlebox, follow the instructions below to install one.


## Building the Spicy MoodleBox disk image from scratch

To build a Spicy MoodleBox from scratch with this script, you need a Raspberry Pi 3B, 3B+ or 4B.

1. Clone [Ubuntu ARM-64](https://ubuntu.com/download/raspberry-pi) on your microSD card. Last known working version was Ubuntu-20.04.2
2. Static Network addresses and Wifi credentials can be preconfigured in network-config present in SD card (Optional).
3. Insert the microSD card into your Raspberry Pi.
4. Connect your Raspberry Pi to your Ethernet network and boot it.
5. Login with username `ubuntu` password `ubuntu`. Set new password to `Moodlebox4$`.
6. Type the commands below to update and upgrade the Linux

		sudo apt update && sudo apt upgrade -y
		
8. Ubuntu will take a few minutes to upgrade as it may be running some updates already. 
9. Create a `ssh.txt` file on the `boot` partition, e.g. using 
        
		sudo touch /boot/ssh.txt
		
7. [Install Ansible](https://docs.ansible.com/intro_installation.html](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu)) on your computer.
8. [Install `sshpass`](https://gist.github.com/arunoda/7790979) to enable passing SSH password to the Raspberry Pi. On macOS, use e.g. `brew tap esolitos/ipa; brew install sshpass`.

			sudo apt install sshpass ansible -y

10. [Clone this repository](https://github.com/dhuny/spicyMoodlebox.git) to your local drive. A quick way is:
	
	git clone https://github.com/dhuny/spicyMoodlebox.git
	
11. Create a `keys` directory in the repository folder and copy your public key into it, under the name `id_rsa.pub`.

		mkdir /home/ubuntu/spicyMoodlebox/keys/

13. A quick way to create public keys is: 

		ssh-keygen -t rsa

	with blanks as defaults. 
	
		sudo cp /home/ubuntu/.ssh/id_rsa.pub /home/ubuntu/spicyMoodlebox/keys/
	
1. Get the IP address of your Raspberry Pi `ifconfig` or `ip a` and change it in the `hosts.yml` file. Do not change anything else, unless you know what you're doing. You're still on your own.
1. Create a config.yml if required with 		`cp default.config.yml config.yml`.
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

The Moodlebox code is available on [Moodlebox git](https://github.com/moodlebox/moodlebox).
The Spicy Moodlebox code is available on [SpicyMoodlebox git](https://github.com/dhuny/spicymoodlebox).

### Release notes, Thanks & License

This work is simply a port of the original work by Nicolas Martignoni, please visit [Moodlebox git](https://github.com/moodlebox/moodlebox) for the Original Release Notes, Thanks and License. 

This contribution is from [R.Dhuny](riyad@dhuny.org)

```
@copyright Copyright © <year>, <your name> (<your email address>)
```

