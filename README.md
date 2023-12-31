# OLA2

## Group Members

- Kasper Dahl, <cph-kd131@cphbusiness.dk>
- Mads Kristensen, <cph-mk613@cphbusiness.dk>
- Maria McNally, <cph-mm620@cphbusiness.dk>
- Mathias Jensen, <cph-mj839@cphbusiness.dk>

## Table of Contents

- [Group Members](#group-members)
- [Questions](#questions)
  - [1)](#1)
  - [2)](#2)
  - [3)](#3)
  - [4)](#4)
  - [5)](#5)
  - [6)](#6)
  - [7)](#7)
  - [8)](#8)
  - [9)](#9)
  - [10)](#10)
  - [11)](#11)
  - [12)](#12)
  - [13)](#13)

## Questions

### 1)

**What is a ’Layered’ Architecture?**

A "Layered" architecture is a design pattern commonly used in software development to structure and organize the components of a system into different layers, each with a specific set of responsibilities. These layers are typically designed to interact with one another in a well-defined and controlled manner. The layered architecture is often associated with the concept of "separation of concerns," where each layer handles a specific aspect of the application, making the system more modular, maintainable, and easier to develop.

**What is the difference between an open and a closed layer?**

In a layered architecture, there are generally two types of layers: open and closed layers:

- Open Layer:

  - An open layer is a layer that allows interaction from both the layers above and below it.
  - The features and functionality of an open layer is exposed to other layers and they are able to use these
  - Usually open layers are used for communication and data flow between the various parts of the system.

- Closed Layers:
  - A closed layer is a layer that restricts direct interaction from other layers except it's closest neighbors.
  - Functionality is restricted and requires access in order to be used.
  - Closed layers are designed to provide isolation to its services.

**Provide an example of why closed layers may cause a problem.**

Closed layers are great in terms of maintaining the integrity of a system and to ensure that each layer's responsibilities are isolated. There are however challenges such as:

- Increased complexity: Closed layers requires additional abstractions and interfaces in order to communicate between layers. This can make the system harder to understand and maintain.
- Performance overhead: When enforcing controlled access between the layers it requires additional data transformation, processing and maybe even network calls.
- Rigidity: They make the system harder to adapt and make changes in. This is due to the fact that changing one thing in one layer may require updates in several other layers aswell. This is very time consuming and could cause multiple errors.
- Debugging and troubleshooting: It can be harder to debug due to layers being abstracted and hidden.

### 2)

**Testability is often given as an advantage of layered architectures. How does a layered architecture make testing easier and more structured?**

A layered architecture is often preferred in software design due to its advatages in terms of testability, maintainabilty and scalability. Here testability is one of the main benefits because it helps to create a more manageable approach to software testing. This is primarily done through the principle of loose coupling and separation of concerns.

**Provide an example of how testing can be supported by using a layered architectural approach.**

If we consider a web application where users can submit and view posts, the application is divided into 3 different layers:

- Presentation Layer (UI)

  This layer handles the user interface and interactions. It is responsible for rendering web pages and handling the users input.

  ```python
  # presentation_layer.py

  class WebApp:
    def __init__(self):
        self.controller = PostController()

    def submit_post(self, content):
        self.controller.create_post(content)

    def view_posts(self):
        posts = self.controller.get_posts()
        # Render posts on the webpage

  web_app = WebApp()
  ```

- Business logic

  This layer contains the core logic of the application - this includes post creation and retrieval.

  ```python
  # business_logic.py

  class PostController:
    def __init__(self):
        self.data_access = PostDataAccess()

    def create_post(self, content):
        # Business logic for creating a new post
        self.data_access.save_post(content)

    def get_posts(self):
        # Business logic for retrieving posts
        return self.data_access.get_all_posts()
  ```

- Data access

  This layer interacts with the database or external data sources.

  ```python
  # data_access.py

  class PostDataAccess:
    def save_post(self, content):
        # Code to save the post to the database

    def get_all_posts(self):
        # Code to retrieve all posts from the database
  ```

#### Example testing scenario

If we wanted to test the creation of a post in the layered archtecture the test could look something like this:

```python
# Test for creating a post using a testing framework like pytest

def test_create_post():
    web_app = WebApp()
    web_app.submit_post("Test Post")

    # Assert that the post was created and exists in the database
    posts = web_app.controller.get_posts()
    assert "Test Post" in posts
```

In this test we can isolate the presentation layer from the business logic and data access layers. This makes it a lot easier to focus on the specific thing we want to test. It generally makes it easier to make tests because you can isolate the thing you want to test.

#### Advantages

- Testing is easier because you can test each layer by itself.
- It's easy to use mocking or test databases for each layer.
- Making changes in a layer has very little affect on other layers as they are loosely coupled.

Should this have been done using a monolithic approach there would have been no layers thus no clear separation of concerns. This makes testing and maintaining the application much more challenging. A monolithic approach would probably be more suitable for very small applications.

### 3)

**One disadvantage of layered architecture is often given as the risk of 'leaking logic' between the layers. What does this mean in practice and why might it be a problem? - hint: What is the point of layers and how do we draw the boundary?**

The term 'leaking logic' refers to a project where the layers of the system becomes blurred. This means that code and logic that is meant for one layers starts to affect other layers. This is a problem because it can lead to a number of different issues and compromises the benefits of a layered architecture.

In practice:

- **Violation of separation of concerns:** Each layer has its own responsibility and should not be concerned with what other layers do.
- **Reduced maintainability:** If the logic is scattered across several layers, it becomes harder to maintain the system. Changes in one layer may have unintended impact on another thus making the codebase more complex.
- **Decreased reusability:** Code that is tightly coupled to a specific layer is less reusable.
- **Testing challenges:** If logic is leaked it also makes testing harder as we are unsure of where the logic comes from.
- **Performance issues:** Leaking logic can result in lowered performance. It might result in suboptimal queries or inefficient data.
- **Security risks:** If logic from the security layer is leaked into another layer this is a big risk.
- **Code duplication:** Leaked logic can result in duplicate code if similar functionality is implemented in multiple layers.

### 4)

**Layers are Logical and Tiers are physical. A widely used version of N-tier involves three tiers – what are they and what is the role of each of them? This could be illustrated with an example application and diagrams.**

- Presentation Tier (User interface tier)
  - **Role:** This is the top tier and it is responsible for receiving and presenting information to the user. This is what the user interacts with.
  - **Components:** User interfaces, web pages, mobile app interfaces or whatever else the user might be able to interact with.
  - **Responsibilities:** Handling user input, diplaying information and user friendly interface.
- Application Tier (Business logic tier)
  - **Role:** This is the middle tier and this is where the core logic and processing is done. This is what connects the presentation tier and the data tier.
  - **Components:** Application servers, web servers etc.
  - **Responsibility:** Processing and managing business rules, making decisions and coordinating communication.
- Data Tier (Data storage tier)
  - **Role:** This is the bottom tier and this is where the data is stored. This usually involves databases and other data storage mechanisms.
  - **Components:** Databases, file systems, data storage services etc.
  - **Responsibility:** Storing, retrieving and managing data as well as insuring integrity and security.

### 5)

**Event Driven Architecture has been described as “a distributed, asynchronous software architecture pattern that integrates applications and components through the production and handling of events”. How would you explain this description to a business user? You might find it helpful to illustrate the answer with diagrams.**

**Analogy: The Office Party**

Imagine your company is like hosting an office party. People in different departments or teams are attending, and they need to communicate and collaborate seamlessly. Event Driven Architecture is like organizing this party to make sure everyone can interact effectively.

**Components of EDA:**

**Events:** In the office party scenario, think of events as messages or announcements. These are notifications that something has happened or needs attention. In your business, events could be things like a new order from a customer, a payment confirmation, or a stock level reaching a critical point.

**Asynchronous:** Instead of everyone being in one big meeting at the same time (synchronous), people can interact at their own pace during the party. In EDA, the communication happens at different times, and this flexibility makes it more efficient for handling different tasks and processes.

**Integration:** Just as you'd want all the partygoers to connect and work together, EDA integrates different applications and systems in your company. It makes sure your sales system talks to your inventory system, and your customer support system communicates with your billing system.

**Distributed:** In the party analogy, you might have different rooms or areas where people gather. EDA also works across different locations or departments in your business. It doesn't matter if your sales team is in one office and your IT team is in another – EDA helps them work together seamlessly.

**Benefits of EDA:**

**Flexibility:** EDA allows your business to respond to changes and events in real-time. Just like at a party, you don't need to wait for a big meeting to make decisions; you can act quickly when needed.

**Scalability:** As your business grows, you can add more people to the party without disrupting the entire event. EDA can handle more events and data without a complete overhaul of your systems.

**Reliability:** When someone leaves the party, others can still enjoy themselves. In EDA, if one component or system fails, it doesn't bring down the entire process. Your business can continue to function even when there are technical hiccups.

**Efficiency:** Just as a well-organized party ensures that people have a good time, EDA helps your business run more smoothly. It streamlines processes and reduces bottlenecks, making your operations more efficient.

In summary, Event Driven Architecture is like organizing an office party where people from different departments can communicate and collaborate efficiently, in their own time, while ensuring the event runs smoothly, adapts to changes, and can scale up as needed. This approach is valuable for modern businesses that need agility and seamless integration between various software systems.

### 6)

**What is the difference between a message queue (point to point pattern) and message topics (publish-subscribe pattern)? Provide diagrams to illustrate your explanation.**

**Message queue**

```
Producer ----> |------- Queue -------| ----> Consumer
               |                   |
               |                   |
               |                   |
               |                   |

```

**Message topics**

```
Publisher ----> |------ Topic -------| ----> Subscriber 1
                |                    |
                |                    |
                |                    |
                |                    |
                |                     ----> Subscriber 2
                |
                 ----> Subscriber 3

```

Key differences:

- **Message Queue**
  - One-to-one
  - Each message is consumed by a single recipient
  - Often used for tasks that must be processed by a single worker.
- **Message Topics**
  - One-to-many
  - Messages are broadcasted to multiple subscribers.
  - Suitable for scenarios where multiple consumers are interested in the same type of message.

### 7)

**A mediator topology and a broker topology can both be used to manage event streams. Briefly describe what broker and mediator topologies are and the use cases they are best suited to. Use diagrams to illustrate your answer, which could relate to the same application but different services (ones better suited to a mediator topology and one to a broker technology).**

**Mediator Topology:**
In a mediator topology, events flow through a central mediator, which routes and processes the events before delivering them to the appropriate consumers. This topology is best suited for applications requiring complex event processing, filtering, and transformation.

- **Complex Event Processing:** Mediator topologies are ideal for applications that require complex event processing, where events need to be filtered, transformed, or aggregated before reaching consumers.
- **Content-Based Routing:** They are suitable for routing events based on content or metadata to specific consumers or services.
- **Orchestration:** Mediator topologies are used in scenarios where events trigger orchestration of multiple services or components.

```
         +--------+
         | Source |
         +---+----+
             |
          +--v--+
          | Mediator |
          +--+---+
             |
    +--------+--------+
    |    Consumer 1   |
    +-----------------+
    |    Consumer 2   |
    +-----------------+
    |    Consumer 3   |
    +-----------------+
```

**Broker Topology:**
In a broker topology, events are published to a central broker, which then distributes them to multiple consumers based on subscriptions. This topology is best suited for applications that require event broadcasting and scalable fan-out.

- **Publish-Subscribe:** Broker topologies are excellent for applications that require a publish-subscribe model, where events are broadcast to multiple consumers or subscribers.
- **Scalable Fan-Out:** They are well-suited for scenarios where you need to distribute events to a large number of consumers efficiently.
- **Loose Coupling:** Broker topologies promote loose coupling between event producers and consumers, making them suitable for decoupled and scalable systems.

```
+--------+    +--------+
| Source |    | Source |
+---+----+    +---+----+
    |            |
    |            |
+---v---+    +---v---+
| Broker|    | Broker|
+--+----+    +--+----+
   |            |
   |            |
+--v--+       +--v--+
|Consumer|    |Consumer|
+-------+    +-------+
```

In summary, mediator topologies are more appropriate for scenarios requiring event processing and content-based routing, while broker topologies are better for broadcasting and scalable distribution of events to multiple consumers. The choice depends on the specific needs for the application.

### 8)

**Follow the Terraform introduction <https://developer.hashicorp.com/terraform/intro> and provide an example of how Terraform can be used to support provisioning infrastructure such as deploying containers. (the quick start tutorial will help <https://developer.hashicorp.com/terraform/tutorials/docker-get-started>)**

**Terraform configuration file:** Here there is defined the necessary infrastructure resources. In this case, you would specify the infrastructure components required for container deployment.

```
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

resource "aws_security_group" "example" {
  name_prefix = "example-"
}
```

**Deploy Containers:** To deploy containers, the example use a container orchestration tool - Kubernetes. In the Terraform configuration, you can define Kubernetes resources like pods, services, and deployments.

```
resource "kubernetes_deployment" "example" {
  metadata {
    name = "example-deployment"
  }

  spec {
    replicas = 3

    template {
      metadata {
        labels = {
          app = "example"
        }
      }

      spec {
        container {
          image = "nginx:latest"
          name  = "example-container"
        }
      }
    }
  }
}
```

**Initialize and Apply:**
Run `terraform init` to initialize the Terraform workspace, and then use `terraform apply` to create the defined infrastructure and deploy containers. Terraform will create and configure the infrastructure resources based on your configuration.

### 9)

**Set up a git repository according to the rules of the Github Flow and Trunk based development strategies and explain the arguments for and against using them. As part of your answer explain why the Git Flow strategy is now considered out of date. Use tools like git log --graph demonstrate how files are stored in git using the different strategies.**

#### Flow

![image](https://github.com/MathiasJensen96/OLA2/assets/56347572/5d6e5532-27e2-43be-8082-6005c3a60713)

Pros

- Allows parallel development since feature branches are long-lived
- Facilitates a continuous delivery workflow
- Lessens the complexity of merging since fewer commits are required

Cons

- Takes time to adjust to
- Makes project history challenging to read
- Requires more effort to fix bugs

#### Trunk

![image](https://github.com/MathiasJensen96/OLA2/assets/56347572/224040de-4106-430f-a97e-5ddc1b37e061)

Pros

- Allows continuous code integration
- Reduces the risk of introducing bugs
- Makes it easy to fix and deploy code quickly
- Allows asynchronous code reviews
- Allows comprehensive automated testing

Cons

- Increases the risk of bug creation since regression testing doesn't happen on every merge
- Requires developers to wait for changes to go through test processes and automated builds before merging
- Requires more manual effort to maintain the core trunk

Git flow is not used for continuous integration and that is why it is considered out of date.

### 10)

**Explain what git blobs, trees, commits and tags are as git object types. Provide a brief explanation of their role in the git system. This answer should include screen shots of a git repository file system being explored showing the different object types.**

**Tree:**

- Role: Trees represent directories or folders. They are like snapshots of a directory at a specific point in time and link to blobs or other trees.
- In the Git file system, trees organize the hierarchy of directories and point to blobs and other trees.

_How to find trees in terminal:_
![image](https://github.com/MathiasJensen96/OLA2/assets/70536109/47b39294-00a5-4dc1-9952-df47057654ee)

**Commit:**

- Role: Commits are snapshots of the entire project at a particular point in time. They include a reference to a tree, author information, commit message, and pointers to parent commits.
- In the Git file system, commits are linked together to form a history of changes.

![Commits](/documents/images/git-log.jpg)

**Blob (Binary Large Object):**

- Role: Blobs represent file content at a specific version. They store the actual data of files.
- In the Git file system, blobs are associated with file content.

![Git_objects](./documents/images/tag-blob.png)

**Tags:**

- Role: Tags are references or labels that are assigned to specific commits. They are typically used to mark important milestones or versions in a project's history.
- In the Git file system, tags are used to provide meaningful names to specific commits.

_How to make a tag in terminal:_

![image](https://github.com/MathiasJensen96/OLA2/assets/70536109/770e1937-0641-47d7-aefd-40db2d2fa69b)

Tags are also used by GitHub to mark _releases_.

_Tag shown in repository:_

![image](https://github.com/MathiasJensen96/OLA2/assets/70536109/d2f45e3e-bab5-4c6f-80ce-4faa7291e904)

### 11)

**Explain what the following two commands mean: Provide examples of exploring the git objects stored in your local file system.**

```
$> find .git/objects
$> git cat-file -p 3d8071 (where 3d8071 is a partial SHA1 hash)
```

**`find .git/objects`**

This command is used to list the objects in the `.git/objects` directory, which is where Git stores its object database. These objects are usually stored in subdirectories based on their SHA-1 hash values.

![Git_objects](./documents/images/git-objects.png)

**`git cat-file -p 95ba6f54`**

This command is used to display the contents of a Git object using its SHA-1 hash. The `-p` option stands for "pretty-print", and is used to make the output more readable.

![Git_objects](./documents/images/cat-file.png)

### 12)

**What does the `git cherry-pick` command do? Provide an example of using it and explain how it is useful.**

Cherry picking is the act of picking a commit from a branch and applying it to another. git cherry-pick can be useful for undoing changes. For example, say a commit is accidently made to the wrong branch. You can switch to the correct branch and cherry-pick the commit to where it should belong.

```
a - b - c - d   Main
         \
           e - f - g Feature
```

You can find a commit reference by using git log. In this example we have constructed lets say we wanted to use commit `f` in main. First we ensure that we are working on the main branch.
Then we can run the `git cherry-pick f` (change the `f` with the commit reference, the `f` is for demo purpose)

```
a - b - c - d - f   Main
         \
           e - f - g Feature
```

The commit `f` is now in main branch

### 13)

**Describe the role of CI/CD pipelines in managing software deployment.**

Continuous Integration/Continuous Deployment (CI/CD) pipelines play a crucial role in managing the software deployment process by automating and streamlining the delivery of software from development to production. These pipelines are essential in modern software development to achieve faster, more reliable, and scalable deployments.

**Continuous Integration (CI):**

- **Automated Code Integration:** CI involves regularly integrating code changes from multiple developers into a shared repository. This process is automated, allowing for frequent code integration.

- **Automated Testing:** After code integration, CI pipelines automatically trigger a suite of tests, including unit tests, integration tests, and sometimes end-to-end tests, to ensure that the new code does not introduce regressions.

- **Immediate Feedback:** If the tests fail, developers receive immediate feedback, allowing them to address issues early in the development cycle. This reduces the likelihood of defects accumulating.

- **Code Quality and Style Checks:** CI pipelines often include checks for code quality, coding standards, and security vulnerabilities, ensuring that the codebase maintains a high level of quality.

**Continuous Deployment (CD):**

- **Automated Deployment:** CD is responsible for automating the deployment of the software to various environments, such as development, staging, and production. This automation ensures consistency and reduces the risk of manual errors during deployment.

- **Environment Promotion:** CD pipelines facilitate the promotion of code through different environments, allowing for thorough testing and validation at each stage. This helps catch issues before they reach the production environment.

- **Rollbacks and Versioning:** CD pipelines support the ability to roll back to previous versions in case of issues in production. Versioning and tagging are often used to keep track of the deployed software and simplify rollback processes.

- **Monitoring and Feedback:** CD pipelines can integrate with monitoring and logging systems to provide feedback on the performance and behavior of the deployed application. This enables quick identification and resolution of issues in the production environment.

**Benefits of CI/CD Pipelines:**

- **Speed:** CI/CD pipelines enable rapid software delivery, reducing the time between code changes and their availability in production.

- **Consistency:** Automation ensures that deployment processes are consistent across different environments, reducing configuration drift and human errors.

- **Quality Assurance:** Automated testing and code quality checks help maintain a high level of software quality and reduce the risk of deploying faulty code.

- **Scalability:** CI/CD pipelines can easily adapt to the needs of growing software projects and support the deployment of new features and updates with minimal friction.

- **Collaboration:** CI/CD promotes collaboration among development, testing, and operations teams, fostering a culture of shared responsibility for software delivery.
