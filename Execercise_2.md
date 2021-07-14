# Excercise 2:

## This is about the install sample React app in the AWS EC2 instance

### Prerequisites

* Create an AWS account
* AWS CLI and AWS vault
* Docker and Dockerhub account
* Terraform
* Ansible
* Install sample React app

### Steps

1. Connect to the AWS account add the user
- Login to the AWS account, got to **Services**, then click on IAM and add a user and provide the neceesary access

2. Create the Key-pair for the server

3. Dockerize the app (Replce all variables with appropriate values)

- Dockerfile is already created for containerized the sample react-app

- Build a docker image from the Dockerfile

  ``` docker build -t <image-name> . ```
- Go to the terminal and add a tag to the image

  ``` docker tag <image-name> <docker_hub_username>/<repo-name>:<tag-name> ```
-  Login to the DockerHub Account (Good to use the private docker registry)

   ``` docker login ```
- Push the image to the DockerHub

  ``` docker tag <image-name> <docker_hub_username>/<repo-name>:<tag-name> ```

4. Provision the server using the terraform (Here we are creating the AWS instance using ubuntu server image)

- cd in to the **frontend** folder and initialize a working directory containing terraform configuration files

  ``` terraform init ``` 
- After that run following command to verify the setup (Enter the instance name, frontend)

  ``` terraform plan ```
- Spin up the EC2 instance

  ``` terraform apply ```
- Create a security group for the server (Add the inbound ports for allow the traffic to the server)

5. Setting up the Ansible in your machine

- Setting the Ansible configuration

   ``` sudo vim /home/<username>/.ansible.cfg ```

    Insert the following to the file

    ``` 
    [defaults]
    host_key_checking = False
    private_key_file=~/.ssh/<your_KeyPair>.pem
    inventory=/etc/ansible/hosts
    remote_user = ubuntu

    [ssh_connection]
    control_path=%(directory)s/%%h-%%r
    control_path_dir=~/.ansible/cp
    #pipelining = True
    scp_if_ssh = True
    ```

- Add host to the ansible host file

  ``` sudo vim /etc/ansible/hosts ```

- Insert the followings to the file 
 
  ``` [frontend] ```
- Test the connectivity to the server using the below command

  ``` sudo ansible all -m ping ```

- Install the application by running the ansible playbook

  ``` ansible-playbook provision_frontend.yaml ```
