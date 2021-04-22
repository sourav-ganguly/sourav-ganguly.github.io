---
layout: post
title:  "Introduction to PromiseKit"
date:   2021-04-21 23:40:16 +0600
categories: iOS
---

PromiseKit is a library that helps with asynchronous programming in swift and Objective-C. It is a wrapper around async task. Instead of using completion and error handler, a method can return a Promise. It is explained in the official doc as:
> “PromiseKit is a thoughtful and complete implementation of promises” 

#### Advantages of using PromiseKit over traditional iOS async programming:
* Cleaner and readable code
* Quick learning curve
* Very easy to call multiple async functions and managing it
* Save us from Callbacks Pyramid of Doom. Nested callbacks are now in the chain.
* Error Handling is separated. Duplicate multiple error handlings are in one place.

Example:

```swift
UIApplication.shared.isNetworkActivityIndicatorVisible = true

let fetchImage = URLSession.shared.dataTask(.promise, with: url).compactMap{ UIImage(data: $0.data) }
let fetchLocation = CLLocationManager.requestLocation().lastValue

firstly {
    when(fulfilled: fetchImage, fetchLocation)
}.done { image, location in
    self.imageView.image = image
    self.label.text = "\(location)"
}.ensure {
    UIApplication.shared.isNetworkActivityIndicatorVisible = false
}.catch { error in
    self.show(UIAlertController(for: error), sender: self)
}
```

## Common Syntax:

### ☐ then and done:
then and done are kinda like completion handlers. But the difference is that we can use them to chain multiple async calls or other operators like error handling. It is also great for readability.

Example:

```swift
firstly {
    login()
}.then { creds in
    fetch(avatar: creds.user)
}.done { image in
    self.imageView = image
}
```

### ☐ catch:
catch is closure for error handling. What is interesting here is that the catch closure catches all the errors of the whole chain.

Example:

```swift
firstly {
    login()
}.then { creds in
    fetch(avatar: creds.user)
}.done { image in
    self.imageView = image
}.catch {
    // any errors in the whole chain land here
}
```

### ☐ ensure:
ensure closure is called no matter the promise succeeds or fails. We should put “must be called” codes inside ensure clause.

Example:

```swift
firstly {
    UIApplication.shared.isNetworkActivityIndicatorVisible = true
    return login()
}.then {
    fetch(avatar: $0.user)
}.done {
    self.imageView = $0
}.ensure {
    UIApplication.shared.isNetworkActivityIndicatorVisible = false
}.catch {
    //…
}
```

### ☐ when:
Parallel multiple async calls are so much easier with promise. when takes multiple promises and wait for them to end and return a promise containing the results.

Example:

```swift
firstly {
    when(fulfilled: operation1(), operation2())
}.done { result1, result2 in
    //…
}
```

### ☐ map, compactMap etc:
The PromiseKit also included this and some other functional methods. These methods are used to change the result of a promise. These methods make the code more readable and compact.

Example:

```swift
firstly {
    URLSession.shared.dataTask(.promise, with: rq)
}.compactMap {
    try JSONSerialization.jsonObject($0.data) as? [String]
}.done { arrayOfStrings in
    //…
}.catch { error in
    // Foundation.JSONError if JSON was badly formed
    // PMKError.compactMap if JSON was of different type
}
```

### ☐ firstly:
firstly does not do anything actually. It is syntactic sugar. It just makes the code more readable.

Example:

```swift
firstly {
    login()
}.then { creds in
    //…
}
```

### ☐ Creating a promise:

```swift
func getWeather(atLatitude latitude: Double, longitude: Double) -> Promise<WeatherInfo> {
    return Promise { seal in
        let urlString = "http://api.openweathermap.org/data/2.5/weather?" + "lat=\(latitude)&lon=\(longitude)&appid=\(appID)"
        let url = URL(string: urlString)!
        let dataTask = URLSession.shared.dataTask(with: url) { data, _, error in
            guard let data = data,
                  let result = try? JSONDecoder().decode(WeatherInfo.self, from: data) else {
                seal.reject(error ?? NSError())
                return
            }
            seal.fulfill(result)
        }
        dataTask.resume()
    }
}
```

### ☐ Threading:

By default, all PromiseKit handlers run on the main queue. All handlers take an on parameter where you can specify the queue.

Example:

```swift
class MyRestAPI {
    func avatar() -> Promise<UIImage> {
        let bgq = DispatchQueue.global(qos: .userInitiated)

        return firstly {
            user()
        }.then(on: bgq) { user in
            URLSession.shared.dataTask(.promise, with: user.imageUrl)
        }.compactMap(on: bgq) {
            UIImage(data: $0)
        }
    }
}
```

### Do I need to use [weak self] in PromiseKit block?

“Generally, no. Once a promise completes, all handlers are released and so, any references to self are also released.” 
Details:[documentation](https://github.com/mxcl/PromiseKit/blob/master/Documentation/FAQ.md#do-i-need-to-worry-about-retain-cycles)

### Thanks and additional resources:
* <https://github.com/mxcl/PromiseKit/tree/master/Documentation> (all my examples are copied from this Doc. This doc is fantastic, please take a look)
* <https://agostini.tech/2018/10/08/using-promisekit/>
* <https://www.raywenderlich.com/9208-getting-started-with-promisekit>