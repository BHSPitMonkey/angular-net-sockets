# Angular Net Sockets

A simple Angular.JS  module that provides a cross-vendor-compatible interface
for using TCP/UDP sockets.

## Setup

1. Place net-sockets.js somewhere in your app (e.g. "lib/angular-net-sockets/").
2. Include the net-socket.js file in your HTML using a `<script>` tag (or
   however you are currently getting your scripts loaded into the page).
3. Add `stepheneisenhauer.netsockets` to the list of dependencies in your app
   module.

## Usage

Inject the NetSocket object as a dependency in your controller/service/etc. and
instantiate it using `new`:

    angular.module('myApp.services', []).
      factory('MyService', function(NetSocket) {
        var socket = new NetSocket({...});
        ...
      });

The NetSocket constructor takes an "options" object which must be present,
though it can be empty. In practice you will most likely want to listen for
callbacks from the socket, which you define in the options object. For example:

    var socket = new NetSocket({
      protocol: "tcp",
      onCreate: onSocketCreate,
      onConnect: onSocketConnect,
      onReceive: parseMessages,
      onSend: onSocketSend,
      onError: onSocketError,
      onDisconnect: onSocketDisconnect,
      onClose: onSocketClose,
    });

These options are described in further detail in the following section.

### Options/Callbacks

* `protocol`: A string, either `"tcp"` or `"udp"`. Note that UDP support is
  mostly nonexistent in this version. Defaults to `"tcp"` if not specified.
* `onCreate`: A function that is called when the socket is finished being
  created. The function will receive a boolean parameter indicating if the
  creation was successful.
* `onConnect`: A function that is called when the socket is finished connecting
  (after `connect(...)` is called). The function will receive a boolean
  parameter indicating if the connection attempt was successful.
* `onReceive`: A function that is called when the socket receives some data. The
  function will receive an ArrayBuffer object as a parameter.
* `onSend`: A function that is called when a write operation (using `write(...)`
  has completed. The function will receive a boolean parameter indicating if the
  write was successful.
* `onError`: A function that is called when the socket has reported some kind of
  error. The function will receive a string parameter describing the error.
* `onDisconnect`: A function that is called when the socket has disconnected.
* `onClose`: A function that is called when the socket is finished being closed.

### Methods

* `connect(host, port)`: Connect to the host on the specified port using the
  protocol that was specified when the socket was created. The `onConnect`
  callback will tell you whether the connection attempt succeeded or failed.
* `send(data)`: Write the contents of `data` (which should be an ArrayBuffer)
  to the socket.
* `disconnect()`: Disconnect the socket from its current connection.
* `close()`: Closes the socket, removing it from the OS and rendering it no
  longer usable.

## Status

TCP socket connections are supported on all platforms and appear to work
reliably. UDP is not yet completely supported.

## Supported Platforms

* **Node-WebKit** (using the node.js
  [net](http://nodejs.org/api/net.html)
  module)
* **Chrome Packaged Apps** (using the
  [sockets](https://developer.chrome.com/apps/app_network)
  API)
* **Firefox Packaged Apps** (using the
  [mozTCPSocket](https://developer.mozilla.org/en-US/docs/WebAPI/TCP_Socket)
  API)

## License

MIT
