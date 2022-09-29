# ManiTrust
GitHub of ManiTrust Service.

## Installation
> This library is project published with JitPack
First, add authToken that we provide to get the library with jitpack.io.
In gradle.properties include this line:
```
authToken=AUTH_TOKEN
```
Next, include JitPack in the repositories list:
  ```groovy
  buildscript {
    repositories {
        maven {
          url 'https://jitpack.io'
          credentials { userName authToken }
        }
    }
  }
  ```
Now you can add the dependency:
```groovy
dependencies {
  implementation 'com.github.KevychSolutions:manitrust-android:Tag'
}
```
## Setup project
### Keys and tokens
First of all we have to exchange with keys.
We provide:
- JitPack.io authentication token
- Server api token
- Project id

You provide:
- Firebase WEB API Key

### Setup Firebase
  1) Add the firebase to the project [FirebaseSetup](https://firebase.google.com/docs/android/setup)  <br />
  2) Generate an Web API Key and provide to the admin  <br />
  3) Create firebase messging service [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  4) Generate FCM Token [GenerateFCMToken](https://firebase.google.com/docs/cloud-messaging/android/client#retrieve-the-current-registration-token)  <br />

## Usage
### Kotlin
  1) Create the RegistrationToken class using server API key class. Provide Token and phone number to the server via this class:
  ```kotlin
val registrationToken = RegistrationTokenBuilder().build(apiKey, projectId, hostUrl)
registrationToken.sendTokenAndPhone(fcmToken, phoneNumber)
  ```
  2) Create firebase messaging service to receive notifications and then pass them to our SDK:
  [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  3) In firebase messaging class call our sdk like this:
  ```kotlin
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        super.onMessageReceived(remoteMessage)
        val msgHandler = MessageHandlerBuilder().build(applicationContext)
        msgHandler.handleMessage(remoteMessage.data)
        ...
    }
  ```
  Or better use suspend methods:
  ```kotlin
    val registrationToken = RegistrationTokenBuilder().build(apiKey, projectId, hostUrl)
    CoroutineScope(Dispatchers.IO).launch{
        registrationToken.sendTokenAndPhoneSuspend(fcmToken, phoneNumber)
    }
  ```
  ```kotlin
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        super.onMessageReceived(remoteMessage)
        val messageHandler: MessageHandler = MessageHandlerBuilder().build(applicationContext)
        CoroutineScope(Dispatchers.IO).launch {
            val handledMessage = messageHandler.handleMessageSuspend(remoteMessage.data)
            if (handledMessage.isOK){
                ...
            } else {
                ...
            }
        }
    }
  ```

### Java
  1) Create the RegistrationToken class using server API key class. Provide Token and phone number to the server via this class:
  ```java
    RegistrationToken registrationToken = new RegistrationTokenBuilder()
             .build(apiKey, projectId, hostUrl);
    registrationToken.sendTokenAndPhone(fcmToken, phoneNum);        
  ```
  2) Create firebase messaging service to receive notifications and then pass them to our SDK:
  [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  3) In firebase messaging class call our sdk like this:
  ```java
    @Override
    public void onMessageReceived(@NonNull RemoteMessage remoteMessage) {
        super.onMessageReceived(remoteMessage);
        MessageHandler msgHandler = new MessageHandlerBuilder()
                .build(getApplicationContext());
        msgHandler.handleMessage(remoteMessage.getData());
      ...
    }
  ```
  
### Warning! 

  In Java project you should call methods: sendTokenAndPhone() and handleMessage() in another thread for main thread safety!
  
## Addinitonal info

### Release tags

[Release](https://jitpack.io/#KevychSolutions/manitrust-android/0.0.1) - jitpack.io

To create the pre-release version of sdk that can be distributed, just push to the master. The github will create the release by itself. Then jitpack will use this release to publish it as privatly-distributed SDK. To change the version of the tag, go to /github/workflows/publish_version.yml and change automatic_release_tag attribute. To finally create release version (currently it's marked as pre-release) change the prerelease attribute to "false".

<br />
<br />

&nbsp;&nbsp;&nbsp;[<Back](index)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Next>](ios) (IOS)
