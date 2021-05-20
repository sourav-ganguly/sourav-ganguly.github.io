<!-- ---
layout: post
title:  VIPER architecture
date:   2000-04-4 23:40:16 +0600
categories: iOS
--- -->

# TODO

Clean Architecture's Charecterstics:
* Major objective is separation of concern
* Independence from frameworks. Using framework as a tool rather than design application around frameworks.
* Highly testable. Each layer is separated with a Interface. So easy to test each layer.
* UI and business logics are separated. 

To Read:
* https://www.objc.io/issues/13-architecture/viper/
* https://www.raywenderlich.com/8440907-getting-started-with-the-viper-architecture-pattern
* https://appsmaventech.com/blog/Understanding-the-Architecture-of-iOS-Apps-with-VIPER


Viper is an implementation of clean architecture. There are five different components in this architecture, and each with distinct role. And each components are well separated from each other with input and output Protocol. This separation conforms to Single Responsibility Principle.

> VIPER is an application of Clean Architecture to iOS apps. (1)
> Clean Architecture divides an app’s logical structure into distinct layers of responsibility. This makes it easier to isolate dependencies (e.g. your database) and to test the interactions at the boundaries between layers. (1)
> At its core, VIPER is an architecture based on the Single Responsibility Principle. (1)

VIPER architecture consists of five layers of components. Each of the letters of VIPER represents one of the layers. Lets take a look at each of the layers:

1. **View (Views + ViewControllers):** This layer is comprised of Views and ViewControllers. All the view presentation logics are in this layer.
2. **Interactor:** Business logics for each specific use cases are in the Interactor layer.
3. **Presenter:** Presenter layes between the Interactor and View layer. It prepares Interactor Data to be used by View. It is sort of ViewModel in MVVM.
4. **Entity (Model):** This layers comprises of the Stucts and Classes to represent the data used by the App.
5. **Router (Coordinator):** This layer manages all the navigation logic of the App.

thanks:
1. https://www.objc.io/issues/13-architecture/viper/
2. https://github.com/nodes-ios/Playbook/blob/master/ViperArchitecture.md

### Iteractor:
> An Interactor represents a single use case in the app. It contains the business logic to manipulate model objects (Entities) to carry out a specific task. (1)
> The work done in an Interactor should be independent of any UI. The same Interactor could be used in an iOS app or an OS X app. (1)
> Any application-dependent logic will most likely be in an Interactor. (1)
> where should this networking take place and what should be responsible for initiating it? It’s typically up to the Interactor to initiate a network operation, but it won’t handle the networking code directly. It will ask a dependency, like a network manager or API client. The Interactor may have to aggregate data from multiple sources to provide the information needed to fulfill a use case. Then it’s up to the Presenter to take the data returned by the Interactor and format it for presentation.
> As an Interactor applies its business logic, it will need to retrieve entities from the data store, manipulate the entities, and then put the updated entities back in the data store. The data store manages the persistence of the entities.
> The Interactor should not know how to persist the entities either. Sometimes the Interactor may want to use a type of object called a data manager to facilitate its interaction with the data store. The data manager handles more of the store-specific types of operations, like creating fetch requests, building queries, etc. This allows the Interactor to focus more on application logic and not have to know anything about how entities are gathered or persisted. 

### Entity:
> Entities are the model objects manipulated by an Interactor. Entities are only manipulated by the Interactor. The Interactor never passes entities to the presentation layer (1)
> If you are using Core Data, you will want your managed objects to remain behind your data layer. Interactors should not work with NSManagedObjects. (1)


### Presenter: 
> The Presenter is a PONSO that mainly consists of logic to drive the UI. It knows when to present the user interface. It gathers input from user interactions so it can update the UI and send requests to an Interactor. (1)
> The Presenter also receives results from an Interactor and converts the results into a form that is efficient to display in a View. (1)
> Entities are never passed from the Interactor to the Presenter. Instead, simple data structures that have no behavior are passed from the Interactor to the Presenter. This prevents any ‘real work’ from being done in the Presenter. The Presenter can only prepare the data for display in the View.

### View:
> The View is passive. It waits for the Presenter to give it content to display; it never asks the Presenter for data. (1)
> The view controller shouldn’t be making decisions based on these actions, but it should pass these events along to something that can. (1)

### Router
> The coordinator (or Router) is responsible to instantiate and initilize the views during the app lifecycle when needed.(2)

Testing:
> Following VIPER encourages a separation of concerns that makes it easier to adopt TDD. The Interactor contains pure logic that is independent of any UI, which makes it easy to drive with tests. The Presenter contains logic to prepare data for display and is independent of any UIKit widgets. Developing this logic is also easy to drive with tests.
