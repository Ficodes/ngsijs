# NGSI.js

[![License](https://img.shields.io/badge/license-AGPLv3+%20with%20classpath--like%20exception-blue.svg)](LICENSE)
[![Documentation Status](https://img.shields.io/badge/docs-stable-brightgreen.svg?style=flat)](https://ficodes.github.io/ngsijs/stable)
[![Tests](https://github.com/Ficodes/ngsijs/actions/workflows/unittests.yml/badge.svg)](https://github.com/Ficodes/ngsijs/actions/workflows/unittests.yml)
[![Coverage Status](https://coveralls.io/repos/github/Ficodes/ngsijs/badge.svg?branch=master)](https://coveralls.io/github/Ficodes/ngsijs?branch=master)

NGSI.js is the JavaScript library used by
[WireCloud](https://github.com/Wirecloud/wirecloud) for adding FIWARE NGSI
capabilities to widgets and operators. However, this library has also been
designed to be used in other environments as normal web pages and
clients/servers running on Node.js.

This library has been developed following the FIWARE
[NGSI v1](https://telefonicaid.github.io/fiware-orion/api/v1/),
[NGSI v2](https://fiware.github.io/specifications/ngsiv2/stable/) and the
[NGSI-LD](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.03.01_60/gs_CIM009v010301p.pdf) specifications
and has been tested to work against version 0.26.0+ of the Orion Context Broker.

Reference documentation of the API is available at
[http://ficodes.github.io/ngsijs/stable/NGSI.html](http://ficodes.github.io/ngsijs/stable/NGSI.html).


## Using NGSI.js from normal web pages

> **Note**: Support for Cross-Origin Resource Sharing (CORS) has been added on
> orion 1.10.0. This support must be enabled to access the context broker from a
> web page in a different domain than the context broker.
>
> CORS support can also be enabled by accessing to the context broker server
> using some proxies, like [cors-anywhere](https://www.npmjs.com/package/cors-anywhere)
> or the [FIWARE's pep proxy](https://github.com/ging/fiware-pep-proxy).
>
> You can access any context broker server (without requiring CORS support and
> regardless of the context broker version) if the context broker is accessible
> throught the same domain as the web page. How to create such configuration is
> out of the scope of this documentation.


Just include a `<script>` element linking to the `NGSI.min.js` file:

```html
<script type="text/javascript" src="url_to_NGSI.js"></script>
```

Once added the `<script>` element, you will be able to use all the features
provided by the NGSI.js library (except receiving notifications):

```javascript
const connection = new NGSI.Connection("http://orion.example.com:1026");
connection.v2.listEntities().then((response) => {
    response.results.forEach((entity) => {
        console.log(entity.id);
    });
});
```

This example will display the `id` of the first 20 entities. See the
documentation of the [`listEntities`](http://ficodes.github.io/ngsijs/stable/NGSI.Connection.html#.%22v2.listEntities%22__anchor)
method for more info.

To be able to receive notifications inside a web browser the library requires
the use of a [ngsi-proxy](https://github.com/conwetlab/ngsi-proxy) server. You
can use your own instance or the `ngsi-proxy` instance available at
`https://ngsiproxy.lab.fiware.org`.

```javascript
const connection = new NGSI.Connection("http://orion.example.com:1026", {
    ngsi_proxy_url: "https://ngsiproxy.lab.fiware.org"
});
```


## Using NGSI.js from Node.js

```bash
$ npm install ngsijs
```

After installing the NGSI.js node module, you will be able to use the API as usual:

```javascript
const NGSI = require('ngsijs');
const connection = new NGSI.Connection("http://orion.example.com:1026");
connection.v2.listEntities().then((response) => {
    response.results.forEach((entity) => {
        console.log(entity.id);
    });
});
```

> **Note:** Node.js doesn't require the usage of a ngsi-proxy as you can create
> an HTTP endpoint easily (e.g. using [express]). Anyway, you can use it if you
> want, you only have to take into account that is better to directly provide
> the HTTP endpoint to reduce the overhead.

[express]: https://expressjs.com/


## Using NGSI.js from WireCloud widgets/operators

WireCloud already provides some components (widgets, operators and mashups)
allowing NGSI connectivity. E.g.:

- [NGSI Source operator](https://github.com/Wirecloud-fiware/ngsi-source-operator)
- [NGSI Browser widget](https://github.com/wirecloud-fiware/ngsi-browser-widget)
- [NGSI datamodel 2 PoI operator](https://github.com/wirecloud-fiware/ngsi-datamodel2poi-operator)

Anyway, WireCloud uses NGSI.js as the binding for connecting to context
brokers. If you need to create a new specific component you can take a look into
the "3.2.1. Using Orion Context Broker" tutorial available at the
[FIWARE Academy].

[FIWARE Academy]: http://edu.fiware.org/course/view.php?id=53#section-3
