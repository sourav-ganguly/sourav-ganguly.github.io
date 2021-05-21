---
layout: post
title:  "iOS Pen Testing/Security Testing; What It Is and How-To"
date:   2021-04-4
categories: iOS pen-testing
---

Security testing is about finding security flaws in an app. In security testing, we try to hack the app and in some cases search in the source code(white box) to find any potential security flaw.

### We try to find security flow in 7 different areas:
1. **Basic Security Flaw:** Look for very common security flaws like revealing some API key or some important data in source code.
2. **Data Storage:** Common data Storage vulnerabilities are like keeping important data unencrypted or leaking internal data in some way.
3. **Authentication:** Vulnearibilities like if it is possible some way to bypass the authentication process or security flaw in session-related data.
4. **Network:** Get unauthorized data from the Network with SQL injection or in some other way.
5. **Cryptographic:** Issues in using vulnerable or non-proper cryptographic algorithms.
6. **Platform:** Issues in third parties library and non-proper usage of system resources.
7. **Anti Reverse Engineering:** For sensitive apps, if necessary anti-reverse engineering measures are taken.

### Different approaches of pen testing are:
1. **Black box:** You have no idea about the internals of the app. Try to hack like some random app.
2. **Gray box:** You have some idea about the architecture and the app. But you don’t have the source code.
3. **White box:** You have full source code. Easier to find vulnerabilities in a limited time frame.

### Courses to follow:
Below two courses can give you some idea about the iOS Pen testing and the tools that are used.
* <https://www.udemy.com/course/hacking-and-pentesting-ios-application>
* <https://www.youtube.com/watch?v=2CKrw7ErzCY&t=3565s>

#### Book must read: 
[OWASP Mobile Security Testing Guide](https://github.com/OWASP/owasp-mstg) is a must-read for every mobile security tester. There are so many details about all kinds of mobile security vulnerabilities. On top of that, you can get a very good idea about the underlying technologies that are used in iOS to provide complex app security. 

#### Other books:
[iOS Application Security, The Definitive Guide for Hackers and Developers by David Thiel](https://nostarch.com/iossecurity)


### Tools to use:
* **Checkra1n:** Very popular jailbreak tool. Works devices lower than iPhone X. Easily un-jailbreak the device.
* **Cydia:** Most popular Appstore for jailbroken devices.
* **OpenSSH:** SSH client for mobile. Helps to run commands on mobile from a computer over SSH protocol.
* **BurpSuite:** Favorite proxy tool. Used to watch network communication and run different network hacking techniques.
* **MobSF:** MobSF (Mobile Security Framework) is an automated, all-in-one mobile application pen-testing framework capable of performing static and dynamic analysis.
* **Objection:** Runtime mobile exploration toolkit, powered by Frida, built to help you assess the security posture of your mobile applications. Can do tasks like Bypass SSL pinning, dump keychains, performing memory-related tasks, explore and manipulate objects on the heap.
* **Frida:** Frida is a free and open-source dynamic code instrumentation toolkit written by Ole André Vadla Ravnås that works by injecting the QuickJS JavaScript engine (previously Duktape and V8) into the instrumented process. Frida lets you execute snippets of JavaScript into native apps on Android and iOS.
* **Fridump:** a memory dumping tool for both Android and iOS.
* **class-dump/ class-dump-z:** is a command-line utility for examining the Objective-C runtime information stored in Mach-O (Mach object) files. It generates declarations for the classes, categories, and protocols.
* **Cycript:** Cydia Substrate (formerly called MobileSubstrate) is the standard framework for developing Cydia runtime patches (the so-called “Cydia Substrate Extensions”) on iOS. It comes with Cynject, a tool that provides code injection support for C.
* **SSL Kill Switch:** Bypass SSL pinning
* **iFunBox:** iFunBox is a file and app management tool that supports iOS. It has several features, like app installation, access the app sandbox without jailbreak and others.
* **Hopper:** This is a disassembler and reverse engineering tool. This tool lets you disassemble, decompile and debug your applications.

#### Testing Checklist:
* [OWASP Check List](https://github.com/OWASP/owasp-mstg/releases)