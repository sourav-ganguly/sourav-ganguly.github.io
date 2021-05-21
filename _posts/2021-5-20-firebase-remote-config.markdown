---
layout: post
title:  "Firebase 🔥 Remote Config for iOS"
date:   2021-5-20
categories: ios, firebase, Swift
---

> Change the behavior and appearance of your app without publishing an app update, at no cost, for unlimited daily active users.

With Firebase Remote Config, we can define remote parameters for our app, and update them in the cloud. On the application side, we can use remote config parameters to show different features and designs of the application without updating the application.


### Some of the Use Cases:
1. Rollout new features within some percentage of the users
2. Define platform and locale-specific promos, features
3. A/B testing of a feature


### How the Remote Config Client Library Works:
Firebase included a client library to handle all the works. With the library we can:
- Set default values for parameters
- Fetch remote config values from the server
- Activate the fetched value at our convenient time (only fetching the value will not activate it)
- Get remote config value over a get method (this get method is the single access point)

As I said, there is a get method which is the single access point for whether we get the server set value or app set default values. Which value we get depends on the precedence of the data. See the below illustration to understand the data precedence.


![Image of Firebase Remote Config Data Precedence](/assets/remoteconfig.png)

Firebase Remote Config value Precedence ⬆️


### Setting Up Steps:

1. Set up RemoteConfig in the firebase( This step is not be included in this tutorial. Check out firebase official doc for details.)

2. Install Firebase SDK
> pod 'Firebase/RemoteConfig
Also import the .plist file from the firebase into your project. And configure firebase by calling FirebaseApp.configure() from the AppDelegate on App launch.

3. Configure the singleton RemoteConfig object

```swift
remoteConfig = RemoteConfig.remoteConfig()
let settings = RemoteConfigSettings()
settings.minimumFetchInterval = 0
remoteConfig.configSettings = settings
```

For debugging purposes, set a low value to minimumFetchInterval while developing. Otherwise, you cannot test the changed remote config value immediately. Need to remove this line in production.

4. Set in-app default values
So that the app works as intended before fetching values from the remote server. You can set defaults from a .plist file or from a dictionary.

```swift
remoteConfig.setDefaults(fromPlist: "RemoteConfigDefaults")
// or
remoteConfig.setDefaults(aDictionary)
```

It is a good practice to provide some default values for the remote config parameters. If the value is not set in the remote or if the app could not fetch from the network, the default value is used. When there is a remote value for a key (and it is fetched and activated), the default value will be replaced by the remote value.

5. Fetch and Activate remote values
Fetch remote parameter values from the server with _fetchWithCompletionHandler:_ method. But to make the fetched values available for the app, you need to use the method _activateWithCompletionHandler:_

```swift
remoteConfig.fetch() { (status, error) -> Void in
  if status == .success {
    print("Config fetched!")
    self.remoteConfig.activate() { (changed, error) in
      // ...
    }
  } else {
    print("Config not fetched")
    print("Error: \(error?.localizedDescription ?? "No error available.")")
  }
  self.displayWelcome()
}
```


Here, I first fetched remote config, and on completion activated it.

There are two different methods for fetching and activating the remote values. There is a reason for that. The fetching takes some time, and it can create inconsistency in view updating. So, what we can do is firstly fetch remote values on the app launch. But wait and activate the remote values at our convenient time.

A good approach is to fetch at the app launch or later but not activating immediately. Activate the fetched value in the next app launch. Activating is almost instantaneous. So this approach does not create any view inconsistency.

```swift
remoteConfig.activate { [weak self] (changed, error) in
    log("Remote config activate changed = \(changed)")
    if error != nil {
        log("Error activating remote config \(error.debugDescription)")
    }
    self?.remoteConfig.fetch(withExpirationDuration: 0) { (status, error) in
        log("Remote config fetch status = \(status.rawValue)")
        if error != nil {
            log("Error fetching remote config \(error.debugDescription)")
        }
    }
}
```

6. Get remote config values to use in the App with configValueForKey:

```swift
let sampleValue = remoteConfig.configValue(forKey: "sample_key")
```

We can get remote config value with this configValue(forKey: ) method. This method is the single access point to retrieve the value. It will provide the remote value if available, otherwise, it will provide the default value.

The return type of the method is FIRRemoteConfigValue. Don’t worry, we can easily convert it to String, Bool, Number, or Data with the stringValue, boolValue, numberValue and dataValue methods.


#### Tips for debugging:
- Set minimal fetch interval to 0 in RemoteConfigSettings

```swift
let settings = RemoteConfigSettings()
settings.minimumFetchInterval = 0
```

- Use fetch(withExpirationDuration: 0) to fetch the value
If we do not set these two values, we will not be able to see the changes of the remote values immediately.

#### Thanks and Further reading:
<https://firebase.google.com/docs/remote-config>
