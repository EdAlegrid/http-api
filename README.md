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

let myData = 'myData';

device.connect('https://dev.node-m2m.com', () => {

  device.setGpio({mode:'input', pin:[11, 13]});
  device.setGpio({mode:'output', pin:[33, 35]});

  device.setData('get-data', (data) => {
    data.send(myData);
  });

  device.setData('send-data', (data) => {
    if(data.payload){
      myData = data.payload;
      data.send(data.payload);
    }
  });

  // error listener
  device.on('error', (err) => console.log('error:', err))
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

device.connect('https://dev.node-m2m.com', () => {
  device.setGpio({mode:'out', pin:[33, 35]}, gpio => console.log(gpio.pin, gpio.state));
  
  device.setData('random-number', (data) => {
    let r = Math.floor(Math.random() * 100) + 25;
    data.send(r);
    console.log('random', r);
  });
});
```
##### 3. Start your device2 application.
```js
$ node device.js
```
## Option2 - Remote Devices Setup using Windows or Linux
#### Remote Device1
##### Here, we don't need to install array-gpio instead the gpio output will run in simulation mode.
##### 1. Create a device project directory and install m2m inside the directory.
```js
$ npm install m2m
```
##### 2. Save the code below as device.js in your device project directory.

```js
const { Device } = require('m2m');

let device = new Device(100);

let myData = 'myData';

device.connect('https://dev.node-m2m.com', () => {

  device.setGpio({mode:'input', pin:[11, 13], type:'simulation'});
  device.setGpio({mode:'output', pin:[33, 35], type:'simulation'});

  device.setData('get-data', (data) => {
    data.send(myData);
  });

  device.setData('send-data', (data) => {
    if(data.payload){
      myData = data.payload;
      data.send(data.payload);
    }
  });

  // error listener
  device.on('error', (err) => console.log('error:', err))
});
```
##### 3. Start your device application.
```js
$ node device.js
```
#### Remote Device2

##### 1. Create a device project directory and install m2m inside the directory.
```js
$ npm install m2m
```
##### 2. Save the code below as device.js in your device project directory.

```js
const { Device } = require('m2m');

const device = new Device(200);

device.connect('https://dev.node-m2m.com', () => {
  device.setGpio({mode:'out', pin:[33, 35], type:'simulation'}, gpio => console.log(gpio.pin, gpio.state));
  
  device.setData('random-number', (data) => {
    let r = Math.floor(Math.random() * 100) + 25;
    data.send(r);
    console.log('random', r);
  });
  
});
```
##### 3. Start your device application.
```js
$ node device.js
```

### Client Application

##### 1. Create a client project directory and install m2m.

```js
$ npm install m2m
```

##### 2. Save the code below as client.js within your client project directory.

**Method 1**

```js
const m2m = require('m2m');

let client = new m2m.Client();

client.connect(() => {

  // capture 'random-number' data using a pull method
  client.getData({id:100, channel:'random-number'}, (data) => {
    console.log('getData random-number', data); // 97
  });

  // capture 'random-number' data using a push method
  client.watchData({id:100, channel:'random-number'}, (data) => {
    console.log('watch random-number', data); // 81, 68, 115 ...
  });

  // update test-data
  client.sendData({id:100, channel:'test-data', payload:'node-m2m is awesome'}, (data) => {
    console.log('sendData test-data', data);
  });

  // capture updated test-data
  client.getData({id:100, channel:'test-data'}, (data) => {
    console.log('getData test-data', data); // node-m2m is awesome
  });

});
```

##### 3. Start your application.
```js
$ node client.js
```
