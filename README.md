# SFTP Server setup with ansible playbook and bash script

## Install & Configure

- Create a python virtual environment with ansible installed in it

```shell
sudo apt update && sudo apt install python3-pip python3-venv
python3 -m venv ansible
source ansible/bin/active
pip install wheel
pip install ansible==2.9.11
```

- Add your server in the ansible inventory file `hosts`

- Run the sftp-server.yml playbook with ansible
  - Some variables are encrypted inside the playbook, which requires ansible-vault password to actually run the playbook

```shell
ansible-playbook sftp-server.yml
```

## Notes

- There is also a `Vagrantfile` included in the [project](tests/Vagrantfile) if you want to test it locally
  - Install vagrant from [https://www.vagrantup.com/downloads](https://www.vagrantup.com/downloads)
  - Install VirtualBox [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
  - Run `vagrant up` in the **tests** folder
  - Run the ansible playbook `ansible-playbook sftp-server.yml`

## sftpuseradd manual

- This is the script that the will create the sftp users
- The script is installed in /usr/local/sbin/sftpuseradd
- It requires root/admin privileges to run
- The users home directory will be created in `/srv/sftp` by default,  configurable in the ansible playbook or shell script
- The created users are set to expire after **15 days** from their creation , also configurable in the ansible playbook or the actual script
- The created accounts have read-only permissions

- **View help message of the script**

```shell
sudo sftpuseradd
# or
sudo sftpuseradd -h
# or
sudo sftpuseradd --help
```

- **Create an sftp user**

```shell
sudo sftpuseradd testing1
```

- **Create an sftp user and send email with credentials to root@localhost by default**

```shell
sudo sftpuseradd -m testing1
```

## TODO

- Maybe: HTTP Access to SFTP Home with same username/password
- Maybe: Store sftp users in config file in git/gitlab and create the users with CI/CD pipeline
