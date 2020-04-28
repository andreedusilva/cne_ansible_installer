### Oracle Linux Cloud Native Environment Installer

- Expects Oracle Linux 7u5 or higher hosts

These playbooks deploy an OLCNE version 1.1.x on top of 2 hosts/VMs (minimum).
To use them, first edit the `hosts` inventory file to contain the hostnames of the machines on which you want OLCNE deployed.

You need to add at least a worker and master node. The API-Server may be the same as the master node.

There is an example of `hosts` file:

    # OLCNE API Server
    api-server  ansible_host=api-server.inat.leao.pro.br

    # Kubernetes Master
    [masters]
    master      ansible_host=master.inat.leao.pro.br

    # Kubernetes Workers
    [workers]
    worker01    ansible_host=worker01.inat.leao.pro.br
    worker02    ansible_host=worker02.inat.leao.pro.br

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
    common_name: cloud.inat.leao.pro.br

    # All nodes to be installed (FQDN)
    all_nodes: api-server.inat.leao.pro.br,master.inat.leao.pro.br,worker01.inat.leao.pro.br,worker02.inat.leao.pro.br

And last, edit `group_vars/all` to setup proxy information of your installation or set `use_proxy` to `False`:

    # Use Proxy?
    use_proxy: False

    # Proxy URL and Rules
    proxy_host: "http://proxy.inat.leao.pro.br:80"
    no_proxy: ".inat.leao.pro.br,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16"

 ### Deploy Oracle Linux Cloud Native Environment

 The site.yml may be used to deploy a full Oracle Linux Cloud Native Environment.

 Run the playbook using:

    ansible-playbook -i hosts site.yml

 ### Creating a Kubernetes Cluster and Other Modules

 After playbook is completed, you can setup a Kubernetes cluster, others modules and a user to control K8s following this doc: https://docs.oracle.com/en/operating-systems/olcne/start/deploy-kube.html
