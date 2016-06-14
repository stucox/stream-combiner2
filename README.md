# stream-combiner2

This is a sequel to
[stream-combiner](https://npmjs.org/package/stream-combiner)
for streams3.

``` js
var Combine = require('stream-combiner2')
```

## Combine (stream1,...,streamN)

Turn a pipeline into a single stream. `Combine` returns a stream that writes to the first stream
and reads from the last stream.

Streams1 streams are automatically upgraded to be streams3 streams.

Listening for 'error' will recieve errors from all streams inside the pipe.

```js
var Combine = require('stream-combiner2')
var es      = require('event-stream')

combine(                                  // connect streams together with `pipe`
  process.openStdin(),                    // open stdin
  es.split(),                             // split stream to break on newlines
  es.map(function (data, callback) {      // turn this async function into a stream
    var repr = inspect(JSON.parse(data))  // render it nicely
    callback(null, repr)
  }),
  process.stdout                          // pipe it to stdout !
)
```

## Combine.obj (stream1,...,streamN)

Same as `Combine()`, but for streams in [object mode](https://nodejs.org/api/stream.html#stream_object_mode).

```js
var Combine      = require('stream-combiner2')
var objectStream = require('object-stream')

Combine.obj(                              // connect streams together with `pipe`
  objectStream.fromArray([1, 2, 3]),      // generate objects
  objectStream.map(function (value) {     // transform them
    return value * 2
  }),
  objectStream.save(function (value) {    // save them
    /* save item... */
  })
)
```

## License

MIT
