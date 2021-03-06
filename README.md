## Carpe Noctem
TODO

### config
The is the main entry point if you want to change system configs.

`env` can be `dev` V `stage` V `prod`

### Setup Dev Machine
#### Automated
1. Run `sh run.sh` to setup environment and spin up local dev resources (e.g. VMs).

#### Manual
TODO

##### Updating Dev Machine files
* If you find another atom package, update and commit the `atom-requirements` using this command:
```
apm list --installed --bare > plays/roles/dev-machine/files/atom-requirements.txt
```

### Teardown
#### Automated
1. Run `sh stop.sh` to uninstall dev-machine software and teardown VMs.

#### Manual
TODO

### Common commands
```
ansible-playbook plays/setup-dev-machine.yml --ask-sudo-pass --extra-vars "@config.yml" --tags "vagrant"
ansible-playbook plays/spinup-env.yml --extra-vars "@config.yml" --tags "jenkins"
```
```
ansible local -a "echo 'hello'"
```
```
> ansible all -m setup -a 'filter=ansible_distribution'

# example run
jenkins-dev | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu"
    },
    "changed": false
}
127.0.0.1 | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "MacOSX"
    },
    "changed": false
}
database-dev | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu"
    },
    "changed": false
}
build-dev | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu"
    },
    "changed": false
}
web-dev | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu"
    },
    "changed": false
}

```


### Known Issues
#### Vagrant
You may encounter an issue if you're coding on a windows machine or OSX on late 2015 Macbook (read: not pro, not air) when running `vagrant up`.
```
$ vagrant up
Bringing machine 'jenkins-dev' up with 'virtualbox' provider...
Bringing machine 'web-dev' up with 'virtualbox' provider...
Bringing machine 'build-dev' up with 'virtualbox' provider...
==> jenkins-dev: Box 'ubuntu/trusty64' could not be found. Attempting to find and install...
    jenkins-dev: Box Provider: virtualbox
    jenkins-dev: Box Version: >= 0
The box 'ubuntu/trusty64' could not be found or
could not be accessed in the remote catalog. If this is a private
box on HashiCorp's Atlas, please verify you're logged in via
`vagrant login`. Also, please double-check the name. The expanded
URL and error message are shown below:

URL: ["https://atlas.hashicorp.com/ubuntu/trusty64"]
Error:
```
Fortunately, denizens of the internet have been able to work out a solution.  As a `MacOSX` on late 2015 Macbook model myself I was interested and able to resolve my issue by following /u/newrulez' suggestion [found in this thread](https://github.com/mitchellh/vagrant/issues/5016).

The fix was interesting because you have to download a better version of `curl` and let `vagrant` know about it.  Only then with running `vagrant up` be able to send those pesky XHRs to download the appropriate VM images.  Neat, eh?
