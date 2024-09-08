This is a simple playbook to provision an Ubuntu server with CTFd using the Ansible playbook.

It runs CTFd using Gunicorn and NGINX, and installs a Lets Encrypt SSL certificate with certbot.

The CTFd site will be unconfigured and require immediate setup. Alternatively you can import your own backup of a CTFd instance by placing ctfd_backup.zip in the root folder of the project.

Comment out the certbot generation for testing so you don't trip the API limits for the same domain in a five day period if you are doing frequent testing.

`service_pass.py` uses Ansible Vault to generate service account passwords for MariaDB and Redis cache.

Check variables in group_vars/ctfd.yml.

The Ansible command looks for a hosts.ini file that contains the server(s) you want to run the playbook against:
```yaml
[ctfd]
123.456.78.9
ctfd.myhost.com
```

It is assumed that the server has had a ctfd user provisioned to install and run CTFd... Will eventually make this part of the playbook.

```bash
sudo useradd ctfd -m -s /bin/bash
sudo usermod -aG sudo ctfd
sudo mkdir -p /home/ctfd/.ssh
# Public Key is created and uploaded to /tmp
sudo cp /tmp/public_key_filename /home/ctfd/.ssh/authorized_keys",
sudo chown -R ctfd:ctfd /home/ctfd/.ssh
sudo chmod 700 /home/ctfd/.ssh && sudo chmod 600 /home/ctfd/.ssh/authorized_keys
sudo su -c \"echo 'ctfd ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers.d/ctfd\"
```

```bash
python3 service_pass.py

ANSIBLE_FORCE_COLOR=1 ansible-playbook --vault-password-file vault_password -u ctfd -i hosts.ini --private-key <LOCATION OF PRIVATE KEY> --ssh-common-args='-o StrictHostKeyChecking=no' playbook.yml -vv"
  }`
```
