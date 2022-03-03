# Monolith-App-Microservices

Hello and welcome to the _Monolith App Microservices_ project!

## Overview

In this exciting project, we will deploy a monolithic node.js application to a Docker container, and then decouple the application into microservices without any downtime. Next, the node.js application will host a simple message board with threads and messages between users.

As the name suggests, monolithic architectures are traditionally resistant to change and difficult to scale. The “mono” prefix of the word, represents the single codebase which contains all of the functionalities. However, as an application's code base grows, it will become increasingly difficult to manage.

![](./images/monolithic.png)

Conversely, microservices architectures have independently-run applications which use APIs communicate with other services. Each service performs a single function. Unlike the Monolith architecture, microservices are designed for failure. In theory, the failure of one microservice would not affect the whole system, other functionalities must remain accessible to the user. Additionally, microservices are capable of adopting a variety of technology stacks, which could be utilized for solving business challenges. This may include various frameworks and programming languages, which can be deployed independently, as a single service, or as a group of services.

![](./images/microservices.png)

--------------------------------------------------------------------------------
## PART 1: Containerize the Monolith

In this module, you will build the container image for your monolithic node.js application and push it to Amazon Elastic Container Registry. 

![https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_3-Image-Deployment-to-Amazon-ECR.ef4f8b89baccbd37380998a8d896126df5ed8a3b.png](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_3-Image-Deployment-to-Amazon-ECR.ef4f8b89baccbd37380998a8d896126df5ed8a3b.png)

## What Is a Container?

Containers allow you to easily package an application's code, configurations, and dependencies into easy to use building blocks that deliver environmental consistency, operational efficiency, developer productivity, and version control. Containers can help ensure that applications deploy quickly, reliably, and consistently regardless of deployment environment.

![https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_2-VM-vs-Containers.78f841efba175556d82f64d1779eb8b725de398d.png](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_2-VM-vs-Containers.78f841efba175556d82f64d1779eb8b725de398d.png)

## Why Use Containers?

**Speed:**  Launching a container with a new release of code can be done without significant deployment overhead. Operational speed is improved, because code built in a container on a developer’s local machine can be easily moved to a test server by simply moving the container. At build time, this container can be linked to other containers required to run the application stack.

**Dependency Control & Improved Pipeline:**  A Docker container image is a point in time capture of an application's code and dependencies. This allows an engineering organization to create a standard pipeline for the application life cycle. For example:

1. Developers build and run the container locally.
2. Continuous integration server runs the same container and executes integration tests against it to make sure it passes expectations.
3. The same container is shipped to a staging environment where its runtime behavior can be checked using load tests or manual QA.
4. The same container is shipped to production.

Being able to build, test, ship, and run the exact same container through all stages of the integration and deployment pipeline makes delivering a high quality, reliable application considerably easier.

**Density & Resource Efficiency:**  Containers facilitate enhanced resource efficiency by allowing multiple heterogeneous processes to run on a single system. Resource efficiency is a natural result of the isolation and allocation techniques that containers use. Containers can be restricted to consume certain amounts of a host's CPU and memory. By understanding what resources a container needs and what resources are available from the underlying host server, you can right-size the compute resources you use with smaller hosts or increase the density of processes running on a single large host, increasing availability and optimizing resource consumption.

**Flexibility:**  The flexibility of Docker containers is based on their portability, ease of deployment, and small size. In contrast to the installation and configuration required on a VM, packaging services inside of containers allows them to be easily moved between hosts, isolated from failure of other adjacent services, and protected from errant patches or software upgrades on the host system.

**Services Used:**

- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)

---

## Implementation Instructions

For the first part of this project, you will build the Docker container image for your monolithic node.js application and push it to Amazon Elastic Container Registry (Amazon ECR). 

In the next few steps, you are going to be using **Docker**, **Github**, **Amazon Elastic Container Service (Amazon ECS)**, and **Amazon ECR** to deploy code into containers. To complete these steps, ensure you have the following tools.

1. **Have an AWS account:** If you don't already have an account with AWS, you can [sign up here](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html). All the exercises in this tutorial are designed to be covered under ****[AWS Free Tier](https://aws.amazon.com/free/). **Note:** Some of the services you will be using may require your account to be active for more than 12 hours. If you experience difficulty with any services and have a newly created account, please wait a few hours and try again.
2. **Install Docker:** You will use Docker to build the image files that will run in your containers. Docker is an open source project. You can download it [for Mac](https://docs.docker.com/docker-for-mac/install/) or [for Windows](https://docs.docker.com/docker-for-windows/install/).
3. After Docker is installed, you can verify it is running by entering **d*ocker --version*** in the terminal. The version number should display, for example: 

```bash
*Docker version 19.03.5, build 633a0ea*
```

1. **Install the AWS CLI:**
    - You will use the AWS Command Line Interface (AWS CLI) to push the images to Amazon ECR. You can learn about and download AWS CLI [here](http://docs.aws.amazon.com/cli/latest/userguide/installing.html).
    - After AWS CLI is installed, verify it is running by entering *aws --version* in the terminal. The version number should display, for example:
        
        ```bash
        *aws-cli/2.4.15 Python/3.9.10 Darwin/19.6.0 source/x86_64 prompt/off*
        ```
        
    - If you already have AWS CLI installed, run the following command in the terminal to ensure it is updated to the latest version:
        
        ```bash
        *pip install awscli --upgrade --user*
        ```
        
    - If you have never used AWS CLI before, you may need to [configure your credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

2. **Have a text editor:** If you don't already have a text editor for coding, install one to your local environment. [Atom](https://atom.io/) is a simple, open-source text editor from GitHub that is popular with developers.

**Download the code from GitHub:** Navigate to [https://github.com/awslabs/amazon-ecs-nodejs-microservices](https://github.com/awslabs/amazon-ecs-nodejs-microservices) and select **Clone or Download** to download the GitHub repository to your local environment. You can also use [GitHub Desktop](https://desktop.github.com/) or [Git](https://git-scm.com/) to clone the repository.

**Open the project files:** Start **Atom**, select **Add Project Folder**, and select the folder where you saved the repository **amazon-ecs-nodejs-microservices.** This will add the entire project into Atom so you can easily work with it.In your project folder, you should see folders for **infrastructure** and **services**. Infrastructure holds the AWS CloudFormation infrastructure configuration code you will use in the next step. **Services** contains the code that forms the node.js application.Take a few minutes to review the files and familiarize yourself with the different aspects of the application, including the database **db.json**, the server **server.js**, **package.json**, and the application **Dockerfile**.

![https://d1.awsstatic.com/Developer%20Marketing/containers/1-2-Github-Project.67636425a75d8099678373871d425dccc89f9cfc.png](https://d1.awsstatic.com/Developer%20Marketing/containers/1-2-Github-Project.67636425a75d8099678373871d425dccc89f9cfc.png)

**Create the repository:**

- Navigate to the [Amazon ECR console](https://console.aws.amazon.com/ecs/home?#/repositories).
- On the **Repositories** page, select **Create Repository**.
- On the Create repository page, enter the following name your repository: *api* **Note:** Under **Tag immutability**, leave the default settings.
- Select **Create repository**.

After the repository is created, a confirmation message showing with the repository address. The repository address is in the following format: ***[account-ID]*.dkr.ecr.*[region]*.amazonaws.com/*[repo-name]***. The *[account-ID]*, *[region]*, and *[repo-name]* will be specific to your setup.

 **Note:** You will need the repository address throughout this tutorial.

![https://d1.awsstatic.com/Developer%20Marketing/containers/1-3-New-Repo.522f613f9043d718abc5e2e7b66054e6fcda4141.png](https://d1.awsstatic.com/Developer%20Marketing/containers/1-3-New-Repo.522f613f9043d718abc5e2e7b66054e6fcda4141.png)

Access your terminal and navigate to the following directory: *~/amazon-ecs-nodejs-microservices/2-containerized/services/api*.

**Use the terminal to authenticate Docker log in:**

1. Run 

```bash
docker login -u AWS -p $(aws ecr get-login-password --region the-region-you-are-in) xxxxxxxxx.dkr.ecr.the-region-you-are-in.amazonaws.com
```

If needed, [configure your credentials](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html).If the authentication was successful, you will receive the confirmation message: **Login Succeeded**.

1. To build the image, run the following command in the terminal: *docker build -t api* .
2. **Note:** **The period (**.**) after *api* is needed.
3. After the build completes, tag the image so you can push it to the repository: 
    
    ```bash
    *docker tag api:latest [account-ID].dkr.ecr.[region].amazonaws.com/api:v1* 
    ```
    
4. **Note:** Replace the *[account-ID]* and *[region]* placeholders with your specific information.**Pro tip:** The *:v1* represents the image build version. Every time you build the image, you should increment this version number. If you were using a script, you could use an automated number, such as a time stamp to tag the image. This is a best practice that allows you to easily revert to a previous container image build in the future.
5. Push the image to Amazon ECR by running: *docker push [account-id].dkr.ecr.[region].amazonaws.com/api:v1*  ****
6. **Note:** replace the [account-ID] and [region] placeholders with your specific information.

If you navigate to your Amazon ECR repository, you should see your image tagged *v1*.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m1s4-3-ecr-image-tag.1650b925b840d135c52f16de8f6f3161ebb061e7.png](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m1s4-3-ecr-image-tag.1650b925b840d135c52f16de8f6f3161ebb061e7.png)

--------------------------------------------------------------------------------
## PART 2: Deploy the Monolith

In this section, we will use Amazon Elastic Container Service (Amazon ECS) to instantiate a managed cluster of EC2 compute instances and deploy your image as a container running on the cluster. 

## Architecture Overview

![https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_4-Monolith-Architecture.4a066ba9d767e193cdc98c2a1735737de67ff3c4.png](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_4-Monolith-Architecture.4a066ba9d767e193cdc98c2a1735737de67ff3c4.png)

**a. Client:** The client makes a request over port 80 to the load balancer.

**b. Load Balancer:** The load balancer distributes requests across all available ports.

**c. Target Groups:** Instances are registered in the application's target group.

**d. Container Ports:** Each container runs a single application process which binds the node.js cluster parent to port 80 within its namespace.

**e. Containerized node.js Monolith:** The node.js cluster parent is responsible for distributing traffic to the workers within the monolithic application. This architecture is containerized, but still monolithic because each container has all the same features of the rest of the containers.

## What is Amazon Elastic Container Service?

Amazon Elastic Container Service (Amazon ECS) is a highly scalable, high performance container management service that supports Docker containers and allows you to easily run applications on a managed cluster of Amazon EC2 instances. With simple API calls, you can launch and stop Docker-enabled applications, query the complete state of your cluster, and access many familiar features like security groups, Elastic Load Balancing, EBS volumes, and IAM roles.

You can use Amazon ECS to schedule the placement of containers across your cluster based on your resource needs and availability requirements. You can also integrate your own scheduler or third-party schedulers to meet business or application specific requirements.

There is no additional charge for Amazon ECS. You pay for the AWS resources (for example, EC2 instances or EBS volumes) you create to store and run your application.

**Services Used:**

- [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)
- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)

---

## Implementation Instructions

Follow the step-by-step instructions below to deploy the node.js application using Amazon ECS. Select each step number to expand the section.

Create an Amazon ECS cluster deployed behind an Application Load Balancer.

1. Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home).
2. Select **Create stack**.
3. Select **Upload a template file** and choose the [ecs.yml](https://github.com/awslabs/amazon-ecs-nodejs-microservices/blob/master/2-containerized/infrastructure/ecs.yml) file from the GitHub project at *amazon-ecs-nodejs-microservice/2-containerized/infrastructure/ecs.yml* then select **Next**.
4. For the stack name, enter *BreakTheMonolith-Demo*. Verify that the other parameters have the following values:
    1. Desired Capacity = *2*
    2. InstanceType = *t2.micro*
    3. MaxSize = *2*
5. Select **Next***.*
6. On the **Configure stack options** page, keep the default options and scroll down and select **Next**.
7. On the **Review BreakTheMonolith-Demo** page scroll to the bottom of the page, acknowledge the **Capabilities** statement by selecting the checkbox, and select **Create stack**.

You will see your stack with the status CREATE_IN_PROGRESS. You can select the refresh button at the top right of the screen to check on the progress. This process typically takes under 5 minutes.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s1-7-cfn-create-in-progress.f5bc670617b48e28000c6a13dc69c73ead7a996a.png](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s1-7-cfn-create-in-progress.f5bc670617b48e28000c6a13dc69c73ead7a996a.png)

**NOTE:** Optionally, you can use the AWS Command Line Interface (AWS CLI) to deploy AWS CloudFormation stacks. Run the following code in the terminal from the folder **amazon-ecs-nodejs-microservices/3-microservices** and replace *[region]* with your AWS Region.

```bash
$ aws cloudformation deploy \
   --template-file infrastructure/ecs.yml \
   --region [region] \
   --stack-name BreakTheMonolith-Demo \
   --capabilities CAPABILITY_NAMED_IAM
```

**Step 2. Check your Cluster is Running**

Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home?).Your cluster should appear in the list.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2-ecs-verify-cluster.75798ae4957efb0ac054357414b00c6ce8259052.jpg](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2-ecs-verify-cluster.75798ae4957efb0ac054357414b00c6ce8259052.jpg)

- Select the cluster **BreakTheMonolith-Demo**, then select the **Tasks** tab to verify that there are no tasks running.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2b-ecs-cluster-tasks.0d3cb4fc039943522ce656295d3d354b497217ed.jpg](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2b-ecs-cluster-tasks.0d3cb4fc039943522ce656295d3d354b497217ed.jpg)

- Select the **ECS Instances** tab to verify there are two Amazon EC2 instances created by the AWS CloudFormation template.**⚐ Note:** If you receive a message that the ECS agent is outdated, select **Learn more** for instructions to update the ECS agent.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2c-ecs-cluster-instances.c4eeb487eb224ef2cd8d2c6e87df1242b1be8f2c.jpg](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s2c-ecs-cluster-instances.c4eeb487eb224ef2cd8d2c6e87df1242b1be8f2c.jpg)

Task definitions specify how Amazon ECS deploys the application containers across the cluster.

- From the **Amazon ECS** left navigation menu, select **Task Definitions**.
- Select **Create new Task Definition**.
- On the **Select launch type compatibility** page, select the **EC2** option then select **Next step**.
- On the **Configure task and container definitions** page, do the following:
    - In the **Task Definition Name** field, enter *api*.
    - Scroll down to **Container Definitions** and select **Add container**.
    - In the **Add container** window:
        - Parameters that are not defined can be either left blank or with the default settings.
        - In the **Container name** field, enter *api*.
        - In the **Image** field, enter *[account-ID].dkr.ecr.[region].amazonaws.com/api:v1*Replace *[account-ID]* and *[region]* with your specific information. Ensure the tag *v1* matches the value you used in Module 1 to tag and push the image. This is the URL of your ECR repository image that was created in the previous module.
        - In the **Memory Limits** field, verify **Hard limit** is selected and enter *256* as the value.
        - Under **Port mappings**, Host port = *0* and Container port = *3000*.
        - Scroll to **ENVIRONMENT**, CPU units = *256*.
- Select **Add**.You will return to the **Configure task and container definitions** page.
- Scroll to the bottom of the page and select **Create**.

Your Task Definition is listed in the console.

![https://d1.awsstatic.com/Developer%20Marketing/containers/2-3-Task-Definition.6845e3a07fc658a7d67422ea45cbed35549ee6ed.png](https://d1.awsstatic.com/Developer%20Marketing/containers/2-3-Task-Definition.6845e3a07fc658a7d67422ea45cbed35549ee6ed.png)

The [Application Load Balancer (ALB)](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html) lets your service accept incoming traffic. The ALB automatically routes traffic to container instances running on your cluster using them as a [target group](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-target-groups.html).

**Check your VPC Name:** If this is not your first time using this AWS account, you may have multiple VPCs. It is important to configure your Target Group with the correct VPC.

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Locate the Load Balancer named **demo**.
- Select the checkbox next to **demo** to see the Load Balancer details.
- In the **Description** tab, locate the **VPC** attribute (in this format: vpc-xxxxxxxxxxxxxxxxx). **Note:** You will need the VPC attribute in the next step when you configure the ALB target group.

![https://d1.awsstatic.com/Developer%20Marketing/containers/2-4-VPC.04939ba459c7e1aaafd562f8cc85b992b9fa60c8.png](https://d1.awsstatic.com/Developer%20Marketing/containers/2-4-VPC.04939ba459c7e1aaafd562f8cc85b992b9fa60c8.png)

**Configure the ALB Target Group**

- Navigate to the [Target Group section of the EC2 Console.](https://console.aws.amazon.com/ec2/v2/home?#TargetGroups:)
- Select **Create target group**.
- Configure the following Target Group parameters (for the parameters not listed below, keep the default values):
    - For the **Target group name**, enter *api*.
    - For the **Protocol**, select **HTTP**.
    - For the **Port**, enter *80*.
    - For the VPC, select the value that matches the one from the Load Balancer description. .
        
        This is most likely NOT your default VPC
        
    - Access the **Advanced health check settings** and edit the following parameters as needed:
        - For **Healthy threshold**, enter *2*.
        - For **Unhealthy threshold**, enter *2*.
        - For **Timeout**, enter *5*.
        - For **Interval**, enter *6*.
- Select **Create**.

![https://d1.awsstatic.com/Developer%20Marketing/containers/2-4-Target-Group.f948d3020af5b18b85fe1625a55a73c58447fd3e.png](https://d1.awsstatic.com/Developer%20Marketing/containers/2-4-Target-Group.f948d3020af5b18b85fe1625a55a73c58447fd3e.png)

The ALB [listener](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html) checks for incoming connection requests to your ALB.

**Add a Listener to the ALB**

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Select the checkbox next to **demo** to see the Load Balancer details.
- Select the **Listeners** tab.
- Select **Add listener** and edit the following parameters as needed:
    - For **Protocol:port**, select **HTTP** and enter *80*.
    - For **Default action(s)**, select **Forward to** and in the **Target group** field, enter *api*.
- Select **Save**.

![https://d1.awsstatic.com/Developer%20Marketing/containers/2-5-Listener.d24b3ec557dc6b6f436ca815f826134775f2f81a.png](https://d1.awsstatic.com/Developer%20Marketing/containers/2-5-Listener.d24b3ec557dc6b6f436ca815f826134775f2f81a.png)

Deploy the monolith as a service into the cluster.

- Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home?) and select **Clusters** from the left menu bar.
- Select the cluster **BreakTheMonolith-Demo**, select the **Services** tab then select **Create**.
- On the **Configure service** page, edit the following parameters (and keep the default values for parameters not listed below):
    - For the **Launch type**, select **EC2**.
    - For the **Service name**, enter *api*.
    - For the **Number of tasks**, enter *1*.
    - Select **Next step**.
- On the **Configure network** page, **Load balancing** section, select **Application Load Balancer**.Additional parameters will apear: **Service IAM role** and **Load balancer name**.
    - For the **Service IAM role**, select BreakTheMonolith-Demo-ECSServiceRole.
    - For the **Load balancer name**, verify that **demo** is selected.
- In the **Container to load balance** section, select **Add to load balancer**.Additional information labeled **api:3000** is shown.
- In the **api:3000** section, do the following:
    - For the **Production listener port** field, select **80:HTTP**.
    - For the **Target group name**, select your group: **api**.
    - Select **Next step**.
- On the **Set Auto Scaling** page, leave the default setting and select **Next step**.
- On the **Review** page, review the settings then select **Create Service**.
- After the service has been created, select **View Service**.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s6-ecs-service-api.120051323a137b3e5327e4950796391c6a21b006.jpg](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m2s6-ecs-service-api.120051323a137b3e5327e4950796391c6a21b006.jpg)

Nice work! You now have a running service. It may take a minute for the container to register as healthy and begin receiving traffic.

Validate your deployment by checking if the service is available from the internet and pinging it.

**To Find your Service URL:**

- Navigate to the [Load Balancers](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:) section of the EC2 Console.
- Select your load balancer **demo**.
- In the **Description** tab, copy the DNS name and paste into a new browser tab or window.
- You should see the message **Ready to receive requests**.

**See Each Part of the Service:** The node.js application routes traffic to each worker based on the URL. To see a worker, simply add the worker name *api/[worker-name]* to the end of the DNS Name as follows:

- http://*[DNS name]*/api/users
- http://*[DNS name]*/api/threads
- http://*[DNS name]*/api/posts

You can also add a record number at the end of the URL to drill down to a particular record. For example: *http://[DNS name]/api/posts/1* or *http://[DNS name]/api/users/2*

![https://d1.awsstatic.com/Developer%20Marketing/containers/2.7-users.dd8ff14d7dc5349272d3c9eed4d22017fcc46a05.png](https://d1.awsstatic.com/Developer%20Marketing/containers/2.7-users.dd8ff14d7dc5349272d3c9eed4d22017fcc46a05.png)

--------------------------------------------------------------------------------
## PART 3: Break the Monolith

In this module, you will break the node.js application into several interconnected services and push each service's image to an Amazon Elastic Container Registry (Amazon ECR) repository. 

## Architecture Overview

The final application architecture uses Amazon Elastic Container Service (Amazon ECS) and the Application Load Balancer (ALB).

![https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_5-Microservices-app-architecture.a0103d1b39c5702fb94cfa20ec7fa29f1bb75e1f.png](https://d1.awsstatic.com/Developer%20Marketing/containers/monolith_5-Microservices-app-architecture.a0103d1b39c5702fb94cfa20ec7fa29f1bb75e1f.png)

**a. Client**The client makes traffic requests over port 80.

**b. Load Balancer** The ALB routes external traffic to the correct service. The ALB inspects the client request and uses the routing rules to direct the request to an instance and port for the target group matching the rule.

**c. Target Groups** Each service has a target group that keeps track of the instances and ports of each container running for that service.

**d. Microservices** Amazon ECS deploys each service into a container across an EC2 cluster. Each container only handles a single feature.

## Why Microservices?

**Isolation of Crashes** Even the best engineering organizations can and do have fatal crashes in production. In addition to following all the standard best practices for handling crashes gracefully, one approach that can limit the impact of such crashes is building microservices. Good microservice architecture means that if one micro piece of your service is crashing, then only that part of your service will go down. The rest of your service can continue to work properly.

**Isolation for Security** In a monolithic application if one feature of the application has a security breach, for example a vulnerability that allows remote code execution, then you must assume that an attacker could have gained access to every other feature of the system as well. This can be dangerous if, for example, your avatar upload feature has a security issue which ends up compromising your database with user passwords. Separating features into microservices using Amazon ECS allows you to secure access to AWS resources by giving each service its own AWS Identity and Access Management (IAM) role. When microservice best practices are followed, the result is that if an attacker compromises one service, they only gain access to the resources of that service, and cannot horizontally access other resources from other services without breaking into those services as well.

**Independent Scaling** When features are broken out into microservices, then the amount of infrastructure and number of instances used by each microservice class can be scaled up and down independently. This makes it easier to measure the cost of a particular feature and identify features that may need to be optimized first. If one particular feature is having issues with its resource needs, other features will not be impacted and reliable performance can be maintained.

**Development Velocity** Microservices lower the risks in development, which can enable a team to build faster. In a monolith, adding a new feature can potentially impact every other feature that the monolith contains. Developers must carefully consider the impact of any code they add, and ensure that they do not break anything. On the other hand, a proper microservice architecture has new code for a new feature going into a new service. Developers can be confident that any code they write will actually not be able to impact the existing code at all unless they explicitly write a connection between two microservices.

**Services Used:**

- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)

---

## Implementation Instructions

Follow the step-by-step instructions below to break the monolith. Select each step number to expand the section.

In the previous two modules, you deployed your application as a monolith using a single service and a single container image repository. To deploy the application as three microservices, you will need to provision three repositories (one for each service) in Amazon ECR.

Our three services are:

1. users
2. threads
3. posts

Create the three repositories by repeating the following steps for each service:

- Navigate to the [Amazon ECR console](https://console.aws.amazon.com/ecs/home?#/repositories).
- Select **Create repository**.
- In the **Create repository** page, **Repository name** field, create a repository for the service (posts, threads, or users).
- **Note:** For the **Tag immuntability** options, keep the default settings.

You should have four repositories in Amazon ECR. Record the repository information for each microservice that you created. This information will be needed in an upcoming step. The information you need is in the following format:*[account-id].dkr.ecr.[region].amazonaws.com/[service-name]*

![https://d1.awsstatic.com/Developer%20Marketing/containers/3-1-Repositories.84d4925daa34fd283cfe9b289941f2610ac4f87e.png](https://d1.awsstatic.com/Developer%20Marketing/containers/3-1-Repositories.84d4925daa34fd283cfe9b289941f2610ac4f87e.png)

You will need access to Docker to build and push the images for each service. If you are working on this project at different points in time, you may have been logged out of Docker. If this is the case, take the following steps to log into Docker again.

- Run *$(aws ecr get-login --no-include-email --region [your-region])*Replace *[your-region]*, for example: *$(aws ecr get-login --no-include-email --region us-west-2)*

If the authentication was successful, you will receive the confirmation message: **Login Succeeded**.

In the project folder **amazon-ecs-nodejs-microservices/3-microservices/services**, you will have folders with files for each service. Notice how each microservice is essentially a clone of the previous monolithic service.

You can see how each service is now specialized by comparing the file **db.json** in each service and in the monolithic api service. Previously posts, threads, and users were all stored in a single database file. Now, each is stored in the database file for its respective service.

Open your terminal, and set your path to *~/amazon-ecs-nodejs-microservices/3-microservices/services*

**Build and Tag Each Image**

- In the terminal, run *docker build -t [service-name] ./[service-name]*Replace the *[service-name]*, for example: *docker build -t posts ./posts*
- After the build completes, tag the image so you can push it to the repository:*docker tag [service-name]:latest [account-ID].dkr.ecr.[region].amazonaws.com/[service-name]:v1*Replace *[service-name]*, *[account-ID]*, and *[region]*, for example: *docker tag posts:latest [account-id].dkr.ecr.us-west-2.amazonaws.com/posts:v1*
- Push your image to ECR: docker push [account-id].dkr.ecr.[region].amazonaws.com/[service-name]:v1Replace *[service-name]*, *[account-ID]*, and *[region]*.

If you navigate to your ECR repository, you should see your images tagged with v1.

Repeat these steps for each microservice image.

**NOTE:** Be sure to build and tag all three images.

--------------------------------------------------------------------------------
## PART 4: Deploy Microservices

In this module, you will deploy your node.js application as a set of interconnected services behind an Application Load Balancer (ALB). Then, you will use the ALB to seamlessly shift traffic from the monolith to the microservices. 

## Architecture Overview

This is the process that you will follow to deploy the microservices and safely transition the application's traffic away from the monolith.

![https://d1.awsstatic.com/Developer%20Marketing/containers/5786%20Monolith%20Containers_6-switching-traffic.d54c5d1d16f4e86f2a712f862988fa05aa885b22.png](https://d1.awsstatic.com/Developer%20Marketing/containers/5786%20Monolith%20Containers_6-switching-traffic.d54c5d1d16f4e86f2a712f862988fa05aa885b22.png)

1. **Switch the Traffic** This is the starting configuration. The monolithic node.js app running in a container on Amazon ECS.
2. **Start Microservices** Using the three container images you built and pushed to Amazon ECR in the previous module, you will start up three microservices on your existing Amazon ECS cluster.
3. **Configure the Target Groups** Like in Module 2, you will add a target group for each service and update the ALB Rules to connect the new microservices.
4. **Shut Down the Monolith** By changing one rule in the ALB, you will start routing traffic to the running microservices. After traffic reroute has been verified, shut down the monolith.

**Services Used:**

- [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)
- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)

---

## Implementation Instructions

Follow the step-by-step instructions below to deploy the microservices. Select each step number to expand the section.

You will deploy three new services to the cluster that you launched in Module 2. Like Module 2, you will write Task Definitions for each service.

**NOTE**: It is possible to add multiple containers to a single task definition. This means you could run all three microservices as different containers from a single service. However, this approach is still monolithic since each container would need to scale linearly with the service. Your goal is to have three independent services. Each service requires its own task definition running a container with the image for that respective service.

You can either create these task definitions from the Amazon ECS console, or speed things up by writing them as JSON. To write the task definition as a JSON file, follow these steps:

1. From the [Amazon Container Services console](https://console.aws.amazon.com/ecs/home/), under **Amazon ECS**, select **Task definitions**.
2. In the **Task Definitions** page, select the **Create new Task Definition** button.
3. In the **Select launch type compatibility** page, select the **EC2** option and then select **Next step**.
4. In the Configure task and container definitions page, scroll to the **Volumes** section and select the **Configure via JSON** button.
5. Copy and paste the following code snippet into the JSON field, replacing the existing code.Remember to replace the [service-name], [account-ID], [region], and [tag] placeholders.

**Note:** The following parameters are used for the task definition:

- Name = *[service-name: posts, threads, and users]*
- Image = *[Amazon ECR repository image URL]:latest*
- cpu = *256*
- memory = *256*
- Container Port = *3000*
- Host Post = *0*

```bash
{
    "containerDefinitions": [
        {
            "name": "[service-name]",
            "image": "[account-id].dkr.ecr.[region].amazonaws.com/[service-name]:[tag]",
            "memoryReservation": "256",
            "cpu": "256",
            "essential": true,
            "portMappings": [
                {
                    "hostPort": "0",
                    "containerPort": "3000",
                    "protocol": "tcp"
                }
            ]
        }
    ],
    "volumes": [],
    "networkMode": "bridge",
    "placementConstraints": [],
    "family": "[service-name]"
}
```

Repeat the steps to create a task definition for each service:

- posts
- threads
- users

As in Module 2, configure a target group for each service (posts, threads, and users). A target group allows traffic to correctly reach a specified service. You will configure the target groups using AWS CLI. However, before proceeding, ensure you have the correct VPC name that is being used for this project:

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Select the checkbox next to demo, select the **Description** tab, and locate the **VPC** attribute (in this format: vpc-xxxxxxxxxxxxxxxxx). **Note:** You will need the VPC attribute when you configure the target groups.

**Configure the Target Groups**

In your terminal, enter the following command to create a target group for each service (posts, threads, and users). In addition, you will create a target group (drop-traffic) to keep traffic from reaching your monolith after your microservices are fully running. Remember to replace the following placeholders: [region], [service-name], and [vpc-attribute].

Service names: **posts**, **threads**, **users**, and **drop-traffic**

```bash
aws elbv2 create-target-group --region [region] --name [service-name] --protocol HTTP --port 80 --vpc-id [vpc-attribute] --healthy-threshold-count 2 --unhealthy-threshold-count 2 --health-check-timeout-seconds 5 --health-check-interval-seconds 6
```

![https://d1.awsstatic.com/Developer%20Marketing/containers/4.2-targets.e39a29c6ca46d303d398dc08ecb883aab0d08aa6.png](https://d1.awsstatic.com/Developer%20Marketing/containers/4.2-targets.e39a29c6ca46d303d398dc08ecb883aab0d08aa6.png)

The [listener](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html) checks for incoming connection requests to your ALB in order to route traffic appropriately.

Right now, all four of your services (monolith and your three microservices) are running behind the same load balancer. To make the transition from monolith to microservices, you will start routing traffic to your microservices and stop routing traffic to your monolith.

**Access the listener rules**

- Navigate to the [Load Balancer section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Locate the Load Balancer named **demo** and select the checkbox next to it to see the Load Balancer details.
- Select the **Listeners** tab.

**Update Listener Rules**

There should only be one listener listed in this tab. Take the following steps to edit the listener rules:

- Under the **Rules** column, select **View/edit rules**.
- On the **Rules** page, select the plus (**+**) button.The option to **Insert Rule** appears on the page.
- Use the following rule template to insert the necessary rules which include one to maintain traffic to the monolith and one for each microservice:
    - IF Path = /api/[service-name]* THEN Forward to [service-name]For example: IF Path = /api/posts* THEN Forward to posts
    - Insert the rules in the following order:
        - api: */api** forwards to *api*
        - users: */api/users** forwards to *users*
        - threads: */api/threads** forwards to *threads*
        - posts: */api/posts** forwards to *posts*
- Select **Save**.
- Select the back arrow at the top left corner of the page to return to the load balancer console.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m3s3-config-listeners.3509f544d65c0e98f9e8747c98e7fe931773cf83.png](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m3s3-config-listeners.3509f544d65c0e98f9e8747c98e7fe931773cf83.png)

Deploy the three microservices (posts, threads, and users) to your cluster. Repeat these steps for each of your three microservices:

- Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home) and select **Clusters** from the left menu bar.
- Select the cluster **BreakTheMonolith-Demo**, select the **Services** tab then select **Create**.
- On the **Configure service** page, edit the following parameters (and keep the default values for parameters not listed below):
    - For the **Launch type**, select **EC2**.
    - For the **Task Definition**, select the **Enter a value** button to automatically select the highest revision value.For example: api:1
    - For the **Service name**, enter a service name (*posts, threads, or users*).
    - For the **Number of tasks**, enter *1*
- Select **Next step**.
- On the **Configure network** page, **Load balancing** section, do the following:
    - For the **Load balancer type**, select **Application Load Balancer**.
    - For the **Service IAM role**, select **BreakTheMonolith-Demo-ECSServiceRole**.
    - For the **Load balancer name**, verify that **demo** is selected.
    - In the **Container to load balance** section, select the **Add to load balancer** button and make the following edits:
        - For the **Production listener port**, set to **80:HTTP**.
        - For the **Target group name**, select the appropriate group: *(***posts**, **threads**, or **users**)
- Select **Next step**.
- On the **Set Auto Scaling** page, select **Next step**.
- On the **Review** page, select **Create Service**.
- Select **View Service**.

It should only take a few seconds for all your services to start. Double check that all services and tasks are running and active before you proceed.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m4s4-deploy-microservices.2509306b0752f2c8fca45c51aa6be5773a65367e.png](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m4s4-deploy-microservices.2509306b0752f2c8fca45c51aa6be5773a65367e.png)

Your microservices are now running, but all traffic is still flowing to your monolith service. To reroute traffic to the microservices, take the following steps to update the listener rules:

- Navigate to the [Load Balancers section of the EC2 Console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Select the checkbox next to **demo** to see the Load Balancer details.
- Select the **Listeners** tab.There should only be one listener listed.
- Under the **Rules** column, select **View/edit rules**.
- On the **Rules** page, select the minus (****) button from the top menu.
- Delete the first rule (*/api* forwards to api*) by selecting the checkbox next to the rule.
- Select **Delete**.
- Update the default rule to forward to drop-traffic:
    - Select the edit (pencil) button from the top menu.
    - Select the edit (pencil) icon next to the default rule (**HTTP 80: default action**).
    - Selec the edit (pencil) icon in the **THEN** column to edit the **Forward to**.
    - In the **Target group** field, select **drop-traffic**.
    - Select the **Update** button.

See the following screenshot for an example of the updated rules.

![https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m3s3-drop-traffic.0747eaf7bc383d0c5eaa5be72d63d65efcbbb3e6.png](https://d1.awsstatic.com/Getting%20Started/break-a-monolith-app-into-microservices/m3s3-drop-traffic.0747eaf7bc383d0c5eaa5be72d63d65efcbbb3e6.png)

**Disable the monolith:** With traffic now flowing to your microservices, you can disable the monolith service.

- Navigate back to the Amazon ECS cluster **BreakTheMonolith-Demo-ECSCluster**.
- In the **Services** tab, select the checkbox next to **api** and select **Update**.
- On the **Configure service** page, locate **Number of tasks** and enter *0*.
- Select **Skip to review**.
- Select **Update Service**.

Amazon ECS will now empty connections from containers the service has deployed on the cluster then stop the containers. If you refresh the Deployments or Tasks lists after about 30 seconds, you will see that the number of tasks will drop to 0. The service is still active, so if you needed to roll back for any reason, you could simply update it to deploy more tasks.

Optionally, you can delete the api service. In the **Services** tab, select the checkbox next to **api**, select **Delete**, and confirm the deletion.

**You have now fully transitioned your node.js from the monolith to microservices, without any downtime!**

**Find your service URL**: This is the same URL that you used in Module 2 of this tutorial.

- Navigate to the [Load Balancers section of the EC2 console](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:).
- Select the checkbox next to **demo** to see the Load Balancer details.
- In the **Description** tab, locate the **DNS name** and select the copy icon at the end of the URL.
- Paste the DNS name into a new browser tab or window.

You should see a message 'Ready to receive requests'.

**See the values for each microservice:** Your ALB routes traffic based on the request URL. To see each service, simply add the service name to the end of your DNS name:

- http://[DNS name]/api/users
- http://[DNS name]/api/threads
- http://[DNS name]/api/posts

![https://d1.awsstatic.com/Developer%20Marketing/containers/4.6-threads.cc39d9da1c6cd1be0cd830dfe355afbde9503f8c.png](https://d1.awsstatic.com/Developer%20Marketing/containers/4.6-threads.cc39d9da1c6cd1be0cd830dfe355afbde9503f8c.png)

**NOTE:** These URLs perform exactly **the same as when the monolith is deployed. This is very important because any APIs or consumers that would expect to connect to this app will not be affected by the changes you made. Going from monolith to microservices required no changes to other parts of your infrastructure.

You can also use tools such as [Postman](https://www.getpostman.com/) for testing your APIs.

--------------------------------------------------------------------------------

## PART 5: Clean Up

In this module, you will terminate the resources you created during this tutorial. You will stop the services running on Amazon ECS, delete the ALB, and delete the AWS CloudFormation stack to terminate the ECS cluster, including all underlying EC2 instances.

Cleaning up is not required, but will help you avoid ongoing charges for keeping these services running.

**Services Used:**

- [Amazon Elastic Container Service](https://aws.amazon.com/ecs/)
- [Amazon Elastic Container Registry](https://aws.amazon.com/ecr/)
- [Amazon EC2](https://aws.amazon.com/ec2/)
- [Amazon VPC](https://aws.amazon.com/vpc/)
- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/)

---

## Clean Up Instructions

Follow the instructions below to delete the AWS resources you created in each module.

Start clean up by deleting each of the services (posts, threads, abd users) that are running in your cluster:

- Navigate to the [Amazon ECS console](https://console.aws.amazon.com/ecs/home?) and select **Clusters**.
- Select the **BreakTheMonolith-Demo** cluster.
- In the **Services** tab, select a service and then select **Delete**.
- Confirm the deletion.
- Repeat the steps until all the services are deleted.

Before proceeding to the next step, you nned to either wait for all running tasks to terminate or select the **Tasks** tab and select **Stop All**.

Repeat these steps for each of your services on the cluster.

Navigate to the [Load Balancer section of the EC2 Console.](https://console.aws.amazon.com/ec2/v2/home?#LoadBalancers:)

- Select the checkbox next to **demo** and select the **Listeners** tab.
- Select the listener, then select **Delete**.
- Confirm the deletion.Navigate to [Target Groups](https://console.aws.amazon.com/ec2/v2/home?#TargetGroups:) in the EC2 console.Check the checkbox at the top of the list (next to **Name**) to select all target groups.Select **Actions** then select **Delete**.Confirm the deletion.

![https://d1.awsstatic.com/Developer%20Marketing/containers/5-2-Delete-Listeners.62c05114c03193a4047d1190b96c410b06f5e49b.png](https://d1.awsstatic.com/Developer%20Marketing/containers/5-2-Delete-Listeners.62c05114c03193a4047d1190b96c410b06f5e49b.png)

![https://d1.awsstatic.com/Developer%20Marketing/containers/5.3-targets.cc1de9ff2babfbecde6f15fb273949ecfc6c4cb7.png](https://d1.awsstatic.com/Developer%20Marketing/containers/5.3-targets.cc1de9ff2babfbecde6f15fb273949ecfc6c4cb7.png)

Navigate to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home?).

- Check the box next to the Cloudformation stack **BreakTheMonolith-Demo**.
- Select **Actions** then select **Delete Stack**.
- Confirm the deletion.
- The stack status should change to DELETE_IN_PROGRESS*.*

**⚠** **WARNING!** Leaving a stack running will result in charges on your AWS account.

![https://d1.awsstatic.com/Developer%20Marketing/containers/5.4-stacks.ef22ce8a9155389d2f2f00dd3841337c756eb50d.png](https://d1.awsstatic.com/Developer%20Marketing/containers/5.4-stacks.ef22ce8a9155389d2f2f00dd3841337c756eb50d.png)

Navigate to [Task Definitions](https://console.aws.amazon.com/ecs/home?#/taskDefinitions) in the Amazon ECR console.

- Select a task definition (api, posts, threads, or users).
- On the **Task Definition Name** page, select the checkbox next to the task name.
- Select **Actions** then from the drop-down list select **Deregister**.
- Confirm your action.

Repeat these steps for all four task definitions.

Navigate to [Repositories](https://console.aws.amazon.com/ecs/home?#/repositories) in the Amazon ECR console.

- Select the checkbox next to a repository and then select **Delete**.
- Confirm the deletion.
- Repeat the steps until all the repositories are deleted.

****Congratulations:****

# You Just Broke the Monolith! Well done.