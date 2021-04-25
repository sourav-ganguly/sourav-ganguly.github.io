---
layout: post
title:  "Asynchronus Programming is Flutter: Future, async, await"
date:   2021-4-25 1 12:10:16 +0600
categories: flutter, dart, async
---

Asynchronous operations let your program complete work while waiting for another operation to finish. 

#### Synchronous vs asynchronous operation:
Synchronous operation: A operation that blocks other operations from executing before it completes.
Asynchronous operation: A operation that allows other operation to execute before it completes.

#### Here are some common asynchronous operations:
* Fetching data over a network.
* Writing to a database.
* Reading data from a file.

To perform asynchronous operations in Dart, you can use the Future class and the async and await keywords.

## Future
A future represents the result of an asynchronous operation. It has two states, completed and uncompleted. A future is uncomplete before the function's asynchronous operation finishes. If the asynchronous operation succeeds, the future completes with a value, otherwise future completes with an error.

## async & await
To declare a function asyncronous, use *async* keyword before the function body. 
*await* keyword used to wait for a asynchronous process to finish before continue to execute later part of the function. But remember, *await* function only works with a function declare *async*. 

## How to make a synchronous method asynchronous:

#### Synchronous

```dart
String createOrderMessage() {
  var order = fetchUserOrder();
  return 'Your order is: $order';
}
```

#### Asynchronous
1. Add Future as return type
2. Add async before function body
3. Use await for calling anohter async function or operation

```dart
Future<String> createOrderMessage() async {
  var order = await fetchUserOrder();
  return 'Your order is: $order';
}
```