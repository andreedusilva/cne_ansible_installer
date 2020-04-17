### Oracle Linux Cloud Native Environment Installer

- Expects Oracle Linux 7u5 hosts

These playbooks deploy an OLCNE version 1.0.x on top of 2 hosts/VMs (minimum). 
To use them, first edit the `hosts` inventory file to contain the hostnames of the machines on which you want OLCNE deployed.

You need to add at least a worker and master node. The API-Server may be the same as the master node.

There is an example of `hosts` file:

    # OLCNE API Server
    api-server  ansible_host=api-server.leao.org

    # Kubernetes Master
    [masters]
    master      ansible_host=master.leao.org

    # Kubernetes Workers
    [workers]
    worker01    ansible_host=worker01.leao.org
    worker02    ansible_host=worker02.leao.org

    # Kubernetes hosts
    [kubernetes:children]
    masters
    workers

Then edit `host_vars/api-server` file to edit self-signed certification data as below (do not forget to add all FQDN of hostnames of your installation on `all_nodes` variable):

    # Certificate information
    org_unit: Labs
    org: Leao Org
    locality: Salvador
    state: Bahia
    country: BR
    common_name: cloud.leao.org

    # All nodes to be installed (FQDN)
    all_nodes: api-server.leao.org,master.leao.org,worker01.leao.org,worker02.leao.org

And last, edit `group_vars/all` to setup proxy information of your installation or set `use_proxy` to `False`:

    # Use Proxy?
    use_proxy: False

    # Proxy URL and Rules
    proxy_host: "http://wwww-proxy.us.leao.org:80"
    no_proxy: ".leao.org,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16"
 
 ### Deploy Oracle Linux Cloud Native Environment
 
 The site.yml may be used to deploy a full Oracle Linux Cloud Native Environment. 
 
 Run the playbook using:
 
    ansible-playbook -i hosts site.yml
 
 ### Creating a Kubernetes Cluster and Other Modules
 
 After playbook is completed, you can setup a Kubernetes cluster, others modules and a user to control K8s following this doc: https://docs.oracle.com/en/operating-systems/olcne/start/deploy-kube.html
 

