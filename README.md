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
