# wikipod
Install a hosted Wikipedia clone on Debian utilizing [kiwix](https://kiwix.org). It targets small devices like the Raspberry Pi and virtual machines.

* tested on debian 10.0+
* utilizes [kiwix-serve](https://wiki.kiwix.org/wiki/Kiwix-serve)
* caches responses with Nginx
* downloads zim files with bittorrent using [aria2](https://aria2.github.io/)
* aria2 will use [local peer discovery](https://en.wikipedia.org/wiki/Local_Peer_Discovery) to create a LAN bittorrent swarm, saving bandwidth & time on simultaneous deploys to local devices
* Service logs and the cache are kept in memory to reduce disk wear

## usage
### testing

First, [install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine), then [install Vagrant](https://www.vagrantup.com/), then run:
```
vagrant up
```

### "production" usage

Check out the `group_vars/all` file to change which .zim files to download; a list of .zim files [can be found here](https://wiki.kiwix.org/wiki/Content_in_all_languages). To run this against a host you have SSH access to, [create a `hosts`/inventory file for Ansible](https://codereviewvideos.com/course/ansible-tutorial/video/ansible-inventory-with-our-own-hosts-files). The bulk of the data will be stored in the `data_volume` specified in `group_vars/all`, with a default value of `/var/data`; consider mounting a big or fast disk here. Then run something like this:

```
ansible-playbook -bkK -i hosts site.yml
```

## todo
* explicitly host an access point and route all traffic to `kiwix-serve` instance
* handle "data" volume more gracefully.
* set up `unattended-upgrades` & `logrotate`
* implement some measures to lengthen the lifespan of an SD card, like [these suggestions](https://raspberrypi.stackexchange.com/questions/169/how-can-i-extend-the-life-of-my-sd-card) and perhaps disabling rsyslogd, cups
