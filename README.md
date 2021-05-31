# This project is to build-eks-cluster from CLI
---
<details><summary>1). Install kubectl on your system</summary>
<p> 

See [aws documentation](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) for guide.

- If using Windows OS open `PowerShell` as Administrator, enter the url below to download kubectl binaries.
```bash
curl -o kubectl.exe https://amazon-eks.s3.us-west-2.amazonaws.com/1.20.4/2021-04-12/bin/windows/amd64/kubectl.exe
```
- a). Create a `bin` folder in any location of your choice.
- b). Open location where `kubectl.exe` is located.
- c) .Copy or cut `kubectl.exe`  file and paste in you `bin` folder.
- d). Add the Path of the `bin` folder to your environment variable (under `User variable for <user-name>`)
- e). Enter `kubectl version --short --client` to test kubectl version.

</p>
</details>

<details><summary>2). Install eksctl on your system</summary>
<p>

If you are using Windows OS, cick [here](https://github.com/weaveworks/eksctl/releases/download/0.51.0/eksctl_Windows_amd64.zip) to download the zip file or see [aws documentation](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) for installation guide.

- a). Follow configuration steps( a to d) above to set up eksctl. Remember,you will have to create a folder with different name other than `bin` if they have to be in the same location or directory.
- b) Enter `eksctl version` to see its current version

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
- Ether `aws eks --region us-west-2 describe-cluster --name apple-cluster --query cluster.status` to check the status of the cluster. If successfully created, you should see `"ALIVE"`
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

- Now that your cluster is up and running, move to directory where your `"YAML defination file"` are located and start creating your appliactions.

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

<details><summary>5). Clean up </summary>
<p>

- Enter `$ eksctl delete cluster --region=us-west-2 --name=apple-cluster` to delete your cluster

</p>
</details>

<details><summary>6). Comming up next </summary>
<p>

- Helm 

</p>
</details>




