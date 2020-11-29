# Enabling HTTPS protocol for Localhost

Scripts for generating HTTPS certificate for your local environment.

## How-to

1. Clone this repository and `cd` into it:

```
git clone https://github.com/Vishal-SIH/Local-Host-HTTPS
cd Local-Host-HTTPS
```
2. Root Certificates can be created by running the script below:

```
sh createRootCA.sh
```

3. The root certificate just generated, should be added to the list of trusted certificates. This step depends on the operating system you're running:

    - **Linux**: Depending on your Linux distribution, you can use `trust`, `update-ca-certificates` or another command to mark the generated root certificate as trusted.

*Note*: You may need to restart your browser to load the newly trusted root certificate correctly.

4. Run the script to create a domain certificate for `localhost`: 

```
sh createSelfSigned.sh
```

5. Move `server.key` and `server.crt` to an accessible location on your server and include them when starting it. In an Express app running on Node.js, you'd do something like this:

```js
var path = require('path')
var fs = require('fs')
var express = require('express')
var https = require('https')

var certOptions = {
  key: fs.readFileSync(path.resolve('build/cert/server.key')),
  cert: fs.readFileSync(path.resolve('build/cert/server.crt'))
}

var app = express()

var server = https.createServer(certOptions, app).listen(443)
```
