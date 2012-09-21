Jasy comes with a built-in web server for delivering content easily without setting up a system server or requiring administrator rights. The web server component is based on the excellent and simple to use [CherryPy](http://www.cherrypy.org).

## Features

* Super easy setup and usage
* Custom top-level routes
* Custom content types (MIME-types) 
* Delivering static local files
* Aliasing local folders via routes feature
* Proxying requests to remote server via local route
* Mirroring feature to fasten delivery from slower remote servers (GET requests only)
* Offline support to send `404` when files are not mirrored
* Integrated Basic Authentification support via headers or route configuration
* All responses are *CORS* enabled for cross domain usage
* Integrated into Jasy logging infrastructure

## Basic Usage

Just call the globally available `serve()` method. There are no required parameters. By default (when no custom routes are configured) it just delivers all files from the project's root folder. In the simplest use case just add a task like that one:

```python
@task
def server():
  Server().start()
```

## Parameters

* `port`: Supports any valid port. If you run this script as a user with normal privileges (recommended), you might not have access to starting the server on a low port number (< 1024). Low port numbers are reserved for the administrator/superuser (root).
* `host`: Any valid IP to bind to. Defaults to `127.0.0.1` which means that `localhost` is supported as well. Use `0.0.0.0` to bind to all interfaces, or a specific IP address to bind to a particular interface, and make your server reachable from non-local clients as well.
* `mimeTypes`: Configure the custom content/MIME -types. A map/dict where the key is the file extension and the value holds the custom content type.

*Example*:

```python
@task
def server():
  Server(
    mimeTypes = {
      "manifest": "text/cache-manifest",
      "js": "application/javascript"
    }, 
    host = "0.0.0.0", 
    port = 1234
  ).start()
```


## Routes

You are able to add custom routes to your server before starting it:

```python
@task
def server():
  http = Server(
    mimeTypes = {
      "manifest": "text/cache-manifest",
      "js": "application/javascript"
    }, 
    host = "0.0.0.0", 
    port = 1234
  )

  http.setRoutes({
    "name" : config
  })

  http.start()
```

The parameter to `setRoutes()` is a map/dict where the key is the top-level name (e.g. `myroute` → `http://localhost/myroute`) of the route and the value holds the configuration options.


### Static Routes

These are the valid configuration parameters:

* `root`: Define the folder to map to the given route e.g. `"api/v08"`
* `debug` (`=False`): Enable debug mode

*Example*:

```python
"api" : {
  "root" : "api/v08",
  "debug" : True
}
```

When debugging is enabled every request is logged and printed out to the console.

### Proxy Routes

These are the valid configuration parameters:

* `host`: Defines the host name / address to mirror
* `debug` (`=False`): Enable debug mode
* `auth` (`=False`): Use the given basic auth data
* `mirror` (`=False`): Enable dynamic mirroring of all remotely loaded files (`GET` only). Leads to the creation of a file "jasymirror-ROUTENAME" inside the root folder of the application.
* `offline` (`=False`): Don't load files from the remote host - only deliver files from mirror cache

*Note*: *SSL* verification is disabled for the proxying so that all proxied server answers are regarded as okay even if the certificate is invalid. This is because most test servers don't have valid *SSL* certificates and the Jasy web server is not planned being used in production anyway.

*Example:*

```python
"github" : {
  "debug" : True,
  "host" : "https://api.github.com",
  "mirror" : True,
  "offline" : False,
  "auth" : {
    "method" : "basic",
    "user": "myname",
    "password": "mypass"
  }
}
```

This route forwards all requests from ˚http://localhost/github/*` to `https://api.github.com/*` and uses the authentication data configured for the route. It automatically mirrors all data from `GET` requests.

When debugging is enabled every request is logged and printed out to the console.


#### Using HTTP headers

Just pass the headers to the proxy. It will forward them 1:1 to the proxied server. This can be used e.g. via the jQuery `headers` object during `beforeSend`:

```js
$.ajax({
  url: "http://fiddle.jshell.net/favicon.png",
  beforeSend: function (xhr) {
    xhr.headers["X-My-Header"] = "foo-bar"
  }
}).done(function(data) { ... });
```


#### Enabling Basic Authentification

There are two way to enable Basic Auth for remote hosts. Either you can define a header `X-Proxy-Authorization` on every request or define the authentication data inside the route via the `auth` key (a dict with the keys `method` (should be `basic`), `user` and `password`). The header `X-Proxy-Authorization` is automatically mapped to `Authorization` on the mirrored request and must qualify for the normal Basic Auth criteria (username and password in a single string which is base64 encoded).


#### Resetting the mirror

To clear the mirror cache you can delete the `.jasy/mirror-ROUTENAME` (e.g. `.jasy/mirror-github`) from the root folder of your application. Stop the server first to do so.