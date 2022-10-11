## INSTALAR CLIENTE KUBERNETES:

source: 

https://docs.aws.amazon.com/es_es/eks/latest/userguide/getting-started-console.html

https://www.youtube.com/watch?v=7SwFzYKySu0&ab_channel=Releaseworks

https://medium.com/blog-apside/cluster-de-kubernetes-con-aws-eks-explicado-e1c00c6ec686


------------------------------------------

### 1 - INSTALAR AWS CLI:

https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-format

`sudo apt install awscli`

`aws configure`

_AWS Access Key ID [None]: aca poner el ID key de aws_

_AWS Secret Access Key [None]: aca poner la sec key_

_Default region name [None]: us-east-1_

_Default output format [None]: json_


Try work: aws ec2 describe-regions

directory: cd ~/.aws

--------------------------------------------

### 2 - INSTALAR EKSCTL:

https://eksctl.io/introduction/#installation

`curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp`


`sudo mv /tmp/eksctl /usr/local/bin`


`eksctl version`

--------------------------------------

### 3 - INSTALAR KUBECTL:
v1.19:
`curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.0/bin/linux/amd64/kubectl`

ó

Última versión:
`curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"`

IMPORTANTE: puede ser que la última versión no funcione correctamente, da el error:

_error: exec plugin: invalid apiVersion "client.authentication.k8s.io/v1alpha1"_

_Unable to connect to the server: getting credentials: decoding stdout: no kind "ExecCredential" is registered for version "client.authentication.k8s.io/v1alpha1" in scheme "pkg/client/auth/exec/exec.go:62"_

Para solucionar eso usar la versión 1.23.6:

`curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.23.6/bin/linux/amd64/kubectl`

`chmod +x ./kubectl`


`sudo mv ./kubectl /usr/local/bin/kubectl`


`kubectl version --client`


---------------------------------------

### 4 - INSTALAR AWS IAM AUTHENTICATOR:

https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

`curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.17.9/2020-08-04/bin/linux/amd64/aws-iam-authenticator`


`chmod +x ./aws-iam-authenticator`


`mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin`


`echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc`


`aws-iam-authenticator help`

-----------------------------------------
## EXTRAS:
-----------------------------------------

### UPDATEAR KUBE CONFIG:

`aws eks --region us-east-1 update-kubeconfig --name nombredelcluster`


`kubectl config use-context nombrecluster`

-----------------------------------------

### CHECK CONFIG:

`kubectl config get-contexts`

Para borrar un contexto viejo: `kubectl config unset contexts.arn:aws:eks:us-east-1:xxxxxxxxxxx:cluster/NOMBRE`

`kubectl config view`
/home/USUARIO/.kube


`aws configure list`
/home/USUARIO/.aws


-----------------------------------------

### CONFIGURAR KUBECTL CON PERMISOS DE WRITE: (para administrar usuarios)

`eksctl utils write-kubeconfig --cluster=stg`

luego agregar el usuario en el configmap:

`kubectl describe configmap -n kube-system aws-auth  (cambiar el describe por edit)`
