---
title: Query Parameters
date: Last Modified 
permalink: /query-params/index.html
eleventyNavigation:
  key: Query Parameters
  order: 22
---
Sometimes you're doing [deep linking](/deep-linking) and you'd like a user to
first login before going to the location that represents the deep link. In that
case, you can use query parameters in the redirect function:

```dart
class App extends StatelessWidget {
  final loginInfo = LoginInfo();
  ...
  late final _router = GoRouter(
    routes: ...,
    errorPageBuilder: ...,

    // redirect to the login page if the user is not logged in
    redirect: (state) {
      final loggedIn = loginInfo.loggedIn;

      // check just the subloc in case there are query parameters
      final goingToLogin = state.subloc == '/login';

      // the user is not logged in and not headed to /login, they need to login
      if (!loggedIn && !goingToLogin) return '/login?from=${state.subloc}';

      // the user is logged in and headed to /login, no need to login again
      if (loggedIn && goingToLogin) return '/';

      // no need to redirect at all
      return null;
    },

    // changes on the listenable will cause the router to refresh it's route
    refreshListenable: loginInfo,
  );
}
```

In this example, if the user isn't logged in, they're redirected to `/login`
with a `from` query parameter set to the deep link. The `state` object has the
`location` and the `subloc` to choose from. The `location` includes the query
parameters whereas the `subloc` does not. Since the `/login` route may include
query parameters, it's easiest to use the `subloc` in this case (and using the
raw `location` will cause a stack overflow, an exercise that I'll leave to the
reader).

Now, when the `/login` route is matched, we want to pull the `from` parameter
out of the `state` object to pass along to the `LoginPage`:

```dart
GoRoute(
  path: '/login',
  pageBuilder: (context, state) => MaterialPage<void>(
    key: state.pageKey,
    // pass the original location to the LoginPage (if there is one)
    child: LoginPage(from: state.queryParams['from']),
  ),
),
```

In the `LoginPage`, if the `from` parameter was passed, we use it to go to the
deep link location after a successful login:

```dart
class LoginPage extends StatelessWidget {
  final String? from;
  const LoginPage({this.from, Key? key}) : super(key: key);

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

              // if there's a deep link, go there
              if (from != null) context.go(from!);
            },
            child: const Text('Login'),
          ),
        ],
      ),
    ),
  );
}
```

It's still good practice to pass in the `refreshListenable` when manually
redirecting, as we do in this case, to ensure any change to the login info
causes the right routing to happen automatically, e.g. the user logging out will
cause them to be routed back to the login page.