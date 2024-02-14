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

Update inventory/group_vars/all.yml with your defaults for instance

Run to do full provision:
```commandline 
ansible-playbook -i inventory/local site.yml -vvv
```

Create a new instance:
```commandline
ansible-playbook -i inventory/local new_instance.yml -vvv
```


Provision only, update inventory.yml then:
```commandline
ansible-playbook -i inventory.yml provision_instance.yml -vvv
```