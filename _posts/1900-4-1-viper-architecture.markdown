---
layout: post
title:  VIPER architecture
date:   2021-04-4 23:40:16 +0600
categories: iOS
---

# TODO


VIPER architecture consists of five layers of components. Each of the letters of VIPER represents one of the layers. Lets take a look at each of the layers:

1. **View (Views + ViewControllers):** This layer is comprised of Views and ViewControllers. All the view presentation logics are in this layer.
2. **Interactor:** Business logics for each specific use cases are in the Interactor layer.
3. **Presenter:** Presenter layes between the Interactor and View layer. It prepares Interactor Data to be used by View. It is sort of ViewModel in MVVM.
4. **Entity (Model):** This layers comprises of the Stucts and Classes to represent the data used by the App.
5. **Router (Coordinator):** This layer manages all the navigation logic of the App.


thanks:
* https://github.com/nodes-ios/Playbook/blob/master/ViperArchitecture.md