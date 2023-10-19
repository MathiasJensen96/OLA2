# OLA2

## Group Members
- Kasper Dahl, cph-kd131@cphbusiness.dk
- Mads Kristensen, cph-mk613@cphbusiness.dk
- Maria McNally, cph-mm620@cphbusiness.dk
- Mathias Jensen, cph-mj839@cphbusiness.dk

## Questions

### 1) 

#### What is a ’Layered’ Architecture?

A "Layered" architecture is a design pattern commonly used in software development to structure and organize the components of a system into different layers, each with a specific set of responsibilities. These layers are typically designed to interact with one another in a well-defined and controlled manner. The layered architecture is often associated with the concept of "separation of concerns," where each layer handles a specific aspect of the application, making the system more modular, maintainable, and easier to develop.

#### What is the difference between an open and a closed layer?

In a layered architecture, there are generally two types of layers: open and closed layers:

  - Open Layer:
    -  An open layer is a layer that allows interaction from both the layers above and below it.
    -  The features and functionality of an open layer is exposed to other layers and they are able to use these
    -  Usually open layers are used for communication and data flow between the various parts of the system.
   
  -  Closed Layers:
     -  A closed layer is a layer that restricts direct interaction from other layers except it's closest neighbors.
     - Functionality is restricted and requires access in order to be used.
     - Closed layers are designed to provide isolation to its services.

####  Provide an example of why closed layers may cause a problem

Closed layers are great in terms of maintaining the integrity of a system and to ensure that each layer's responsibilities are isolated. There are however challenges such as:

- Increased complexity: Closed layers requires additional abstractions and interfaces in order to communicate between layers. This can make the system harder to understand and maintain.
- Performance overhead: When enforcing controlled access between the layers it requires additional data transformation, processing and maybe even network calls.
- Rigidity: They make the system harder to adapt and make changes in. This is due to the fact that changing one thing in one layer may require updates in several other layers aswell. This is very time consuming and could cause multiple errors.
- Debugging and troubleshooting: It can be harder to debug due to layers being abstracted and hidden.


### 2)

#### Testability is often given as an advantage of layered architectures. How does a layered architecture make testing easier and more structured?

A layered architecture is often preferred in software design due to its advatages in terms of testability, maintainabilty and scalability. Here testability is on of the main benefits because it helps to create a more manageable approach to software testing. This is primarily done through the principle of loose coupling and separation of concerns.

#### Provide an example of how testing can be supported by using a layered architectural approach

If we consider a web application where users can submit and view posts, the application is divided into 3 different layers:

- Presentation Layer (UI)

  This layer handles the user interface and interactions. It is responsible for rendering web pages and handeling the users input.
  ```bash
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
  ```bash
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
  ```bash
  # data_access.py

  class PostDataAccess:
    def save_post(self, content):
        # Code to save the post to the database

    def get_all_posts(self):
        # Code to retrieve all posts from the database
  ```

#### Example testing scenario
If we wanted to test the creation of a post in the layered archtecture the test could look something like this:
```bash
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

#### One disadvantage of layered architecture is often given as the risk of 'leaking logic' between the layers. What does this mean in practice and why might it be a problem? - hint: What is the point of layers and how do we draw the boundary?


