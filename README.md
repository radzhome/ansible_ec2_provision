# ansible-ec2-provision

simple ec2 server provision 
Used to provision new amazon EC2 instances for personal use and testing

## Usage

Install
```commandline
pip install boto3 ansible
```

Update keys.sh with your AWS keys (IAM/Users create)
```
export AWS_ACCESS_KEY_ID=''
export AWS_SECRET_ACCESS_KEY=''
```

Update inventory/group_vars/all.yml with your defaults for instance, go over each in aws_defaults
```commandline
vim inventory/group_vars/all.yml
```

Add your key to ssh-agent
```commandline
ssh-add ~/.ssh/id_rsa-aws
```

Run to do full create + provision (new instance and provision):
```commandline 
ansible-playbook -i inventory/local site.yml -vvv
```

Provision only, update inventory.yml (ansible_host, ansible_user) then:
```commandline
ansible-playbook -i inventory.yml provision_instance.yml -vvv
```

Create a new instance only (ensure credentials filled):
```commandline
ansible-playbook -i inventory/local new_instance.yml -vvv
```

## Migration notes (optional)

Moving existing things like /var/web, /etc/letsencrypt zip i.e.
```commandline
sudo tar -czvf ~/letsencrypt_backup.tar.gz /etc/letsencrypt
sudo tar -czvf ~/dokuwiki_backup.tar.gz /var/web/dokuwiki
sudo tar -czvf ~/web_backup.tar.gz /var/web
# and copy to new server
scp -i ~/.ssh/id_rsa ubuntu@<ip>:~/letsencrypt_backup.tar.gz ~
scp -i ~/.ssh/id_rsa ubuntu@<ip>:~/dokuwiki_backup.tar.gz ~
# and extrat
tar -xzvf letsencrypt_backup.tar.gz
tar -xzvf ~/dokuwiki_backup.tar.gz 


# dokuwiki cp files and make sure permissions match
cp -f conf/local.php /var/web/dokuwiki/conf/
cp -f conf/users.auth.php /var/web/dokuwiki/conf
cp -f conf/mime.local.conf /var/web/dokuwiki/conf
cp -f conf/acl.auth.php /var/web/dokuwiki/conf
mv  data/ /var/web/dokuwiki/
```


## Instance notes

Command `free -m` might show different memory available due to os kernel size.

`sudo dmesg | grep -i memory` i.e.

```
Memory: 357336K/481324K available (20480K kernel code, 4261K rwdata, 13248K rodata, 4908K init, 17284K bss, 123728K reserved, 0K cma-reserved)
```

https://serverfault.com/questions/1057043/aws-not-giving-advertised-ram

In /var/log/syslog you might see something like:
```
A process of this unit has been killed by the OOM killer.
Out of memory: Killed process 1678 (apt-check) total-vm:96404kB ...
```

Check top 20 mem usage processes:
```commandline
ps aux --sort -rss | head -20
```

Memory summary:
t3.nano(x64) ubuntu22.04 407mb
t3.nano(x64) debian12 TODO
t2.nano(x385) ubuntu22.04 445mb
t2.nano(x386) ubuntu18.04 486mb
t2.nano(x386) debian12 470mb
t3 has more cpu and bandwidth. You can change by stopping instance, go to instance settings and change instance type and start again.


## Debian notes

Changes would have to be made to use debian, i.e.:

* have to use apt to install certbot
* have to use php 8.2 instead of 8.1 (ubuntu)