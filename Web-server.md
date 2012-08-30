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

Just call the globally available `serve()` method.

```python
@task
def server():
  serve(routes?, port?, host?)
```

There are no required parameters. By default (when no custom routes are configured) it just delivers all files from the project's root folder. In the simplest use case just add a task like that one:

```python
@task
def server():
  serve()
```

