---
layout: post
title:  "Redux for Flutter"
date:   2021-07-2
categories: flutter
---

Redux is an architecture + an library for state management in flutter. One of the the key take of this architecture is uniderectional data flow. State management is completely separated from UI. 

Redux helps you manage "global" state - state that is needed across many parts of your application.

> Redux is a unidirectional data flow architecture that makes it easy to develop, maintain and test applications. (1)

> Redux is a pattern and library for managing and updating application state, using events called "actions". (3)


 ### 3 major tenants of redux:
 1. Single source of truth: The state of your whole application is stored in an object tree within a single store.
 2. State is read-only: The only way to change the state is to emit an action, an object describing what happened.
 3. Changes are made with pure functions: To specify how the state tree is transformed by actions, you write pure reducers.

 ## Redux Basics:

 ### the store: 
 The center of the redux application is the store. Store is lika a container that holds the app global state. The views from different part of the application get application state from the store. 
 * Never directly modify the state inside the store.
 * Only way to update the state is to dispatch an action(action describe some event) into the store to tell it what happend. 
 * When action is dispatched, the store use reducer to update the global state.
 * The Store notify the view that the state has been updated, and the view update itself.

 ### the view:
 View is the visual representation of the current app state. View is the flutter app we wrote. How the app looks and all of it's designs are defined here. 
 * View knows nothing about the State Management.
 * View never directly update state in the Store.
 * View subscribes to the Store, and get the State whenever the state changes.
 * View defines itself based on the state. 
 * For events, view dispatches events into the Store.

 ### actions:
 All of the events that can occur in the app are defined as Actions.
 * View dispathes Actions to the Store when events occur.
 * Store envoke reducer with the action. Reducer retuns a new state based on the Action. The Store updates the state.

 ### reducer:
 Reducer is a pure function that take an action an state as an input and returns a new state as output. There is a root reducer. There are multiple reducer for each actions. The root reducer pipeline the action to respective sub-reducer, and return the returned State. 
 * The store uses reducer to update the global state for actions.

 ### middleware:
 Middleware is reducer with side effect. For things like, network call or save data to  device memory we use reducer.
 * Before reducer, the action comes to middleware when a middlewire is present.
 * On getting action, the reducer may do some side effect like network call or save data to memory.
 * Or maybe it do nothing and directly relay the action and the Store use reducer to handle that action.
 * After finishing the side effect, the reducer may dispatch a new action and the Store will hand the action to the reducer.

### data flow:

![Image of redux data flow](/assets/redux-data-flow.png)

In this data flow diagram, you can see redux is all about state. There is a single app state, and the state is read only for view. View build itself based on the state. View send action for events. Action fires reducer, reducer create a new state. The view update itself with the new State. And this repeats.

### data flow with middleware:

![Image of redux data flow](/assets/dataflow-redux-middleware.png)

### data flow in steps:
* actions are dispatched in response to a user interaction like a click
* the store runs middleware if exist. middleware do side effect and may or may not dispatch new action
* the store runs the reducer function to calculate a new state
* the UI reads the new state to display the new values


### show me the code:
Below is a very simple example of redux from the package flutter_redux:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_redux/flutter_redux.dart';
import 'package:redux/redux.dart';

// One simple action: Increment
enum Actions { Increment }

// The reducer, which takes the previous count and increments it in response
// to an Increment action.
int counterReducer(int state, dynamic action) {
  if (action == Actions.Increment) {
    return state + 1;
  }

  return state;
}

void main() {
  // Create your store as a final variable in the main function or inside a
  // State object. This works better with Hot Reload than creating it directly
  // in the `build` function.
  final store = Store<int>(counterReducer, initialState: 0);

  runApp(FlutterReduxApp(
    title: 'Flutter Redux Demo',
    store: store,
  ));
}

class FlutterReduxApp extends StatelessWidget {
  final Store<int> store;
  final String title;

  FlutterReduxApp({Key key, this.store, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // The StoreProvider should wrap your MaterialApp or WidgetsApp. This will
    // ensure all routes have access to the store.
    return StoreProvider<int>(
      // Pass the store to the StoreProvider. Any ancestor `StoreConnector`
      // Widgets will find and use this value as the `Store`.
      store: store,
      child: MaterialApp(
        theme: ThemeData.dark(),
        title: title,
        home: Scaffold(
          appBar: AppBar(
            title: Text(title),
          ),
          body: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                // Connect the Store to a Text Widget that renders the current
                // count.
                //
                // We'll wrap the Text Widget in a `StoreConnector` Widget. The
                // `StoreConnector` will find the `Store` from the nearest
                // `StoreProvider` ancestor, convert it into a String of the
                // latest count, and pass that String  to the `builder` function
                // as the `count`.
                //
                // Every time the button is tapped, an action is dispatched and
                // run through the reducer. After the reducer updates the state,
                // the Widget will be automatically rebuilt with the latest
                // count. No need to manually manage subscriptions or Streams!
                StoreConnector<int, String>(
                  converter: (store) => store.state.toString(),
                  builder: (context, count) {
                    return Text(
                      'The button has been pushed this many times: $count',
                      style: Theme.of(context).textTheme.display1,
                    );
                  },
                )
              ],
            ),
          ),
          // Connect the Store to a FloatingActionButton. In this case, we'll
          // use the Store to build a callback that will dispatch an Increment
          // Action.
          //
          // Then, we'll pass this callback to the button's `onPressed` handler.
          floatingActionButton: StoreConnector<int, VoidCallback>(
            converter: (store) {
              // Return a `VoidCallback`, which is a fancy name for a function
              // with no parameters and no return value. 
              // It only dispatches an Increment action.
              return () => store.dispatch(Actions.Increment);
            },
            builder: (context, callback) {
              return FloatingActionButton(
                // Attach the `callback` to the `onPressed` attribute
                onPressed: callback,
                tooltip: 'Increment',
                child: Icon(Icons.add),
              );
            },
          ),
        ),
      ),
    );
  }
}
```

 ### advantages of edux:
  1. Total control over the states. Know exactly what causes state changes. Single source of truth.
  2. The patterns and tools provided by Redux make it easier to understand when, where, why, and how the state in your application is being updated, and how your application logic will behave when those changes occur.
  3. Redux guides you towards writing code that is predictable and testable, which helps give you confidence that your application will work as expected.
  4. Pure reducer is easy to test.
  5. No need to pass states all the way down.

### redux is more useful when:
 1. You have large amounts of application state that are needed in many places in the app
 2. The app state is updated frequently over time
 3. The logic to update that state may be complex
 4. The app has a medium or large-sized codebase, and might be worked on by many people
  
### thanks and further reading:
1. <https://blog.novoda.com/introduction-to-redux-in-flutter/>
2. <https://hackernoon.com/flutter-redux-how-to-make-shopping-list-app-1cd315e79b65>
3. <https://redux.js.org/tutorials/fundamentals/part-1-overview>
4. <https://pub.dev/packages/flutter_redux>