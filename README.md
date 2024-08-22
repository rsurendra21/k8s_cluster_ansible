# k8s_cluster_ansible

# Objective

The main objective is to automate the addition of nodes in the k8s cluster using ansible, and later deploy the nvidia gpu operator through helm chart. The base kubernetes provides the platform and on top of this we can have our own flavour of nvidia-gpu operator which will manages the orchestration of gpu, and above this we can use argo cd to deploy the applications superquick.

# Architecture

1. Ansible playbook consists of two roles which take care of installation of docker and addition of nodes in the k8s cluster
2. The prechecks role take care of installation of docker which is a prerequiste before creating k8s cluster
3. The k8s_cluster role take care of k8s cluster creation and addition of nodes to the cluster, when ever we need to scale the cluster by adding additional nodes we just need to upade nodes in cluster.yml and if require any taints or labels we can updates in the nodes section of cluster.yml file.
4. Helm will helps in installation of nvidia-gpu-operator or else we can deply the operator usning argocd, argo is prferred than helm.
5. Appliaction deployments will created in Github and will be deployed using argocd, argocd helps in quick rollback and have better visibility on the services.


# Future Work

1. for us to deploy appliactions on k8s cluster with gpu's we must maintain the same OS on all the nodes if we are going with the conatinerisation of nvidia-gpu-driver.
2. for observability we need to deploy the prometheus operator, which helps in monitoring the resource monitoring and helps in troubleshooting too.
3. for us to expose our traffic and to loadbalance and do ratelimiting of the incomming traffic toour services we must use either istio service mesh or nginx ingress controller.
4. we must use jenkins to run the ansible playbook and update the rke updated config and rkecluster state and store this in git repo, after every node addition.


# instructions to run ansible

The good thing about ansible is we are not required to make our hands dirty just one command is enough to setup the infra.

1. we must have one bastion server which has ansible installed in it.
2. check all the nodes which need to be added have ssh access from bastion server check this using

        ansible all -m ping 
3. run the below commnd to create the cluster
                  
         ansible-playbook playbook.yaml


