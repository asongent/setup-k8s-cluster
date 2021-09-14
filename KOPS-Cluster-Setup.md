
#### Set-up k8s cluster with KOPS

**Step 1**: Create Ubuntu EC2 instance
**Step 2**: install AWSCLI 

```bash
curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
 apt install unzip python
 unzip awscli-bundle.zip
 #sudo apt-get install unzip - if you dont have unzip in your system
 ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

**Step 3**:  Install kubectl on ubuntu instance

```bash

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 chmod +x ./kubectl
 sudo mv ./kubectl /usr/local/bin/kubectl

```

**Step 4**: Install kops on ubuntu instance.

```bash
 curl -LO  https://github.com/kubernetes/kops/releases/download/1.15.0/kops-linux-amd64
 chmod +x kops-linux-amd64
 sudo mv kops-linux-amd64 /usr/local/bin/kops
 kops version (it should be 1.15.0)
 Note: use below command if you wish to use latest version. For now we could see latest version of kops. So ignore it until further update. 
 # curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
```
**Step 5**: Create an `IAM user/role` with `Route53`, `EC2`, `IAM` and `S3 full access` and Attach it to ubuntu instance

**Step 6**: Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)

Go to 

Route53 --> hosted zones --> created hosted zone  
Domain Name: fredtech.com, tech.net etc
Type: Private hosted zone for Amazon VPC. Make sure you are chosing right VPC if you have multiple

**Step 7**: Create s3 bucket, set env variable, and generate keys

```bash 
aws s3 mb s3://kops-bucket-demo
```

```bash
export KOPS_STATE_STORE=s3://kops-bucket-demo
```

```bash
ssh-keygen
```

**Step 8**: Create kubernetes cluster definitions on S3 bucket.

```bash
kops create cluster --cloud=aws --zones=us-west-2b --name=01-bucket-sample --dns-zone=fredtechnology.com --dns private 
```

**Step 9**: Create kubernetes cluser.
 
```bash
kops update cluster kops-bucket-demo --yes
```

**Step 10**: To cahnge the kubernetes master and worker instance sizes.

```bash
kops edit ig --name=<cluster_name> nodes
#kops edit ig --name=kops-bucket-demo nodes 
kops edit ig --name=<cluster_name> master-<zone_name>
#kops edit ig --name=demo.kops-bucket-demo master-us-west-2b

```

**Step 11**: Validate your cluster.

```bash
 kops validate cluster
```



