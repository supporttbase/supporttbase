# TerraBase Sensor API
Endpoints for sensor manufacturers to interface with TerraBase to submit real-time sensor data.

Base URL: https://sensor.terrabase.com

Interactive swagger interface for full API is available at https://sensor.terrabase.com/index.html.
Access to the API has to be granted by TerraBase as it is not open to the public.

## /api/v1/Authenticate/login
*HTTP POST*

JWT Authentication, good for 24 hours.  Login with a username and password.

**Request fields:**
- username
- password

**Response fields:**
- token 

**Example code:**
```

let data = {
    username: "demo",
    password: "demo"
};
let uri= new URLSearchParams(data);
let url = 'https://sensor.terrabase.com/api/v1/Authenticate/login?'+uri;

let fetchInit = {
	method: 'POST',
  mode: 'cors',
  headers: {
    'Access-Control-Allow-Methods': 'POST',
  },
};

fetch(url, fetchInit
).then(res => {
  return res.json()
}).then(body => {
    console.log(body.token);
});
```

## /api/v1/SensorData
*HTTP POST*

Posts sensor data results to TerraBase.  One or more readings can be wrapped up into a single post operation.  A single post operation can incorporate one or more devices. The various parts are all optional: latlngData, measurementData, sysData and windData.  Authentication is required.

**Example code:**
```
let accessToken = "1234abcd1234abcd1234";

let data=[
  {
    "id": "device001",
    "name": "Front Parking Lot",
    "cat": "air",
    "latlngData": [
      {
        "ts": "2019-04-22T12:20:35Z",
        "lat": 49.0346,
        "lng": -122.784
      }
    ],
    "measurementData": [
      {
        "ts": "2019-04-22T12:20:35Z",
        "param": "BENZENE",
        "result": 4.7,
        "peakTime": 231,
        "peakHeight": 23,
        "unit": "ppb",
        "status": "meas"
      }
    ],
    "sysData": [
      {
        "ts": "2019-04-22T12:20:35Z",
        "microTempC": 24.8,
        "batteryVoltage": 12.1,
        "enclosureTempC": 23.1
      }
    ],
    "windData": [
      {
        "ts": "2019-04-22T12:20:35Z",
        "speed": 4.6,
        "dir": 8.7,
        "unit": "m/s",
        "gust": 5.6
      }
    ]
  }
];

fetch("https://sensor.terrabase.com/api/v1/SensorData", {
  method: "POST",
  mode: 'cors',
  headers: {
    'Access-Control-Allow-Methods': 'POST',
  },
  headers: new Headers({
     'Authorization': 'Bearer ' + accessToken
   }),
}).then(res => {
  return res.json()
}).then(body => {
    console.log(body.message);
});
```

