# TerraRTAndroid

The time is here! We are letting you test out our Real Time Streaming Features!

##Quick Start

A demo app can be found here to get you started quickly!

https://github.com/tryterra/TerraRTAndroid-Demo

## A Worthy Note

This packages uses Bluetooth Low Energy (BLE) and Bluetooth services. Thus only devices that support these can use the library! These will be requested as permissions upon initialisation of the package.

The package streams real time data through **Websockets** and is hosted by us! You can learn more about it here on our [docs](https://docs.tryterra.co/reference/using-the-websocket-api).

The data streamed to your websocket connection (as a developer) will follow the following format:

```json
{
  "op":5,
  "d":
    {
      "ts": <String> (In ISOFormat Date Form)
       "val": <Double>
       "d" <Array<Double>>
    },
    "uid": <String> (user ID)
    "seq": <Int>,
    "t": <String> (Datatype name: Exactly the same as the name of `DataTypes` enum)
}
```

`ts` is the timestamp of the record.

For each datatype, either `val` or `d` is populated.

Datatypes:
- HEART_RATE: `val` the BPM of each reading
- STEPS: `val` the amount of accumulated steps 
- DISTANCE: `val` the accumulated distance in meters
- FLOORS_CLIMBED: `val` the accumulated floors climbed
- STEPS_CADENCE: `val` the amount of steps per second taken by the user
- SPEED: `val` the speed in meter per second of the user
- ACCELERATION: `d` the acceleration data of the device. 
  - d[0] -> acceleration in x direction
  - d[1] -> acceleration in y direction
  - d[2] -> acceleration in z direction
  
- GYROSCOPE: `d` the rotation rate of the device.
  - d[0] -> the rotation rate in x axis
  - d[1] -> the rotation rate in y axis
  - d[2] -> the rotation rate in z axis

## Installation

The library is part of mavenCentral!

You may import it in your app gradle file as: `implementation co.tryterra:terra-rtandroid:0.0.2`

### Extra Steps

For Google Fit, please register your project package name from your `AndroidManifest.xml` file with [Google API](https://console.cloud.google.com). You will need to create a new project and a new set of Oauth credentials within this page. When creating the project, you will also need to consider scopes needed for the project. Enable Fitness API and People API, and select the following scopes:

**People API**
- openid
- /auth/user.birthday.read
- /auth/user.emails.read
- /auth/user.gender.read
- /auth/userinfo.email
- /auth/userinfo.profile

**Fitness API**
- /auth/fitness.activity.read
- /auth/fitness.blood_glucose.read
- /auth/fitness.blood_pressure.read
- /auth/fitness.body.read
- /auth/fitness/heart_rate.read
- /auth/fitness.body_temperature.read
- /auth/fitness.location.read
- /auth/fitness.nutrition.read
- /auth/fitness.oxygen_saturation.read
- /auth/fitness.reproductive_health.read
- /auth/fitness.sleep.read

Finish creating the OAuth Credentials as required and you should be able to start developing with Google Fit!

## Usage

The package revolves mainly around a single class: `TerraRT`. 

You will need to initialise one as follows:

```kotlin
terraRT = TerraRT(
  userId: String = USERID,
  xAPIKey: String = XAPIKEY,
  devId: String = DEVID,
  context: Context = this,
  referenceId: String? = REFERENCE_ID
)
```

**Arguments**:

- `userId: String` => This is the Terra User ID you wish to stream real time from.
- `xAPIKey: String` => The X-API-KEY Terra provides to you when you sign up and pay for our subscription (on our [dashboard](https://dashboard.tryterra.co).
- `devId: String` => The dev-id Terra provides to you wish your subscription
- `context: Context` => The app context for which you call this function from (usually from a class that extends from `Activity` types)
- `referenceId: String` => The reference ID you wish to assign to this user (usually your server's ID for this user)


### Initialising Connections

After initialisation of this class, you can initialise different connections.

You can do this as follows:

```kotlin
terraRT.initConnection(connection: Connections, context: Context, googleFitPermissions: Set<GoogleFitPermissions>)
```

**Arguments**:
- `connection: Connections` => This argument takes a `Connections` enum, indicating the connection you wish to make
- `context: Context` => The app context for which you call this function from (usually from a class that extends from `Activity` types)
- (Optional) `googleFitPermissions: Set<GoogleFitPermissions>` => This argument signifies the permissions you wish to ask for from GoogleFit For streaming their data. It takes a set of an enum of type `GoogleFitPermissions`.

Currently supported Connections:

- BLE (includes Garmin HR Broadcasts, Polar, Wahoo, **XIAOMI Bands** and more!!!)
- Google Fit (SDK Stream)
- Wear OS (Bluetooth) **(Please check out our [WearOS SDK](https://github.com/tryterra/TerraWearOS) for this!)**

Upon running this `initConnection` function, any necessary permission request will be requested.

For Bluetooth/BLE related connections, you will also be required to run 
```kotlin
terraRT.startBluetoothScan(type: Connections, callback: (Boolean) -> Unit)
```

`type: Connections` => The same `Connections` enum as before!

This will cause Bluetooth connection widget will pop up asking you to select the Bluetooth device you wish to connect to. **Connecting to any other devices will simply fail if not supported!**

The callback is to check whether the connection is successful or not. **Please wait for the callback before proceeding!**


### Streaming Data (WOOOO)

Here comes the fun part. You can now simply start streaming data by running

```kotlin
terraRT.startRealtime(type: Connections, dataTypes: DataTypes)
```

**Arguments**

- `connection: Connections` => This argument takes a `Connections` enum, indicating the connection you wish to start realtime for
- `dataTypes: DataTypes` => This argument takes a `DataTypes` enum indicating the datatype you wish to stream for. 

Similarly now, you may stop real time streaming:

```kotlin
terraRT.stopRealtime(type: Connections)
```

**N.B Streaming from WEAR OS is handled on the Watch itself!!**


### Disconnecting

Finally, disconnecting a device is done as:

```kotlin
terraRT.disconnect(type: Connections)
```





  

