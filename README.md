BluemixNodeJS
=============

Deploy a Node.js Application to Bluemix

this info is used form Bluemix

Use cf buildpacks  to check  status of the Node.js buildpacks 

$ cf buildpacks 
Getting buildpacks... 
buildpack           position  enabled   locked  
nodejs_buildpack      8       true      false 
sdk-for-nodejs        2       true      false


Use the engines section in  package.json file to specify 
 version of Node.js runtime, see following example:

{ 
  "name": "myapp", 
  "description": "this is my app", 
  "version": "0.1", 
  "engines": { 
    "node": "0.10.26" 
  } 
}

To start a local a Node.js web server , you need following code inexample.js:

var http = require('http'); 

var host = process.env.VCAP_APP_HOST || 'localhost'; 
var port = process.env.VCAP_APP_PORT || 1337 

http.createServer(function (req, res) { 
  res.writeHead(200, {'Content-Type': 'text/html'}); 
  res.end('Hello Node.js! running version ' + process.version); 
}).listen(port, null); 

console.log('Server running at http://' + host + ':' + port + '/');

Now test  example.js file locally from  command line (install a node runtime to your local machine).

% node example.js 
Server running at http://127.0.0.1:1337/

To push Node.js application to Bluemix, use text file Procfile to describe startup command to run application. 
Save Procfile in root directory of application by entering following command:

web: node app.js

Here, app.js is the startup js script for your application.
If Procfile is not present, the IBM Node.js buildpack checks for a scripts.
start entry in the package.json file. If a start script entry is present, 
a Procfile is generated automatically. 
Otherwise, IBM Node.js buildpack checks for a server.js file in the root directory of your application. 
If a server.js file is found, a Procfile is also generated automatically.

{ ... "scripts": { "start": "node app.js" } }

The content of the auto-generated Procfile is shown below:

web: npm start

