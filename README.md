# Deploy a Web Application on AWS with Docker, Load Balancer, Amazon ECR, and ECS. 

I demonstrated how to deploy a web application to Amazon Elastic Container Service (ECS) using various tools such as EC2, Docker, Elastic Container Registry (ECR), Application Load Balancer in AWS.

1. Create an EC2 instance and install Docker on it.
2. Containerize the web application by using Docker and pushing the container image to Amazon Elastic Container Registry (ECR).
3. Create an ECS cluster, ECS Service and Task Definition that specifies how the container should be run.
4. Create a load balancer that distributes traffic evenly across the instances in the ECS cluster.
5. Configure the load balancer to use the ECS task definition and set up health checks to ensure that traffic is only routed to healthy instances.
6. Confirm that the web application is properly deployed to Amazon ECS cluster via DNS name of the application load balancer. 
