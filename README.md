[![Build Status](https://travis-ci.org/Eyevinn/hls-ts-js.svg?branch=master)](https://travis-ci.org/Eyevinn/hls-ts-js)
[![Coverage Status](https://coveralls.io/repos/github/Eyevinn/hls-ts-js/badge.svg?branch=master)](https://coveralls.io/github/Eyevinn/hls-ts-js?branch=master)

A Javascript library to parse Apple HLS MPEG Transport Stream segments

## Usage (Node JS)

```
npm install --save hls-ts
```

The library implements the `Writable` stream interface and acts a a "sink". For example to download
and parse an HLS MPEG Transport Stream segment:

```
const request = require("request");
const hlsTs = require("hls-ts");

request.get("http://example.com/seg10.ts")
.pipe(hlsTs.parse())
.on("finish", function() {

  // Obtain all programs found in the Transport Stream
  const programs = hlsTs.programs;

  // Obtain all packets for a specific program stream
  const avcPackets = hlsTs.getPacketsByProgramType("avc");

  // Obtain the payload for a program stream
  const avcPayload = hlsTs.getDataStreamByProgramType("avc");

  // where avcPayload.data is a Uint8Array
});
```

## Usage (Browser version)

```
<script src="dist/hls-ts.min.js"></script>
<script>
  var xhr = new XMLHttpRequest();
  var url = "http://example.com/hls/seg-10s.ts";
  var parser = new window.HlsTs({ debug: false });
  xhr.responseType = "arraybuffer";
  xhr.onloadend = function() {
    var buffer = xhr.response;
    var data = new Uint8Array(buffer);
    parser.parse(data).then(function() {
      var avcData = parser.getDataStreamByProgramType("avc");
      console.log(avcData.data.length);
    }).catch(function(err) { console.error(err.message); }).then(done);
  };
  xhr.open("GET", url);
  xhr.send();
</script>
```

## Contributing
All contributions are welcome but before you submit a Pull Request make sure you follow the same
code conventions and that you have written unit tests
