tiny-kubernetes
===============

`tiny-kubernetes` provides a thin wrapper on top of [requests][0] to make it
easier to work with Kubernetes APIs.

Features
--------

 * Automatic authentication (`$KUBECONFIG`)
 * Simplified response traversal (`never['do']['this']['again']`)
 * Sane exceptions
 * No crazy complex client-side APIs to deal with

Why?
----

The official [kubernetes][1] package is, uh, complex. While it does support all
the advanced features the Kubernetes API offers (like WebSocket streaming) it is
often non-trivial to work with. tiny-kubernetes aims to provide most of the same
functionality with minimal overhead.

Installation
------------

To manually install, run:
```
pip install git+https://github.com/monasca/tiny-kubernetes.git
```

To depend on tiny-kubernetes from `requirements.txt`, add the following:

```
-e git+https://github.com/monasca/tiny-kubernetes.git#egg=tiny-kubernetes
```

(note: requires pip 7.0 or greater)

You can depend on a specific commit too:

```
-e git+https://github.com/monasca/tiny-kubernetes.git@[GIT SHA]#egg=tiny-kubernetes
```

Usage
-----

```python
from tiny_kubernetes import KubernetesAPIClient

client = KubernetesAPIClient()
client.load_auto_config()

pods = client.get('/api/v1/namespaces/{}/pods', 'default')
for pod in pods['items']:
  print pod.metadata.name
```

Notice that you can access (almost) all parts of the Kubernetes API response
using dot accessors (courtesy of [DotMap][2]). As `.items()` is a reserved name
in dictionaries, it needs to be accessed more traditionally, but most other
member names can use the shortened syntax.

The endpoint is formatted using [`str.format`][3] with the remaining positional
arguments. All keyword arguments are passed through to requests's
[`Session.request`][4], so you can include a body with `json={...}` or URL
parameters with `params={...}`.

A special `json_patch` method exists to help perform patches. It works around
a few potential requests bugs automatically. For example, it can be used to
append labels to a resource:

```python
client.json_patch([{
  'op': 'add',
  'path': '/metadata/labels/foo',
  'value': 'bar'
}], '/api/v1/namespaces/{}/pods/{}', 'default', 'some-pod')
```

Notes
-----

 * Temporary files may be created if using a local `$KUBECONFIG` with
   base64-encoded certificates. Due to limitations in python's SSL library these
   must be decoded and written to the filesystem to be used. They should be
   cleaned up when the interpreter exits.


[0]: http://docs.python-requests.org/en/master/
[1]: https://github.com/kubernetes-incubator/client-python
[2]: https://github.com/drgrib/dotmap
[3]: https://docs.python.org/2/library/stdtypes.html#str.format
[4]: http://docs.python-requests.org/en/master/api/#requests.Session.request
