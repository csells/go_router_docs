---
title: Top-level Redirection
date: Last Modified 
permalink: /top-level-redirection/index.html
eleventyNavigation:
  key: Top-level Redirection
  order: 18
  parent: Redirection
---
Sometimes you want to guard pages from being accessed when they shouldn't be,
e.g. when the user is not yet logged in. For example, assume you have a class
that tracks the user's login info:

```dart
class LoginInfo extends ChangeNotifier {
  var _userName = '';
  String get userName => _userName;
  bool get loggedIn => _userName.isNotEmpty;

  void login(String userName) {
    _userName = userName;
    notifyListeners();
  }

  void logout() {
    _userName = '';
    notifyListeners();
  }
}
```

You can use this info in the implementation of a `redirect` function that you
pass as to the `GoRouter` constructor:

```dart
class App extends StatelessWidget {
  final loginInfo = LoginInfo();
  ...
  late final _router = GoRouter(
    routes: [
      GoRoute(
        path: '/',
        pageBuilder: (context, state) => MaterialPage<void>(
          key: state.pageKey,
          child: HomePage(families: Families.data),
        ),
      ),
      ...,
      GoRoute(
        path: '/login',
        pageBuilder: (context, state) => MaterialPage<void>(
          key: state.pageKey,
          child: const LoginPage(),
        ),
      ),
    ],

    errorPageBuilder: ...,

    // redirect to the login page if the user is not logged in
    redirect: (state) {
      final loggedIn = loginInfo.loggedIn;
      final goingToLogin = state.location == '/login';

      // the user is not logged in and not headed to /login, they need to login
      if (!loggedIn && !goingToLogin) return '/login';

      // the user is logged in and headed to /login, no need to login again
      if (loggedIn && goingToLogin) return '/';

      // no need to redirect at all
      return null;
    },
  );
}
```

In this code, if the user is not logged in and not going to the `/login`
path, we redirect to `/login`. Likewise, if the user *is* logged in but going to
`/login`, we redirect to `/`. If there is no redirect, then we just return
`null`. The `redirect` function will be called again until `null` is returned to
enable [multiple redirects](#multiple-redirections).

To make it easy to access this info wherever it's need in the app, consider
using a state management option like
[provider](https://pub.dev/packages/provider) to put the login info into the
widget tree:

```dart
class App extends StatelessWidget {
  final loginInfo = LoginInfo();

  // add the login info into the tree as app state that can change over time
  @override
  Widget build(BuildContext context) => ChangeNotifierProvider<LoginInfo>.value(
        value: loginInfo,
        child: MaterialApp.router(...),
      );
  ...
}
```

With the login info in the widget tree, you can easily implement your login
page:

```dart
class LoginPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) => Scaffold(
    appBar: AppBar(title: Text(_title(context))),
    body: Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          ElevatedButton(
            onPressed: () {
              // log a user in, letting all the listeners know
              context.read<LoginInfo>().login('test-user');

              // go home
              context.go('/');
            },
            child: const Text('Login'),
          ),
        ],
      ),
    ),
  );
}
```

In this case, we've logged the user in and manually redirected them to the home
page. That's because the go_router doesn't know that the app's state has
changed in a way that affects the route. If you'd like to have the app's state
cause go_router to automatically redirect, you can use the `refreshListenable`
argument of the `GoRouter` constructor:

```dart
class App extends StatelessWidget {
  final loginInfo = LoginInfo();
  ...
  late final _router = GoRouter(
    routes: ...,
    errorPageBuilder: ...,
    redirect: ...

    // changes on the listenable will cause the router to refresh it's route
    refreshListenable: loginInfo,
  );
}
```

Since the `loginInfo` is a `ChangeNotifier`, it will notify listeners when it
changes. By passing it to the `GoRouter` constructor, the go_router will
automatically refresh the route when the login info changes. This allows you to
simplify the login logic in your app:

```dart
onPressed: () {
  // log a user in, letting all the listeners know
  context.read<LoginInfo>().login('test-user');

  // router will automatically redirect from /login to / because login info
  //context.go('/');
},
```

The use of the top-level `redirect` and `refreshListenable` together is
recommended because it will handle the routing automatically for you when the
app's data changes.