# Stream to ______ [![Build Status](https://travis-ci.org/jonathanong/stream-to.png)](https://travis-ci.org/jonathanong/stream-to)

Concatenate a readable stream's data into either a single buffer, string, or array.

You may also be interested in:

- [cursor-methods](https://github.com/jonathanong/cursor-methods)
- [cat-stream](https://github.com/jonathanong/cat-stream)

## API

```js
var streamTo = require('stream-to')
var stream = fs.createReadStream('some file.txt')
```

### streamTo.array(stream, callback(err, arr))

Returns all the data objects in an array.
This is useful for streams in object mode if you want to just use an array.

```js
streamTo.array(stream, function (err, arr) {
  assert.ok(Array.isArray(arr))
})
```

### streamTo.buffer(stream, callback(err, buffer))

Returns all the data objects concatenated into a single buffer.

```js
streamTo.buffer(stream, function (err, buffer) {
  assert.ok(Buffer.isBuffer(buffer))
})
```

### streamTo.string(stream, callback(err, string))

Returns all the data objects concatenated into a single buffer.
The source stream _should_ emit strings.
Otherwise, you should juse use `streamTo.buffer()`.

```js
stream.setEncoding('utf8')
streamTo.string(stream, function (err, string) {
  assert.ok(typeof string === 'string')
})
```

vs.

```js
streamTo.buffer(stream, function (err, buffer) {
  assert.ok(typeof buffer.toString('utf8') === 'string')
})
```

Of course, you don't always get to choose the encoding of the source stream.

### Destroyed streams

If a stream if destroyed (ie `close` is emitted) before it ends,
then the callback will be returned, but no value will be.
Thus, you should check that the value is valid.

```js
streamTo.buffer(stream, function (err, buffer) {
  if (err)
    callback(err)
  else if (!buffer)
    callback(new Error('The stream was destroyed.'))
  else
    callback(null, buffer)
})
```

## License

The MIT License (MIT)

Copyright (c) 2013 Jonathan Ong me@jongleberry.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
