# we SSH to our Ansible-server and sudo to ansadmin

- next : we will go to our /opt/docker
$ cd /opt/docker

- next: we will create a deployment file using ansible playbook
$ nano kube_deployment.yml

```
---
- hosts: kubernetes
  user: root
  tasks:
  - name: deploy reg app on kubernetes
    command: kubectl apply -f deployment.yaml

```

we have deployment.yaml on our root on the bootstrap ec2 instance

# since we have our Inventory file with the Ec2 Bootstrap instance IP and we have been able to test connectivity scuessfully then we should be able to use the below command on our ansible host

```
$ ansible-playbook kube_deployment.yml - i inventory

```

# we want to have a CI and also a CD job on our jenkins

- next : we go have a CD job that deploys 
and we link it with a CI job that builds new image to the dockerhub

using the post build ADD build other project which helps us link the 2 jobs together 


# on our ansible playbook we have a rolling update which helps rollout retsrat when we have a new update on dockerhub
```
---
- hosts: kubernetes
  user: root
  tasks:
  - name: deploy reg app on kubernetes
    command: kubectl apply -f deployment.yaml

  - name: update deployment with new pods if image update in dockerhub
    command: kubectl rollout restart deployment.v1.apps/opeyemi-regapp

```


