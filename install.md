To run:

1) install boto

    pip install boto ansible

2) in your bash_profile add AWS Keys & source ie.

    export AWS_ACCESS_KEY_ID=''
    export AWS_SECRET_ACCESS_KEY=''

3) run ssh-add on your ssh key used for the instance

4) run on control system:

    # To create ec2 volumes on existing instance
    ansible-playbook -i inventory/ec2env site.yml

    # To create a new instance & volumes ONLY, instance name = test-staging (name tag)
    ansible-playbook -i inventory/local new_instance.yml --extra-vars "project=test site=staging"

    # Create instance & mount volumes, don't check host keys on connect to new instance
    # export ANSIBLE_HOST_KEY_CHECKING=False  to not prompt for accepting ssh key
    # -e 'host_key_checking=False'
    ansible-playbook -i inventory/ec2env site.yml --extra-vars "project=test site_name=staging"
    ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory/ec2env site.yml --extra-vars "project=test site_name=staging" -vvvv


    # ec2 connection test, update appservers ip first, then
    ansible-playbook -i inventory/ec2env ec2_conn_test.yml

Note: Make sure inventory is updated with correct appserver(s)

Note: When done testing, don't forget to clean up to minimize AWS costs:

Instance Cleanup:

- Remove the instances manually
- Remove the Volumes & Elastic IPs manually



Test out ForwardAgent with git module:

1) Make sure step 2 is setup in above & ensure test_ssh_fwd.yml git command points to a valid private repo

2) Make sure to update intventory with correct app server.


3) Make sure ~/.ssh/config contains:

Host *
    ForwardAgent yes


4) run ssh-add on your ssh key that you want to use

5) run on control system:

    ansible-playbook -i inventory/ec2env test_ssh_fwd.yml


References: http://thinkingeek.com/2014/05/10/create-configure-ec2-instances-rails-deployment-ansible/
