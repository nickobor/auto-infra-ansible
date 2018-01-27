## Automated Infrastructure Creation with Ansible

## Google Cloud Platform

The project is implemented on Google Cloud.

## Software

1. Install Dependencies.

    ```
    sudo apt-get install -y build-essential git python-dev python-pip
    ```
2. Install Ansible.

3. Install libcloud.
    ```
    sudo pip install apache-libcloud==0.20.1
    ```

## Ansible demo setup

1. Check out this repository.
    ```
    cd $HOME
    git clone https://github.com/nickobor/auto-infra-ansible.git
    ```

2. Edit the `gce_vars/auth` file and specify your Project ID in the
`project_id` variable, Service Account email address in the `service_account_email` variable,
and the location of your JSON key (downloaded earlier) in the `credentials_file` variable.
    ```
    ---
    # Google Compute Engine required authentication global variables
    # (Replace 'YOUR_PROJECT_ID' with the Project ID used in creating your GCP project.)
    project_id: YOUR_PROJECT_ID
    service_account_email: demo-ansible@YOUR_PROJECT_ID.iam.gserviceaccount.com
    credentials_file: /home/ansible-user/demo-ansible.json
    ```

Use `ansible-playbook` to create and bootstrap
the instances, install Apache, and set up a Compute Engine load-balancer.

## Run the playbook

Use the `ansible-playbook` command to create the instances based on the
attributes in the configuration files.

```
ansible-playbook site.yml
```

1. The output from this command will display the public IP address associated
with your new load-balancer. You can also look in the Developers Console
under *Networking &gt; Load balancing* and find the Forwarding Rules under the 
"Advanced" menu.

1. Put the public IP address of your load-balancer into
your browser and take a look at the result. Within a few seconds you should
start to see a flicker of pages that will randomly bounce across each of your
instances.

    For the demo, a javascript function is set to fire when the page loads
that pauses for a half-second, and then reloads itself. Since we installed
a modified Apache configuration file to disable client-side caching *and* we
enabled Apache's `mod_headers`, each "reload" results in a new HTTP request
to the page. This is just a fancy hands-free way of asking you to do a
"hard refresh" of the load-balancer IP address in order to see the cycling
between instances.

To run the demo go to http://35.224.151.179
Please close the page after the demo!


