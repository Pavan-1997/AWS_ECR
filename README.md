# AWS ECR

AWS Elastic Container Registry (ECR) is a fully managed container image registry service provided by Amazon Web Services (AWS). It enables you to store, manage, and deploy container images (Docker images) securely, making it an essential component of your containerized application development workflow. ECR integrates seamlessly with other AWS services like Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS).

Elastic in AWS means highly scalable and available.

It is similar to other Container Registries as DockerHub, Quay.io, GCR, GHCR - these are used to store the Docker Images.

![image](https://github.com/Pavan-1997/AWS_ECR/assets/32020205/89c3b9c5-c0fe-41ed-b296-e27f8b355dcd)
     
 
## Key Benefits of ECR
- **Security**: ECR offers encryption at rest, and images are stored in private repositories by default, ensuring the security of your container images.
- **Integration**: ECR integrates smoothly with AWS services like ECS and EKS, simplifying the deployment process.
- **Scalability**: As a managed service, ECR automatically scales to meet the demands of your container image storage.
- **Availability**: ECR guarantees high availability, reducing the risk of image unavailability during critical times.
- **Lifecycle Policies**: You can define lifecycle policies to automate the cleanup of unused or old container images, helping you save on storage costs.

---
# Creating an ECR repo - Build Docker Image - Push Docker Image

1. Goto ECR from AWS Console


2. Click on Get started


3. As you can see by default the AWS ECR registry is set to Private


4. Give a Repository name -> Enable Tag immutability -> Enable Image scan settings -> Click on Create repository

![image](https://github.com/Pavan-1997/AWS_ECR/assets/32020205/b91671d7-c4c4-4ab9-a144-fefbafa550c5)

 
5. Now goto EC2 from AWS Console -> Click on Launch instance

   Give a name
   
   Use Ubuntu as an image
   
   Instance type as t2.micro
   
   Create a key pair if already present use existing one
   
   Click on Launch instance
   
   Connect to the instance


6. Install Docker in EC2
```
sudo apt update -y

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"

apt-cache policy docker-ce

sudo apt install docker-ce

sudo usermod -aG docker $USER

sudo systemctl status docker 
```
```
sudo reboot
```
`Reboot the instance for Ubuntu user to execute docker commands`


7. Install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

sudo apt install unzip

unzip awscliv2.zip

sudo ./aws/install

aws --version
```

`Enter your AWS Access Key ID, Secret Access Key, default region, and preferred output format (json) when prompted`


8. Build your Docker image locally using the `docker build` command:
```
docker build -t <your-image-name> <path-to-dockerfile>
```


9. Tag the image with your ECR repository URI:

```
docker tag <your-image-name>:<tag> <your-aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repository-name>:<tag>
```


10. Log in to your ECR registry using the AWS CLI:
```
aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-aws-account-id>.dkr.ecr.<your-region>.amazonaws.com
```


11. Push the Docker image to ECR:
```
docker push <your-aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repository-name>:<tag>
```
![image](https://github.com/Pavan-1997/AWS_ECR/assets/32020205/faa3c74f-57e8-4d84-b7c8-ee377075a70e)

![image](https://github.com/Pavan-1997/AWS_ECR/assets/32020205/1fa16a9c-3795-42b3-8c08-42e3b5b13ee5)


12. Pulling Docker Images from ECR
    To pull and use the Docker images from ECR on another system or AWS service, follow these step -> Log in to ECR using the AWS CLI as shown in Step 10. -> Pull the Docker image from ECR:
```
docker pull <your-aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/<your-repository-name>:<tag>
```

13. Cleaning Up Resources
  
    As good practice, clean up resources that you no longer need to avoid unnecessary costs. To delete an ECR repository:
  
   - Make sure there are no images in the repository, or delete the images using `docker rmi` locally.
   - Go to the AWS Management Console, navigate to the Amazon ECR service, and select your repository.
   - Click on "Delete" and confirm the action.
