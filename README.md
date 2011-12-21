# NodeTcl

NodeTcl is a native Node extension that embeds a Tcl interpreter within the Node.js environment, allowing you to invoke Tcl commands from within JavaScript code.

This is especially useful for leveraging existing Tcl code or packages in a new Node.js application.


## Installation

Just run `make` and a shared-library named `nodetcl.node` will be created.

Compilation has only been tested on FreeBSD 8.2 with node-0.4.12 and tcl-8.5.10.


## Example

Included is an `example1.js` which contains the following:

```js
var tcl = require('./nodetcl.node');
var interp = new tcl.NodeTcl();
console.log(interp.eval("expr 6*7"));
```

Once you have built the extension, you can run it with:

```bash
  node example1.js
```

Which will print simply:

```js
  42
```
### Call vs Eval

The `call` method differs from `eval` in that the arguments are not evaluated (no need to escape special characters). It takes an arbitrary amount of arguments and executes them as a Tcl statement:

```js
interp.call("puts", "[hello world]")
```

Which will return:

```js
  [hello world]
```

`call` also accepts arrays (will be converted to lists) and simple key-value-mapping objects (will be converted to dicts) as arguments:

```js
interp.call("llength", [1, 2, 3, 4, 5])
```

Which will return:

```js
  5
```

`call` also converts return values: lists become arrays, dicts become objects and numbers are returned as numbers, not as strings.

Return values from custom procs can also have different types, just like call's arguments:

```js
interp.proc("foo", function() { return ["foo", "bar"] })
```

### Other Methods

* `getStacktrace()` returns Tcl's stacktrace of the last error that occured
* `setTimeLimit(seconds)` sets a time limit (in seconds) for all subsequent 'call' or 'eval' calls, a limit of 0 disables this
* `getTimeLimit()` returns the current time limit setting
* `makeSafe()` converts the interpreter into a safe interpreter
* `deleteProc(name)` removes a proc from the interpreter (also works on default procs, such as `exit`)

## Tcl Event Loop

Keep the Tcl event loop alive to handle Tcl after timers, file events, etc, in an asynchronous Node-compatible style by periodically invoking `interp.process_events()`.

## Known Limitations

* the Tcl event loop is not invoked after eval returns, so any Tcl timers or events will not be triggered.