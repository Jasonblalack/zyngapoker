Jasy comes with a built-in web server for delivering content easily without setting up a system server or requiring administrator rights. The Webserver component is based on the excellent and simple to use [CherryPy](http://www.cherrypy.org).

## Features

* Custom top-level routes
* Aliasing local folders via routes feature
* Delivering static local files
* Super easy setup and usage
* Mirroring feature to fasten delivery from slower remote servers (GET requests only)
* Offline support to send `404` when files are not mirrored
* Integrated BasicAuth support via headers or route configuration
* All responses are *CORS* enabled for cross domain usage
* Integrated into Jasy logging infrastructure

## Usage

Just call the globally available `serve()` method. There are no required parameters. By default (when no custom routes are configured) it just delivers all files from the project's root folder. In the simplest use case just add a task like that one:

```python
@task
def server():
  serve()
```

### Parameters

* `routes`: Configure the routes. A map/dict where the key is the top-level name of the route and the value holds the configuration options.
* `port`: Supports any valid port. If you run this script as a user with normal privileges (recommended), you might not have access to start a port on a low port number. Low port numbers are reserved for the administrator/superuser (root).
* `host`: Any valid IP to bind to. Defaults to `127.0.0.1` which means that `localhost` is supported as well. Use `0.0.0.0` or your external IP address to bind to all addresses and making your server reachable from other computers as well.


### Routes