# ansible-ec2-provision

Simplified ec2 provisioning. 
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

Run to do full provision (new instance and provision):
```commandline 
ansible-playbook -i inventory/local site.yml -vvv
```

Provision only, update inventory.yml (ansible_host) ip then:
```commandline
ansible-playbook -i inventory.yml provision_instance.yml -vvv
```

Create a new instance only:
```commandline
ansible-playbook -i inventory/local new_instance.yml -vvv
```

## Notes

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