# Client-server application using http api

![](https://raw.githubusercontent.com/EdoLabs/src2/master/quicktour4.svg?sanitize=true)
[](quicktour.svg)

Every developer is familiar with http api and this demo quickly illustrates how we can create a classic client-server application using http.


### Device1

##### 1. Create a device1 project directory and install m2m.
```js
$ npm install m2m
```
##### 2. Save the code below as device.js in your device1 project directory.

```js
const { Device } = require('m2m');

let device = new Device(100);

device.connect(() => {

  device.get('/device/state', (req, res) => {
    res.send('off');
  });

  device.post('/machine-control/:id/actuator/:number/action/:state', (req, res) => {
    console.log('req.body', req.body);
    let body = JSON.stringify(req.body);
    res.send({id:req.params.id, num:req.params.number, state:req.params.state, body:body});
  });

});
```
##### 3. Start your device1 application.
```js
$ node device.js
```
### Device2

##### 1. Create a device2 project directory and install m2m.
```js
$ npm install m2m
```
##### 2. Save the code below as device.js in your device2 project directory.

```js
const { Device } = require('m2m');

const device = new Device(200);

device.connect(() => {

  device.get('/device/state/status', (req, res) => {
    res.send('on');
  });

  device.post('/update-data', (req, res) => {
    console.log('req.body', req.body);
    res.send({path:'/update-data'});
  });

  device.post('/machine-control/:id/actuator/:number/action/:state', (req, res) => {
    console.log('req.body', req.body);
    res.send({id:req.params.id, num:req.params.number, state:req.params.state});
  });

});
```
##### 3. Start your device2 application.
```js
$ node device.js
```

### Client Application

##### 1. Create a client project directory and install m2m.

```js
$ npm install m2m
```

##### 2. Save the code below as client.js within your client project directory.

```js
const m2m = require('m2m');

let client = new m2m.Client();

client.connect(() => {

  client.get({id:100, path:'/device/state'}, (data) => {
    console.log('device 100 state', data); 
  });

  client.get({id:200, path:'/device/state/status'}, (data) => {
    console.log('device 200 state', data); 
  });

  client.post({id:100, path:'/update-data', body:{name:'Jim', age:'30'}}, (data) => {
    console.log('device 100 post /update-data result', data); 
  });

  client.post({id:200, path:'/machine-control/m120/actuator/25/action/on', body:{id:200, state:'true'}}, (data) => {
    console.log('machine ' +data.id +' actuator '+data.num +' is ' +data.state);  
  });

});
```

##### 3. Start your application.
```js
$ node client.js
```
