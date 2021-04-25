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


VIPER architecture consists of five layers of components. Each of the letters of VIPER represents one of the layers. Lets take a look at each of the layers:

1. **View (Views + ViewControllers):** This layer is comprised of Views and ViewControllers. All the view presentation logics are in this layer.
2. **Interactor:** Business logics for each specific use cases are in the Interactor layer.
3. **Presenter:** Presenter layes between the Interactor and View layer. It prepares Interactor Data to be used by View. It is sort of ViewModel in MVVM.
4. **Entity (Model):** This layers comprises of the Stucts and Classes to represent the data used by the App.
5. **Router (Coordinator):** This layer manages all the navigation logic of the App.


thanks:
* https://github.com/nodes-ios/Playbook/blob/master/ViperArchitecture.md