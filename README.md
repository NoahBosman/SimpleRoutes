# SimpleRoutes

What is SimpleRoutes? Well, it's a small framework that helps to create custom routes in WordPress.

### Custom Routing

SimpleRoutes supports custom routing. I could go into this in more depth, but here are some basic examples.

__A simple redirect__

This redirects from /old-page to /new-page
``` PHP
(new SimpleRoutes\Route)->url('old-page')
    ->redirect('/new-page');
```

__A simple custom route__

This will use Controllers\Custom()'s showPage method for any calls to sites with /custom-page/:id such as /custom-page/1, /custom-page/450, etc. The variable can be access in the controller via $this->params['id']. The call method here tells the route to execute.
``` PHP
(new SimpleRoutes\Route)->url('custom-page/:id')
    ->controller('Controllers\Custom@showPage')
    ->call();
```

__Accepted Methods__

You can define if the URL should accept GET and/or POST. You can also define 2 routes for the same url with different methods. Hoping to expand this out to other methods such as PUT, DELETE and PATCH.

Accept both GET and POST
``` PHP
(new SimpleRoutes\Route)->url('custom-page/:id')
    ->methods(array('GET', 'POST'))
    ->controller('Controllers\Custom@showPage')
    ->call();
```

Use one controller for GET and another for POST to, for example, display a form (showPage method) and process it (processSignup)
``` PHP
(new SimpleRoutes\Route)->url('sign-up')
    ->methods(array('GET'))
    ->controller('Controllers\Custom@showPage')
    ->call();

(new SimpleRoutes\Route)->url('sign-up')
    ->methods(array('POST'))
    ->controller('Controllers\Custom@processSignup')
    ->call();
```

__Closure__

``` PHP
(new SimpleRoutes\Route)->url('custom-page/:id')
    ->closure(function () {
        // Some function code
    });
```

__Middleware__

This is has very limitedly been tested and set up so may still need some work but you can call class methods that to process before the routes controller gets called to, for example, authenticate a logged in user. This example calls (new Auth)->authorize() and redirects to a login page if the auth fails or continues on to SecretPage if it authenticates user.
``` PHP
(new SimpleRoutes\Route)->url('custom-page/:id')
    ->methods(array('GET', 'POST'))
    ->middleware('Auth@authorize')
    ->controller('Controllers\SecretPage@showPage')
    ->call();
```
