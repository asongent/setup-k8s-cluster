# This project is to build-eks-cluster from CLI
---
<details><summary>1). Install kubectl on your system</summary>
<p> 

- If using Windows OS open `PowerShell` as Administrator, enter the url below to download kubectl binaries.
```bash
curl -o kubectl.exe https://amazon-eks.s3.us-west-2.amazonaws.com/1.20.4/2021-04-12/bin/windows/amd64/kubectl.exe
```
- a). Create a `bin` folder in any location of your choice.
- b). Open location where `kubectl.exe` is located.
- c) .Copy or cut `kubectl.exe`  file and paste in you `bin` folder.
- d). Add the Path of the `bin` folder to your environment variable (under `User variable for <user-name>`)
- e). Enter `kubectl version --short --client` to test kubectl version.
- you may see [aws documentation](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) for installation guide.
</p>
</details>

<details><summary>2). Install eksctl on your system</summary>
<p>

- a). If you are using `Windows OS`, cick [here](https://github.com/weaveworks/eksctl/releases/download/0.51.0/eksctl_Windows_amd64.zip) to download the zip file or see [aws documentation](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) for installation guide.

- b). Follow configuration steps( a to d) above to set up eksctl. Remember,you will have to create a folder with different name other than `bin` if they have to be in the same location or directory.
- c). Enter `eksctl version` to see its current version

</p>
</details>

<details><summary>3). Configure aws Credentials</summary>
<p>

- Open `command prompt` or and enter `aws configure` and the enter your `AWS Access Key ID`, `AWS Secret Access Key `, `Default region name [us-east-1]`, etc

 ```bash
 AWS Access Key ID [****************OZOR]: HZYOSSXYULSJSJSHSKK
 AWS Secret Access Key [****************ifZc]: rtshjskll+kksksksksssksks
Default region name [us-east-1]: us-west-1
Default output format [None]: 
```
- If you don't have `awscli` installed, see [aws documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html) for installation guides.

</p>
</details>

<details><summary>4). Create your cluster</summary>
<p>

- On `PowerSell`, enter the command below to create your cluster named `apple-cluster`,
```bash
eksctl create cluster \
--name apple-cluster \
--version 1.16 \
--region us-west-2 \
--nodegroup-name linux-nodes \
--node-type t3.large \
--nodes 2
```
- Enter `aws eks --region us-west-2 describe-cluster --name apple-cluster --query cluster.status` to check the status of the cluster. If successfully created, you should see `"ALIVE"`
- Enter `aws eks --region us-west-2 update-kubeconfig --name apple-cluster` to update and export kubernetes configuration file to your local.
- `$ cat ~/.kube/config` to see kubernetes configuration file. You shold see something like this. 
```bash
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: CERT
    server: https://xxxxxxx.sk1.us-east-1.eks.amazonaws.com
  name: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
contexts:
- context:
    cluster: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
    user: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
  name: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
current-context: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
kind: Config
preferences: {}
users:
- name: arn:aws:eks:us-east-1:xxxxxxx:cluster/eks-cluster
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      args:
      - --region
      - us-east-1
      - eks
      - get-token
      - --cluster-name
      - eks-cluster
      command: aws
```

- Now that your cluster is up and running, move to directory where your `"YAML defination files"` are located and start creating your appliactions.

```bash
kubectl get ns
```
```bash
kubectl get cs
```
```bash
kubectl get node
```
```bash
kubectl get pods -A
```
```bash
kubectl get apiservices
```
```bash
kubectl get svc
```

</p>
</details>

<details><summary>5). To create an IAM OIDC identity provider for your cluster with eksctl</summary>
<p>
 
 - Determine whether you have an existing IAM OIDC provider for your cluster.

View your cluster's OIDC provider URL.

 ```bash
 aws eks --region us-west-2 describe-cluster --name <cluster_name> --query "cluster.identity.oidc.issuer" --output text
 ```
 Example output:

```bash
https://oidc.eks.us-west-2.amazonaws.com/id/EXAMPLED539D4633E53DE1B716D3041E
```
- List the IAM OIDC providers in your account. Replace <EXAMPLED539D4633E53DE1B716D3041E> (including <>) with the value returned from the previous command.

 ```bash
 aws iam list-open-id-connect-providers | grep <EXAMPLED539D4633E53DE1B716D3041E>
 ```

 Example output:

 ```bash
 "Arn": "arn:aws-cn:iam::111122223333:oidc-provider/oidc.eks.us-west-2.amazonaws.com/id/EXAMPLED539D4633E53DE1B716D3041E"
 ```

 If output is returned from the previous command, then you already have a provider for your cluster. If no output is returned, then you must create an IAM OIDC provider.

- Create an IAM OIDC identity provider for your cluster with the following command. Replace <cluster_name> (including <>) with your own value.

```bash
eksctl --region us-west-2 utils associate-iam-oidc-provider --cluster apple-cluster --approve
```
**To create an IAM OIDC identity provider for your cluster with the AWS Management Console**

- Open the Amazon EKS console (here)[https://console.aws.amazon.com/eks/home#/clusters].

- Select the name of your cluster and then select the **Configuration** tab.

- In the **Details** section, note the value of the **OpenID Connect provider URL**.

- Open the IAM console (here)[https://console.aws.amazon.com/iam/].

- In the navigation panel, choose Identity Providers. If a Provider is listed that matches the URL for your cluster, then you already have a provider for your cluster. If a provider isn't listed that matches the URL for your cluster, then you must create one.

- To create a provider, choose **Add Provider**.

- For **Provider Type**, **choose OpenID Connect**.

- For **Provider URL**, paste the OIDC issuer URL for your cluster, and then choose **Get thumbprint**.

- For **Audience**, enter `sts.amazonaws.com` and choose **Add provider**.
</p>
</details>

<details><summary>6). Clean up </summary>
<p>

- Enter `$ eksctl delete cluster --region=us-west-2 --name=apple-cluster` to delete your cluster

</p>
</details>

<details><summary>7). Comming up next </summary>
<p>

- Helm 

</p>
</details>





