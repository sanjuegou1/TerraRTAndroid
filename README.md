# TerraRTAndroid

The time is here! We are letting you test out our Real Time Streaming Features!

## A Worthy Note

This packages uses Bluetooth Low Energy (BLE) and Bluetooth services. Thus only devices that support these can use the library! These will be requested as permissions upon initialisation of the package.

The package streams real time data through **Websockets** and is hosted by us! You can learn more about it here on our [docs](https://docs.tryterra.co/reference/using-the-websocket-api).

## Installation

Download the `.aar` file from this repository and include it within the `app/libs` folder in your project structure. You may now add it as a dependency in your gradle configuration files. In your project level gradle file (`build.gradle(:Project)`), edit the `repositories` to include:

```gradle
flatDir{
  dirs 'libs'
}
```

For example:
```gradle
repositories {
    google()
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
}
```

Then you must include the library and the coroutines library in your app level gradle file(`build.gradle(:app)`) by adding the following lines under `dependencies`:

```gradle
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.5.2'
implementation files('libs/TerraRTAndroid-alpha.aar')

```

For example:
```gradle
dependencies {
    implementation 'androidx.core:core-ktx:1.7.0'
    implementation 'androidx.appcompat:appcompat:1.4.0'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.5.2'
    implementation files('libs/TerraRTAndroid-alpha.aar')
}
```

You may now import classes from the library as: `import co.tryterra.terrartandroid.(Every class in this library imaginable (except private ones))`

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

- Polar Straps (BLE)
- Wahoo Straps (BLE)
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


##$ Streaming Data (WOOOO)

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


##$ Disconnecting

Finally, disconnecting a device is done as:

```kotlin
terraRT.disconnect(type: Connections)
```





  

