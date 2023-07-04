# Module 2 - Create an EKS cluster and Connect it to Calico Cloud

For EKS, the Calico OSS can be used as a CNI, or you can make use of the AWS VPC networking as CNI and have Calico only as a plugin for the security policies. 

We will use the second approach during this workshop. Please, find below an example on how to create a two nodes cluster with an small footprint, but feel free to create your EKS cluster with the parameters you prefer. Do not forget to include the region if different than the default on your account.

```bash
export CLUSTERNAME=tigera-workshop
export REGION=ca-central-1
# 
echo "# Start Lab Params" > ~/labVars.env
echo export CLUSTERNAME=$CLUSTERNAME >> ~/labVars.env
echo export REGION=$REGION >> ~/labVars.env
#
eksctl create cluster --name $CLUSTERNAME --version 1.26  --region $REGION --node-type m5.xlarge
```

- View EKS cluster.

  Once cluster is created you can list it using eksctl.
  
  ```bash
  eksctl get cluster tigera-workshop --region $REGION
  ```

- Test access to EKS cluster with kubectl

  Once the EKS cluster is provisioned with eksctl tool, the kubeconfig file would be placed into ~/.kube/config path. The kubectl CLI looks for kubeconfig at ~/.kube/config path or into KUBECONFIG env var.

  ```bash
  # verify kubeconfig file path
  ls ~/.kube/config
  # test cluster connection
  kubectl get nodes
  ```
  
## Connect your cluster to Calico Cloud

> **Note**: In order to complete this module, you will need to a [Calico Cloud account](https://www.calicocloud.io/). If you are participating in a live workshop, you will receive an invite with the information to login into an active Calico Cloud environment and join your AKS cluster in there.  
If you are running this workshop in a self-paced mode, you can create an Calico Cloud environment following the steps [here](/modules/submodule-3.1-create-calicloud.md).  

Issues with being unable to navigate menus in the UI are often due to browsers blocking scripts - please ensure that you disabled all blocker scripts.

## Step 1 - Accept the Invitation

1. During the workshop, you will receive an invitation to connect to a Calico Cloud organization, just like in the picture below:
 
   ![accept_invitation](https://user-images.githubusercontent.com/104035488/215204989-66b666d9-5e93-45b5-a0c5-2236b135af31.png)
   
2. Click on the link ACCEPT INVITATION and create an password to access the Calico Cloud.

   <img width="300" alt="create a password" src="https://user-images.githubusercontent.com/104035488/215205017-0a41f506-5c91-4830-9249-677c6a06fb3b.png">

3. Once you have access to your **Calico Cloud** environment, go to step 2:

## Step 2 - Connecting your cluster to Calico Cloud.

1. The welcome screen will allow you to choose among four use cases and will provide a guided tour for each use case. After that you can proceed to connect your first cluster. This option directs you to the **Managed Clusters** section. Click on the "**Connect Cluster**" button to start the process.

   The Connect Cluster window will allow you to choose a name to identify your cluster in Calico Cloud and select which platform you are running the cluster on. The next window presents a link for you to review the cluster requirements for Calico Cloud. A kubectl command to run the installation script will be generated, you need to copy and apply this command in your cluster.

   ![registering_get_key](https://user-images.githubusercontent.com/104035488/188036064-f85cac4f-66c0-4c09-bdd3-67922640679d.gif)

2. Run the installation script in your cluster. Script should look similar to this:
    
    <pre>
    kubectl apply -f https://installer.calicocloud.io/manifests/cc-operator/lat
    est/deploy.yaml && curl -H "Authorization: Bearer a7c2oex34:00llxrhcq:1ga2c
    z69d7ug81yjgakpyclv6o3eu8o97kp7t2483lmwajslu47xed94e4ic8ywn" "https://www.c
    alicocloud.io/api/managed-cluster/deploy.yaml" | kubectl apply -f -
    </pre>

    > Output should look similar to:
    <pre>
    namespace/calico-cloud created
    customresourcedefinition.apiextensions.k8s.io/installers.operator.calicocloud.io created
    serviceaccount/calico-cloud-controller-manager created
    role.rbac.authorization.k8s.io/calico-cloud-leader-election-role created
    clusterrole.rbac.authorization.k8s.io/calico-cloud-metrics-reader created
    clusterrole.rbac.authorization.k8s.io/calico-cloud-proxy-role created
    rolebinding.rbac.authorization.k8s.io/calico-cloud-leader-election-rolebinding created
    clusterrolebinding.rbac.authorization.k8s.io/calico-cloud-installer-rbac created
    clusterrolebinding.rbac.authorization.k8s.io/calico-cloud-proxy-rolebinding created
    configmap/calico-cloud-manager-config created
    service/calico-cloud-controller-manager-metrics-service created
    deployment.apps/calico-cloud-controller-manager created
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100   355  100   355    0     0    541      0 --:--:-- --:--:-- --:--:--   541
    secret/api-key created
    installer.operator.calicocloud.io/aks-cc-repo created
    </pre>
    
    Joining the cluster to Calico Cloud can take a few minutes. Meanwhile the Calico resources can be monitored until they are all reporting `Available` as `True`

    ```bash
    kubectl get tigerastatus                                                                                                                    
    ```
    
    > Output should look similar to:
    <pre>
    NAME                            AVAILABLE   PROGRESSING   DEGRADED   SINCE
    apiserver                       True        False         False      96s
    calico                          True        False         False      16s
    compliance                      True        False         False      21s
    intrusion-detection             True        False         False      41s
    log-collector                   True        False         False      21s
    management-cluster-connection   True        False         False      51s
    monitor                         True        False         False      2m1s
    </pre>

    You can also monitor your cluster installation on the Calico Cloud UI. Go to the "**Managed Clusters**" section, select your cluster and expand the timestamp dropdown to see the installation logs.
    In a few minutes the status will change from **Installing** to **Done**. Congratulations! You successfully connected your cluster to Calico Cloud.

    ![installing](https://user-images.githubusercontent.com/104035488/188036070-71cd3cb7-639b-46f2-bd5e-dbdb401b48e3.gif)

## STEP 3 - Selecting your cluster.

Once the installation is completed, you will be able to start interacting with your cluster from the Calico Cloud interface. Calico Cloud provides a single pane of glass for managing multiple clusters. If you followed the previous steps, you would have two clusters connected to Calico Cloud at this point: Your cluster and a pre-configured lab cluster that allows you to explore some of the features in Calico Cloud.

You can switch between clusters by following the steps below:

1. Navigate to the Dashboard section - the first icon under the Calico Cat on the top-left of the UI.

2. Click on the **Cluster** dropdown button on the top-right of the UI.

3. Select your recently added cluster.

   ![selecting_cluster](https://user-images.githubusercontent.com/104035488/188036074-857e6a19-7641-4dff-9f6b-02eb627cf748.gif)

The "**Cluster**" dropdown button will be always visible accross the Calico Cloud UI, no matter which section you are viewing. You can change the cluster you want to interact with at any moment. 
When you change the cluster, the whole Calico Cloud context will change immediatelly to reflect the information regarding the current selected cluster.




Subscribe to the free Calico Cloud trial on the link below:

https://www.calicocloud.io/home

Once you are able to login to Calico Cloud UI, go to the "Managed clusters" section, and click on the "Connect Cluster" button, then leave "Amazon EKS" selected, and give a name to your cluster, and click "Next". Read the cluster requirements in teh next section, and click "Next". Finally, copy the kubectl command you must run in order to connect your cluster to the management cluster for your Calico Cloud instance.

![cluster](https://github.com/tigera-solutions/cc-eks-compliance/assets/104035488/fd7d1d47-9a65-4d45-8798-a50fd125447c)

---

## Enviroment Preparation

### Decrease the time to collect flow logs

By default, flow logs are collected every 5 minutes. We will decrease that time to 15 seconds, which will increase the amount of information we must store, and while that is not recommended for production environments, it will help to speed up the time in which events are seen within Calico observability features.

```bash
kubectl patch felixconfiguration default -p '{"spec":{"flowLogsFlushInterval":"15s"}}'
kubectl patch felixconfiguration default -p '{"spec":{"dnsLogsFlushInterval":"15s"}}'
kubectl patch felixconfiguration default -p '{"spec":{"flowLogsFileAggregationKindForAllowed":1}}'
kubectl patch felixconfiguration default -p '{"spec":{"flowLogsFileAggregationKindForDenied":0}}'
kubectl patch felixconfiguration default -p '{"spec":{"dnsLogsFileAggregationKind":0}}'
```

Configure Felix to collect TCP stats - this uses eBPF TC program and requires miniumum Kernel version of v5.3.0/v4.18.0-193. Further documentation.

```bash
kubectl patch felixconfiguration default -p '{"spec":{"flowLogsCollectTcpStats":true}}'
```

### Install demo applications

Deploy the dev app stack

```bash
kubectl apply -f manifests/dev-app-manifest.yaml
```
 
Deploy the Online Boutique app stack

```bash
kubectl apply -f manifests/kubernetes-manifests.yaml
```

--- 

[:arrow_right: Module 3 - Scan Container Images](/modules/module-3-scan-images.md)  <br>

[:arrow_left: Module 1 - Prerequisites](/modules/module-1-prereq.md)  
[:leftwards_arrow_with_hook: Back to Main](/README.md)  
