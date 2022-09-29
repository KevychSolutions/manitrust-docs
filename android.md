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
  1. Add the firebase to the project [FirebaseSetup](https://firebase.google.com/docs/android/setup)  <br />
  2. Generate an Web API Key and provide to the admin  <br />
  3. Create firebase messging service [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  4. Generate FCM Token [GenerateFCMToken](https://firebase.google.com/docs/cloud-messaging/android/client#retrieve-the-current-registration-token)  <br />


## Permissions
To use our SDK you must grant the following permission:
- `"android.permission.INTERNET"` - to have access to Internet <br />
- `"android.permission.READ_CONTACTS"` - to be able read contacts <br />
- `"android.permission.WRITE_CONTACTS"` - to be able create new contacts <br />


## Usage

In this SDK you will work with the next classes: <br />
- **RegistrationToken**. This class is responsible for sending user's device token and phone number to the server. <br /> To get instance of this class you should use `RegistrationTokenBuilder` class, which builds `RegistrationToken`. <br />
- **MessageHandler**. This class is responsible for handling the message we receive from Firebase Messaging Service.
 You have to write your own Firebase Service, that can receive the push notification and
 then pass the message from the service to this class.  <br /> To get instance of this class you should use `MessageHandlerBuilder` class, which builds `MessageHandler`.
- **Result**(Optional). You can also handle result from any operation you will do with classes above.
 
 Here is descriprion how to use ManiTrust SDK on Kotlin and Java programming languages: 

### Kotlin
  1. Create the `RegistrationToken` class using server API key class. Provide Token and phone number to the server via this class:
  ```kotlin
  val registrationToken: RegistrationToken = RegistrationTokenBuilder()
               .build(apiKey, projectId, hostUrl)
  val sendTokenAndPhoneResult: Result = registrationToken
               .sendTokenAndPhone(fcmToken, phoneNumber)
  ```
  2. Create firebase messaging service to receive notifications and then pass them to our SDK:
  [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  3. In firebase messaging class call our sdk like this:
  ```kotlin
  override fun onMessageReceived(remoteMessage: RemoteMessage) {
           super.onMessageReceived(remoteMessage)
           val messageHandler: MessageHandler = MessageHandlerBuilder()
                   .build(applicationContext)
           val handleMessageResult: Result = messageHandler
                   .handleMessage(remoteMessage.data)
           ...
  }
  ```
  Or better use suspend methods:
  ```kotlin
  val registrationToken: RegistrationToken = RegistrationTokenBuilder()
                .build(apiKey, projectId, hostUrl)
  CoroutineScope(Dispatchers.IO).launch {
           val sendTokenAndPhoneResult: Result = registrationToken
                    .sendTokenAndPhoneSuspend(fcmToken, phoneNumber)
  }
  ```
  ```kotlin
  override fun onMessageReceived(remoteMessage: RemoteMessage) {
           super.onMessageReceived(remoteMessage)
           val messageHandler: MessageHandler = MessageHandlerBuilder()
                   .build(applicationContext)
           CoroutineScope(Dispatchers.IO).launch {
               val handledMessageResult: Result = messageHandler
                       .handleMessageSuspend(remoteMessage.data)
               if (handledMessageResult.isOK){
                   ...
               } else {
                   ...
               }
           }
  }
  ```

### Java
  1. Create the `RegistrationToken` class using server API key class. Provide Token and phone number to the server via this class:
  ```java
  RegistrationToken registrationToken = new RegistrationTokenBuilder()
               .build(apiKey, projectId, hostUrl);
  Result sendTokenAndPhoneResult = registrationToken
               .sendTokenAndPhone(fcmToken, phoneNum);        
  ```
  2. Create firebase messaging service to receive notifications and then pass them to our SDK:
  [CreateFirebaseMessagingService](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessagingService)  <br />
  3. In firebase messaging class call our sdk like this:
  ```java
  @Override
  public void onMessageReceived(@NonNull RemoteMessage remoteMessage) {
           super.onMessageReceived(remoteMessage);
           MessageHandler messageHandler = new MessageHandlerBuilder()
                   .build(getApplicationContext());
           Result handleMessageResult = messageHandler
                   .handleMessage(remoteMessage.getData());
  }
  ```
  
### Pay attention! 

  In Java project you should execute methods: `sendTokenAndPhone()` and `handleMessage()` in new thread for main thread safety!
  
## More Documentation
See in GitHub - [Pages](https://github.com/KevychSolutions/manitrust-docs/)
<hr />
<br />

[<Back](index) (SDKs)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(IOS) [Next>](ios)
