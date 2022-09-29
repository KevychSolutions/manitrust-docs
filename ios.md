# ManiTrust
GitHub of ManiTrust Service.

## Installation

##### CocoaPods 

[CocoaPods](https://cocoapods.org/) is a dependency manager for Cocoa projects. For usage and installation instructions, visit their website. To integrate ManiTrust into your Xcode project using CocoaPods, specify it in your `Podfile`:

- Add below in podfile - in respective target block

```swift
pod 'ManiTrust'
```

- Execute below command in terminal

```swift
pod install
```

##### Swift Package Manager - SPM

The [Swift Package Manager](https://swift.org/package-manager/) is a tool for automating the distribution of Swift code and is integrated into the `swift`compiler.

Once you have your Swift package set up, adding ManiTrust_SDK as a dependency is as easy as adding it to the `dependencies` value of your `Package.swift`.

```swift
dependencies: [
    .package(url: "https://github.com/KevychSolutions/manitrust-SDK.git")
]
```

## Integration SDK

SDK supports Firebase Messaging and APNS. If you set up on Dashboard Firebase please make sure you are also passing inside SDK Firebase token and otherwise APNS token.

1. Import SDK in file in which you are going to use it: `import ManiTrust_SDK`
2. Set up SDK by using method `configureSDK`: 

```swift
    ManiTrustBuilder.shared.configureSDK(url: <Server_URL>,
                                         projectID: <Project_ID>,
                                         apiKey: <APIKEY>,
                                         automaticContactPermission: Bool,
                                         automaticPushPermission: Bool)
```

3. In order to register devices you need to use the method `registeredDevice`:

```swift
    ManiTrustBuilder.shared.registeredDevice(tokenDevice: <TOKEN_DEVICE>,
                                             phoneNumber: <PHONE_NUMBER>)
```

4. You need to integrate `Notification Service Extension` to the project and in method `didReceive` from `UNNotificationServiceExtension` you need add:

 ```swift
 ManiTrustBuilder.shared.setupContact(contactData: <[String: Any]>)
 ```
where parameters `contactData` it's a dictionary with data for adding/deleting contact in the contacts list.

For detailed implementation please check the demo app version.

## Release Framework

The framework is released automatically when the code is pushed into the main branch.

<hr />
<br />

[<Back](index) (SDKs)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(Android) [Next>](android)
