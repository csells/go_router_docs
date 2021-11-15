---
title: Multiple Redirections
date: Last Modified 
permalink: /multi-redirection/index.html
eleventyNavigation:
  key: Multiple Redirections
  order: 21
  parent: Redirection
---
It's possible to redirect multiple times w/ a single navigation, e.g. ```/ =>
/foo => /bar```. This is handy because it allows you to build up a list of
routes over time and not to worry so much about attempting to trim each of them
to their direct route. Furthermore, it's possible to redirect at the top level
and at the route level in any number of combinations.

If you redirect too many times, that's likely to indicate a bug in your app. By
default, more than 5 redirections will cause an exception. You can change this
by setting the `redirectLimit` argument to the `GoRouter` constructor.

The other trouble you need worry about is getting into a loop, e.g. ```/ => /foo
=> /```. If that happens, you'll get an exception.