# My-jenkins-docker-kubernetes-installation
1. First step
#install and configure docker on your jenkins ubuntu server
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker.service


2. second step
#install, configure, and start jenkins
sudo apt-get update
sudo apt install openjdk-11-jdk
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null  
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo usermod -aG docker jenkins
sudo systemctl restart docker.service
sudo echo "jenkins  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/jenkins

3. third step
   #install AWSCLI
sudo apt update -y
sudo apt install unzip wget -y
sudo curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
sudo apt install python3 -y
sudo apt install unzip python3 -y
sudo unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

4. FOURTH STEP
   #Install wget if not installed
   sudo apt install wget -y
sudo wget https://github.com/kubernetes/kops/releases/download/v1.22.0/kops-linux-amd64
sudo chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops

5. FIFTH STEP
   #Install kubectl
    sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 sudo chmod +x ./kubectl
 sudo mv ./kubectl /usr/local/bin/kubectl

6. SIXTH STEP
   #Create an IAM role from AWS console or CLI with the below policies
   AmazonEC2FullAccess 
AmazonS3FullAccess
IAMFullAccess 
AmazonVPCFullAccess

7. SEVENTH STEP
   #create an S3 bucket

8. EIGHT STEP
    #Add enviromental variable in bashrc
   vi .bashrc
export NAME=jenkinstechjourney.k8s.local
export KOPS_STATE_STORE=s3://mybucketlist.local

9. NITHS STEP
    #create ssh before configuring cluster
   ssh-keygen

10. TENTH STEP
    #Create kubernetes cluster defintion on s3 bucket
    kops create cluster --zones us-east-1a --networking weave --master-size t2.medium --master-count 1 --node-size t2.medium --node-count=2 ${NAME}
# copy the sshkey into your cluster to be able to access your kubernetes node from the kops server
kops create secret --name ${NAME} sshpublickey admin -i ~/.ssh/id_rsa.pub
   



