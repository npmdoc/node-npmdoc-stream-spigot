# api documentation for  [stream-spigot (v3.0.6)](https://github.com/brycebaril/node-stream-spigot#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-stream-spigot.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-stream-spigot) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-stream-spigot.svg)](https://travis-ci.org/npmdoc/node-npmdoc-stream-spigot)
#### A readable stream generator, useful for testing or converting simple functions into Readable streams.

[![NPM](https://nodei.co/npm/stream-spigot.png?downloads=true)](https://www.npmjs.com/package/stream-spigot)

[![apidoc](https://npmdoc.github.io/node-npmdoc-stream-spigot/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-stream-spigot_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-stream-spigot/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-stream-spigot/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-stream-spigot/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Bryce B. Baril"
    },
    "browser": {
        "readable-stream/readable": "_stream_readable"
    },
    "bugs": {
        "url": "https://github.com/brycebaril/node-stream-spigot/issues"
    },
    "dependencies": {
        "readable-stream": "~2.2.6",
        "xtend": "~4.0.0"
    },
    "description": "A readable stream generator, useful for testing or converting simple functions into Readable streams.",
    "devDependencies": {
        "concat-stream": "~1.6.0",
        "tape": "~4.6.3"
    },
    "directories": {
        "test": "test"
    },
    "dist": {
        "shasum": "df87da2630221682b13d94f1ef63ae56a4d7cefa",
        "tarball": "https://registry.npmjs.org/stream-spigot/-/stream-spigot-3.0.6.tgz"
    },
    "gitHead": "adb93c2772a57d03b09b53b771ac0dd8498af722",
    "homepage": "https://github.com/brycebaril/node-stream-spigot#readme",
    "keywords": [
        "streams2",
        "testing",
        "readable"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "bryce",
            "email": "bryce@ravenwall.com"
        }
    ],
    "name": "stream-spigot",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/brycebaril/node-stream-spigot.git"
    },
    "scripts": {
        "test": "node test/"
    },
    "version": "3.0.6"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module stream-spigot](#apidoc.module.stream-spigot)
1.  [function <span class="apidocSignatureSpan">stream-spigot.</span>array (options, array)](#apidoc.element.stream-spigot.array)
1.  [function <span class="apidocSignatureSpan">stream-spigot.</span>ctor (options, _read)](#apidoc.element.stream-spigot.ctor)
1.  [function <span class="apidocSignatureSpan">stream-spigot.</span>sync (options, fn)](#apidoc.element.stream-spigot.sync)



# <a name="apidoc.module.stream-spigot"></a>[module stream-spigot](#apidoc.module.stream-spigot)

#### <a name="apidoc.element.stream-spigot.array"></a>[function <span class="apidocSignatureSpan">stream-spigot.</span>array (options, array)](#apidoc.element.stream-spigot.array)
- description and source-code
```javascript
function array(options, array) {
  if (Array.isArray(options)) {
    array = options
    options = {}
  }

  return make(options, _shifter(array))
}
```
- example usage
```shell
...


A generator for (streams2) Readable streams, useful for testing or converting simple lazy functions into Readable streams, or just
 creating Readable streams without all the boilerplate.

'''javascript
var spigot = require("stream-spigot")

spigot.array(["ABCDEFG"]).pipe(process.stdout)
// ABCDEFG

spigot.array(["ABC", "DEF", "G"]).pipe(process.stdout)
// same as: (short form)
spigot(["ABC", "DEF", "G"]).pipe(process.stdout)
// ABCDEFG
...
```

#### <a name="apidoc.element.stream-spigot.ctor"></a>[function <span class="apidocSignatureSpan">stream-spigot.</span>ctor (options, _read)](#apidoc.element.stream-spigot.ctor)
- description and source-code
```javascript
function ctor(options, _read) {
  if (_read == null) {
    _read    = options
    options   = {}
  }

  if (Array.isArray(_read))
    _read = _shifter(_read)

  if (typeof _read != "function")
    throw new Error("You must implement an _read function for Spigot")

  function Spigot (override) {
    if (!(this instanceof Spigot))
      return new Spigot(override)

    this.options = xtend(options, override)
    Readable.call(this, this.options)
  }

  inherits(Spigot, Readable)

  Spigot.prototype._read = _read

  return Spigot
}
```
- example usage
```shell
...
Create a Readable stream instance with the specified _read method. Your _read method should follow the normal [stream.Readable _read
](http://nodejs.org/api/stream.html#stream_readable_read_size_1) syntax. (I.e. it should call 'this.push(chunk)')

spigot([options, ], array)
---

Create a Readable stream instance that will emit each member of the specified array until it is consumed. Creates a copy of the
given array and consumes that -- if this will cause memory issues, consider implementing your own _read function to consume your
 array.

var Spigot = spigot.ctor([options,], _read)
---

Same as the above except provides a constructor for your Readable class. You can then create instances by using either 'var source
 = new Spigot()' or 'var source = Spigot()'.

var Spigot = spigot.ctor([options,], array)
---
...
```

#### <a name="apidoc.element.stream-spigot.sync"></a>[function <span class="apidocSignatureSpan">stream-spigot.</span>sync (options, fn)](#apidoc.element.stream-spigot.sync)
- description and source-code
```javascript
function sync(options, fn) {
  if (typeof options == "function") {
    fn = options
    options = {}
  }
  var toAsync = function toAsync() {
    var self = this
    setImmediate(function later() {
      var val = fn()
      if (val === undefined) {
        val = null
      }
      self.push(val)
    })
  }
  return make(options, toAsync)
}
```
- example usage
```shell
...
var count = 0
function gen() {
  if (count++ < 5) {
    return {val: count}
  }
}

spigot.sync({objectMode: true}, gen).pipe(...)
/*
{val: 1}
{val: 2}
{val: 3}
{val: 4}
{val: 5}
*/
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
