# Ansible CD using Jenkins

Since all the instances in the apigate ecosystem are with os-login enabled ssh keys wont work and we need ssh for running ansible scripts.

## Installation
Creating ssh keys for ansible for os login.

1) #### Create a service account in the GCP project the gke is in.
![Service account creation](/images/service_account.png)

2) #### Add the following roles to the service account.
* Compute Instance Admin (v1)
* Compute OS Admin Login
* Service Account User

3) #### Create key for service account and save it

![Service account key creation](/images/service_account_key.png)

Note: Make sure the key is in `json` format


4) #### Create SSH key for service account

```bash
ssh-keygen -f ssh-key-ansible-sa
```

5) #### Add SSH key for OS login to service account

```bash
gcloud auth activate-service-account \
    --key-file=gcp-key-ansible-sa.json
```
![Service account activation](/images/service_account_activation.png)

6) #### Add SSH key to the service account

```bash
gcloud compute os-login ssh-keys add \
    --key-file=ssh-key-ansible-sa.pub
```

Note the username from the output.

7) #### Switch back from service account
```bash
gcloud config set account your@gmail.com
```



## Jenkins setup

We have to install the following plugins in jenkins before we begin.

* AnsiColor
* Ansible plugin

Once the plugins are installed we can begin with the setup.

1) #### Create credentials for the ansible in Jenkins

Go to credentials and select add crediantals with the following fields.
![Ansible SSH credentials](/images/ansible_ssh_credentials.png)


Note:  The usernames should be the same that we got from the `gcloud compute os-login ssh-keys add` output.


2) #### Add pod Details for jenkins ansible image.

Go to Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Add Pod Template.

Add the following fields to the pod templates.

![Pod Templates](/images/pod_template_details1.png)
![Pod Templates2](/images/pod_template_details2.png)


3) #### Create Jenkins pipeline.

* Create a new pipeline project
* Add the github project where the ansible code is as the git trigger in project url.
* Check `GitHub hook trigger for GITScm polling` option
* Select `Pipeline Defination from SCM`
* Add repository url, credentials for git and branch to be triggered from.


## Usage

For running the ansible script, you just have to push the code in the git.

* Add path of the ansible script to be run in the `playbook` field.
* Add path of the host files for the ansible scripts to be run on at the `inventory` field.
