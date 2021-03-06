# vagrant

* https://docs.saltstack.com/en/latest/ref/modules/all/index.html
* https://docs.saltstack.com/en/latest/ref/states/all/index.html
* http://jinja.pocoo.org/docs/2.9/templates/
* https://docs.saltstack.com/en/latest/topics/jinja/index.html
* https://docs.saltstack.com/en/latest/topics/pillar/
* https://docs.saltstack.com/en/latest/topics/grains/index.html

This repository is meant to be used as a local development environment. Easiest way to get started is to use virtualbox provider.

```
vagrant up
```

Vagrant environment will also deploy a dedicated saltmaster VM, in order to reflect realistic production setup. However, we do not pre-seed minion keys, allowing minion to generate new certificate requests upon each `vagrant up`. Therefore, developer must accept key manually within the saltmaster VM.

```
vagrant ssh saltmaster
sudo salt-key -L
sudo salt-key -A -y
salt '*' test.ping
salt '*' grains.ls
salt '*' grains.get os
salt '*' cmd.run 'ifconfig'
```

Note that developer does not need superuser privileges to use salt execution modules as vagrant user. This is due to pre-configured ACL within salt-master config file. However, local system administration tasks (e.g. accepting certificate requests) still require elevation.

## States

Salt does configuration management, in addition to orchestration.

```
salt '*' state.apply <statename>
salt '*' state.highstate
```
