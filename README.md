Certainly! Here's the revised step for creating an ECS Cluster, Service, and Task Definition using Fargate:

### 3. Create an ECS Cluster, Service, and Task Definition Using Fargate
1. **Create an ECS Cluster**:
   - Open the Amazon ECS console.
   - Click on "Create Cluster".
   - Choose "Networking only (Fargate)".
   - Configure your cluster and create it.

2. **Create a Task Definition**:
   - In the ECS console, click on "Task Definitions".
   - Click on "Create new Task Definition".
   - Select "Fargate" as the launch type.
   - Configure your task definition by adding the container details (e.g., image URI from ECR, memory, CPU, port mappings).
   - Create the task definition.

3. **Create an ECS Service**:
   - In the ECS console, go to your cluster.
   - Click on "Create" under "Services".
   - Choose "Fargate" as the launch type.
   - Configure the service (e.g., service name, number of tasks).
   - Select the task definition created earlier.
   - Create the service.

---

Here is the complete guide with the updated step:

### 1. Create an EC2 Instance and Install Docker
1. **Launch an EC2 Instance**:
   - Open the EC2 console.
   - Click on "Launch Instance".
   - Select an Amazon Machine Image (AMI) (e.g., Amazon Linux 2).
   - Choose an instance type (e.g., t2.micro for the free tier).
   - Configure instance details, add storage, and configure security group to allow necessary ports (e.g., SSH (22), HTTP (80), HTTPS (443)).
   - Review and launch the instance.

2. **Connect to the EC2 Instance**:
   - Use SSH to connect to your instance.
   ```bash
   ssh -i your-key-pair.pem ec2-user@your-ec2-public-ip
   ```

3. **Install Docker**:
   - Update the package index and install Docker.
   ```bash
   sudo yum update -y
   sudo amazon-linux-extras install docker
   sudo service docker start
   sudo usermod -a -G docker ec2-user
   ```

4. **Log out and log back in to apply Docker group membership**:
   ```bash
   exit
   ssh -i your-key-pair.pem ec2-user@your-ec2-public-ip
   ```

### 2. Containerize the Web Application and Push to ECR
1. **Create an ECR Repository**:
   - Open the Amazon ECR console.
   - Click on "Create repository".
   - Provide a name for your repository and create it.

2. **Create a Directory and Dockerfile**:
   - SSH into your EC2 instance.
   - Create a new directory for your web application and navigate to it.
   ```bash
   mkdir my-web-app
   cd my-web-app
   ```
   - Create a `Dockerfile` using `vi` or your preferred text editor.
   ```bash
   vi Dockerfile
   ```
   - Please see [this link](https://github.com/Guuruuu/Web-Application-Project/blob/main/Dockerfile) for Dockerfile details.

3. **Build and Tag Docker Image**:
   - Build and tag your Docker image.
   ```bash
   docker build -t your-app .
   docker tag your-app:latest your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repository-name:latest
   ```

4. **Run the Docker Container Locally**:
   - Use the `docker run` command to run the container on the EC2 instance to ensure it works correctly.
   ```bash
   docker run -d -p 80:80 your-app:latest
   ```
   - Verify the application is running by accessing the EC2 instance's public IP address in a web browser.
   ```bash
   http://your-ec2-public-ip
   ```

5. **Push the Docker Image to ECR**:
   - Authenticate Docker to your ECR registry.
   ```bash
   aws ecr get-login-password --region your-region | docker login --username AWS --password-stdin your-aws-account-id.dkr.ecr.your-region.amazonaws.com
   ```
   - Push the image to ECR.
   ```bash
   docker push your-aws-account-id.dkr.ecr.your-region.amazonaws.com/your-repository-name:latest
   ```

By following these steps, you will create a directory for your web application, write a Dockerfile, build and test your Docker container locally on the EC2 instance, and finally push the container image to Amazon ECR.

### 3. Create an ECS Cluster, Service, and Task Definition Using Fargate
1. **Create an ECS Cluster**:
   - Open the Amazon ECS console.
   - Click on "Create Cluster".
   - Choose "Networking only (Fargate)".
   - Configure your cluster and create it.

2. **Create a Task Definition**:
   - In the ECS console, click on "Task Definitions".
   - Click on "Create new Task Definition".
   - Select "Fargate" as the launch type.
   - Configure your task definition by adding the container details (e.g., image URI from ECR, memory, CPU, port mappings).
   - Create the task definition.

3. **Create an ECS Service**:
   - In the ECS console, go to your cluster.
   - Click on "Create" under "Services".
   - Choose "Fargate" as the launch type.
   - Configure the service (e.g., service name, number of tasks).
   - Select the task definition created earlier.
   - Create the service.

### 4. Create an Application Load Balancer
1. **Create a Load Balancer**:
   - Open the EC2 console and go to "Load Balancers".
   - Click on "Create Load Balancer".
   - Select "Application Load Balancer".
   - Configure the load balancer settings (e.g., name, scheme, listeners, availability zones).
   - Create a target group.

2. **Register Targets**:
   - Register the ECS instances in your target group.
   - Set up health checks.

3. **Configure Load Balancer with ECS**:
   - In the ECS console, update the ECS service to use the load balancer.
   - Specify the load balancer and target group in the service settings.

### 5. Verify the Deployment
1. **Get the DNS Name**:
   - Go to the EC2 console and find your load balancer.
   - Copy the DNS name of the load balancer.

2. **Access Your Application**:
   - Open a web browser and navigate to the load balancer’s DNS name.
   - Verify that your web application is accessible.

This guide covers the basic steps required to deploy a web application on Amazon ECS using the specified tools. Ensure that you have the necessary permissions and configurations set up in your AWS environment for seamless deployment.
