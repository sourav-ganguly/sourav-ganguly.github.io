---
layout: post
title:  "Mastering SSO Web Login in iOS: Exploring Various Approaches"
date:   2024-07-1
categories: iOS, SSO, web-login, SAML
---

There are multiple ways to implement the SSO login feature in iOS mobile App. In the below article, we will explore some of the approaches. Also, we will try to get some understanding of SAML — an underlying technology for SSO. We will also see a few of the security risks involved with iOS SSO implementations.

## Below are 3 common ways we can handle web login:

### 1. Using Builtin ASWebAuthenticationSession:

Among all the approaches, I found this one the easiest and the most used approach. However, the limitation is there is little space for customization. This approach leverages iOS deep-linking to communicate the auth token from the web login page to the mobile app.


```swift
// Use the URL and callback scheme specified by the authorization provider.
guard let authURL = URL(string: "https://example.com/auth") else { return }
let scheme = "exampleauth"

// Initialize the session.
let session = ASWebAuthenticationSession(url: authURL, callbackURLScheme: scheme) { callbackURL, error in
    guard error == nil, let callbackURL = callbackURL else { return }
    
    //   exampleauth://auth?token=1234
    let queryItems = URLComponents(string: callbackURL.absoluteString)?.queryItems
    let token = queryItems?.filter({ $0.name == "token" }).first?.value
    print("token: \(token)")
}

session.start()
```

#### Pros and Cons:

- **Pros:**
  - Built for this purpose
  - Password auto-fill with FaceID
  - Retains web login session if necessary
  - Can open a new private login session for each login try

- **Cons:**
  - Can only read the token from the callback URL
  - Cannot read cookies
  - Limited customization

### 2. Using SFSafariViewController:

Another approach can be using `SFSafariViewController`. You have to implement deep linking yourself and handle the callback URL in `AppDelegate`.

```swift
// Present SSO Login from ViewController
func presentSSOLogin() {
    guard let loginURL = URL(string: "https://your-sso-login-url.com") else {
        print("Invalid URL")
        return
    }
    
    let safariViewController = SFSafariViewController(url: loginURL)
    safariViewController.delegate = self
    present(safariViewController, animated: true, completion: nil)
}

// Handle callback in AppDelegate
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    if url.scheme == "your-app-scheme" && url.host == "callback" {
        handleSSOCallback(url: url)
        return true
    }
    return false
}
```

#### Pros and Cons:

- **Pros:**
  - Shares cookies and other data with Safari, which can help maintain user sessions and provide a seamless login experience.
  - Provides a secure environment for web content, leveraging Safari’s security features.
  - Runs in a separate process, which helps in isolating the web content from the app’s process.

- **Cons:**
  - Cannot read web cookies, so it is not possible to implement SSO login by reading web cookies.

### 3. Using WKWebViewController / WKWebView:

The same implementation as the code mentioned above can be done using `WKWebViewController`/`WKWebView`. Additionally, web login can be implemented by saving the auth token in the browser cookie and reading the cookie from the Mobile Application. However, reading cookies is not a recommended approach.

```swift

func readCookies() {
    let cookieStore = webView.configuration.websiteDataStore.httpCookieStore
    
    cookieStore.getAllCookies { cookies in
        for cookie in cookies {
            print("Cookie token: \(cookie.token), userName: \(cookie.userName)")
        }
    }
}
```

#### Pros and Cons:

- **Pros:**
  - Login session in Safari browser is not shared with WKWebView.
  - Possible to read cookies.

- **Cons:**
  - Security risk with reading cookies directly.

## Understanding SAML (Security Assertion Markup Language)

There are multiple ways to implement SSO login, and SAML is just one. And our web portal will implement them. From an iOS mobile app perspective, SAML is not important.

SAML is an open standard for exchanging authentication and authorization data between parties, specifically, between an identity provider (IdP) and a service provider (SP). SAML is often used to implement SSO solutions.

Other popular ways to implement SSO: OAuth, OpenID Connect.

## Do you need to add Sign In with Apple?

[is Sign In with Apple Mandatory? - Apple Developer Forums](https://forums.developer.apple.com/forums/thread/124006)
[App Review Guidelines - Apple Developer](https://developer.apple.com/app-store/review/guidelines/#4.8)

**Is Sign In with Apple Mandatory?**  
- **App Review Guidelines**  
  - Apps that exclusively use a third-party or social login service (such as Facebook Login, Google Sign-In, Sign in with Twitter, Sign In with LinkedIn, Login with Amazon, or WeChat Login) to set up or authenticate the user’s primary account with the app must also offer Sign in with Apple as an equivalent option. A user’s primary account is the account they establish with your app for the purposes of identifying themselves, signing in, and accessing your features and associated services.

  - **Sign-in with Apple is not required if:**
    - Your app exclusively uses your company’s own account setup and sign-in systems.
    - Your app is an education, enterprise, or business app that requires the user to sign in with an existing education or enterprise account.
    - Your app uses a government or industry-backed citizen identification system or electronic ID to authenticate users.
    - Your app is a client for a specific third-party service and users are required to sign in to their mail, social media, or other third-party account directly to access their content.

## Sharing Auth Token via Deep Link — Are There Any Security Risks?

If a malicious application gains access to the mobile app, there are some ways the app may gain the auth token. However, it is not possible to gain access to the token for an outside entity via sniffing.

URL schemes offer a potential attack vector into your app, so make sure to validate all URL parameters and discard any malformed URLs.

### Explore from Here:

- [Authenticating a User Through a Web Service, Apple Developer Documentation](https://developer.apple.com/documentation/authenticationservices/authenticating-a-user-through-a-web-service)
- [Using ASWebAuthenticationSession with SwiftUI • Andy Ibanez](https://www.andyibanez.com/posts/using-aswebauthenticationaession-swiftui/)
- [Log in to Websites with ASWebAuthenticationSession — Swift Dev Journal](https://www.swiftdevjournal.com/log-in-to-websites-with-aswebauthenticationsession/)
- [Implementing OAuth with ASWebAuthenticationSession - Kodeco](https://www.kodeco.com/19364429-implementing-oauth-with-aswebauthenticationsession)

### Samples found online on SAML SSO implementation in iOS:

- [iOS Deep Link Attacks Part 2 — Exploitation, 8kSec Blogs](https://8ksec.io/ios-deep-link-attacks-part-2-exploitation-8ksec-blogs/)
- [Sharing Web Data with iOS Using WKWebView (atomicobject.com)](https://spin.atomicobject.com/sharing-web-data-wkwebview/)

### Documents on SSO Login not specifying SAML:

- [Authenticating a User Through a Web Service - Apple Developer Documentation](https://developer.apple.com/documentation/authenticationservices/authenticating-a-user-through-a-web-service)
- [iOS — Facebook Login](https://developers.facebook.com/docs/facebook-login/ios/)
