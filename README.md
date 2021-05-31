# This project is to build-eks-cluster from CLI

##### 1). Install kubectl on your system 
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

---

##### 2). Install eksctl on your system

If you are using Windows OS, cick [here](https://github.com/weaveworks/eksctl/releases/download/0.51.0/eksctl_Windows_amd64.zip) to download the zip file or see [aws documentation](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) for installation guide.

- a). Follow configuration steps( a to d) above to set up eksctl. Remember,you will have to create a folder with different name other than `bin` if they have to be in the same location or directory.
- b) Enter `eksctl version` to see its current version

---
##### 1) Configure aws Credentials
- Open `command prompt` or and enter `aws configure` and the enter your `AWS Access Key ID`, `AWS Secret Access Key `, `Default region name [us-east-1]`, etc

 ```bash
 AWS Access Key ID [****************OZOR]: HZYOSSXYULSJSJSHSKK
 AWS Secret Access Key [****************ifZc]: rtshjskll+kksksksksssksks
Default region name [us-east-1]: us-west-1
Default output format [None]: 
```
- If you don't have `awscli` installed, see [aws documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html) for installation guides.






