# Client-server application using http api

![](https://raw.githubusercontent.com/EdoLabs/src2/master/quicktour4.svg?sanitize=true)
[](quicktour.svg)

Every developer is familiar with http api and this demo quickly illustrates how we can create a classic client-server application using http api.


### Device1 Application

#### 1. Create a device1 project directory and install m2m.
```js
$ npm install m2m
```
#### 2. Save the code below as device.js in your device1 project directory.

```js
const { Device } = require('m2m');

let device = new Device(100);

device.connect(() => {

  device.get('/update-data', (req, res) => {
    console.log('req.query', req.query);
    res.json(req.query);
  });

  device.post('/machine-control/:id/actuator/:number/action/:state', (req, res) => {
    console.log('req.params', req.params);
    console.log('req.body', req.body);
    res.json({params:req.params, body:req.body});
  });

  device.post('/update-data', (req, res) => {
    console.log('req.body', req.body);
    res.send('device 100 data updated');
  });

});
```
#### 3. Start your device1 application.
```js
$ node device.js
```
### Device2 Application

#### 1. Create a device2 project directory and install m2m.
```js
$ npm install m2m
```
#### 2. Save the code below as device.js in your device2 project directory.

```js
const { Device } = require('m2m');

let device = new Device(200);

device.connect(() => {

  device.get('/device/state', (req, res) => {
    res.json({id:100, state:'off'});
  });

  device.post('/update-data', (req, res) => {
    console.log('req.body', req.body);
    res.send('device 200 data updated');
  });

  device.post('/machine-control/:id/actuator/:number/action/:state', (req, res) => {
    console.log('req.params', req.params);
    console.log('req.body', req.body);
    res.json({params:req.params, body:req.body});
  });

});
```
#### 3. Start your device2 application.
```js
$ node device.js
```

### Client Application

#### 1. Create a client project directory and install m2m.

```js
$ npm install m2m
```

#### 2. Save the code below as client.js within your client project directory.

```js
const m2m = require('m2m');

let client = new m2m.Client();

client.connect(() => {

  client.get({id:100, path:'/update-data?name=ed&status=member'}, (data) => {
    console.log('device 100 get /update-data result', data); 
  });

  client.get({id:200, path:'/device/state'}, (data) => {
    console.log('device 200 device/state result', data); 
  });

  client.post({id:100, path:'/update-data', body:{name:'Jim', age:'30'}}, (data) => {
    console.log('device 100 post /update-data result', data); 
  });

  client.post({id:200, path:'/machine-control/m120/actuator/25/action/on', body:{id:200, state:'true'}}, (data) => {
    console.log('device 200 /machine-control result', data);
  });

});
```

#### 3. Start your client application.
```js
$ node client.js
```
