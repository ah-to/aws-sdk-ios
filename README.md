# AWS SDK for iOS

[![Release](https://img.shields.io/github/release/aws/aws-sdk-ios.svg)](../../releases)
[![Code Coverage](https://codecov.io/gh/aws-amplify/aws-sdk-ios/branch/main/graph/badge.svg)](https://codecov.io/gh/aws-amplify/aws-sdk-ios)
[![CocoaPods](https://img.shields.io/cocoapods/v/AWSCore.svg)](https://cocoapods.org/pods/AWSCore)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Integration Test](https://github.com/aws-amplify/aws-sdk-ios/actions/workflows/integ-test.yml/badge.svg)](https://github.com/aws-amplify/aws-sdk-ios)
[![Unit Test](https://github.com/aws-amplify/aws-sdk-ios/actions/workflows/unit-test.yml/badge.svg)](https://github.com/aws-amplify/aws-sdk-ios)
[![Discord](https://img.shields.io/discord/308323056592486420?logo=discord)](https://discord.gg/jWVbPfC) 

The AWS SDK for iOS provides a library and documentation for developers to build connected mobile applications using AWS.

We recommend using the latest v2 version of AWS Amplify Library for Swift to quickly implement common app use cases like Authentication, Storage, Push Notifications and more that follow patterns idiomatic to Swift like async/await. 

Note: v2 of Amplify Library for Swift (currently GA) is built on top of the [AWS SDK for Swift](https://github.com/awslabs/aws-sdk-swift) and _only_ provides access to the AWS SDK for Swift which is currently in developer preview. You can access this underlying SDK via the Escape Hatch from AWS Amplify. 

You can head to [the Amplify Library for Swift documentation](https://docs.amplify.aws/lib/auth/getting-started/q/platform/ios/) to learn more about all the features. You can also use AWS Amplify with [your existing AWS cloud resources](https://docs.amplify.aws/lib/project-setup/use-existing-resources/q/platform/ios/). If you are unable to find features you are looking for in Amplify please open an issue in [the Amplify Library for Swift GitHub repo](https://github.com/aws-amplify/amplify-swift/issues/new/choose) and we will be happy to consider you request.

If you still wish to use the AWS SDK for iOS directly you can refer to [the AWS SDK Documentation here](https://docs.amplify.aws/sdk/q/platform/ios/) and follow the setup instructions below. You can also look at sample apps in [the AWS SDK for iOS Samples repo](https://github.com/awslabs/aws-sdk-ios-samples).

## Setup

To use the AWS SDK for iOS, you will need the following installed on your development machine:

* Xcode 11.0 or later
* iOS 12 or later

## Include the SDK for iOS in an Existing Application

We have a couple [samples](https://github.com/awslabs/aws-sdk-ios-samples) applications which showcase how to use the AWS SDK for iOS.  Please note that the code in these sample applications is not of production quality, and should be considered as exactly what we called them: samples.

There are several ways to integrate the AWS Mobile SDK for iOS into your own project:

* [Swift Package Manager](https://swift.org/package-manager/)
* [CocoaPods](https://cocoapods.org/)
* [Carthage](https://github.com/Carthage/Carthage)
* [Dynamic Frameworks](https://aws.amazon.com/mobile/sdk/)

You should use ONE and only one of these ways to import the AWS Mobile SDK. Importing the SDK in multiple ways loads duplicate copies of the SDK into the project and causes compiler/linker errors.

> Note: If you are using XCFrameworks (i.e., either Swift Package Manager, Carthage, or Dynamic Frameworks), some modules are named with the `XCF` suffix to work around a [Swift issue](https://bugs.swift.org/browse/SR-11704). `AWSMobileClient` is named as `AWSMobileClientXCF` and `AWSLocation` is named as `AWSLocationXCF`. To use the `AWSMobileClient` or `AWSLocation` SDKs, import them as:

```swift
import AWSMobileClientXCF
import AWSLocationXCF
```

and use it your app code without the `XCF` suffix.

```swift
AWSMobileClient.default().initialize() 
let locationClient = AWSLocation.default()
```

### Swift Package Manager

1. Swift Package Manager is distributed with Xcode. To start adding the AWS SDK to your iOS project, open your project in Xcode and select **File > Swift Packages > Add Package Dependency**.

    ![Add package dependency](readme-images/spm-setup-01-add-package-dependency.png)

1. Enter the URL for the AWS SDK for iOS Swift Package Manager GitHub repo (`https://github.com/aws-amplify/aws-sdk-ios-spm`) into the search bar and click **Next**.

    ![Search for repo](readme-images/spm-setup-02-search-amplify-repo.png)

    **NOTE:** This URL is _not_ the main URL of the SDK. We maintain the Swift Package Manager manifest (`Package.swift`) file for this library in a separate repo so that apps that use the SDK do not have to download the entire source repository in order to consume the binary targets.

1. You'll see the repository rules for which version of the SDK you want Swift Package Manager to install. Choose the first rule, **Version**, and select **Up to Next Minor** as it will use the latest compatible version of the dependency that can be detected from the `main` branch, then click **Next**.

    ![Dependency version options](readme-images/spm-setup-03-dependency-version-options.png)

    **NOTE:** The AWS Mobile SDK for iOS does [not use Semantic Versioning](https://docs.amplify.aws/sdk/configuration/setup-options/q/platform/ios#aws-sdk-version-vs-semantic-versioning), and may introduce breaking API changes on minor version releases. We recommend setting your **Version** rule to **Up to Next Minor** and evaluating minor version releases to ensure they are compatible with your app.

1. Choose which of the libraries you want added to your project. Always select the **AWSCore** SDK. The remaining SDKs to install will vary based on which SDK you're trying to install. Most SDKs rely only on **AWSCore**, but for a full dependency list, see the [README-spm-support file](README-spm-support.md).

    _Note: AWSLex is not currently supported for the `arm64` architecture through Swift Package Manager due to conflicts with a packaged binary dependency._

    ![Select dependencies](readme-images/spm-setup-04-select-dependencies.png)

    Select all that are appropriate, then click **Finish**.

    You can always go back and modify which SPM packages are included in your project by opening the Swift Packages tab for your project: Click on the Project file in the Xcode navigator, then click on your project's icon, then select the **Swift Packages** tab.

### CocoaPods

1. The AWS Mobile SDK for iOS is available through [CocoaPods](http://cocoapods.org). If you have not installed CocoaPods, install CocoaPods by running the command:

        $ gem install cocoapods
        $ pod setup

    Depending on your system settings, you may have to use `sudo` for installing `cocoapods` as follows:

        $ sudo gem install cocoapods
        $ pod setup

2. In your project directory (the directory where your `*.xcodeproj` file is), run the following to create a `Podfile` in your project.

        $ pod init

3. Edit the podfile to include the pods you want to integrate into your project.  For example, if you need auth, you can use AWSMobileClient, and if you need analytics, you add AWSPinpoint.  As a result, your podfile might look something like this:
```
target 'YourTarget' do
    pod 'AWSMobileClient'
    pod 'AWSPinpoint'
end
```

For a complete list of our pods, check out the .podspec files in the root directory of this project.

3. Then run the following command:
    
        $ pod install --repo-update

4. To open your project, open the newly generated `*.xcworkspace` file in your project's directory with XCode.  You can do this by issuing the following command in your project folder:

        $ xed .

    **Note**: Do **NOT** use `*.xcodeproj`. If you open up a project file instead of a workspace, you may receive the following error:

        ld: library not found for -lPods-AWSCore
        clang: error: linker command failed with exit code 1 (use -v to see invocation)

### Carthage

#### XCFrameworks (recommended)

Carthage supports XCFrameworks in Xcode 12 or above. Follow the steps below to consume the AWS SDK for iOS using XCFrameworks:

1. Install Carthage 0.37.0 or greater.

2. Add the following to your `Cartfile`:

        github "aws-amplify/aws-sdk-ios"

3. Then run the following command:
    
        $ carthage update --use-xcframeworks --no-use-binaries

> As of Carthage 0.37.0, prebuilt binaries using XCFrameworks are not supported, as mentioned in the Carthage release notes - https://github.com/Carthage/Carthage/releases/tag/0.37.0

4. On your application targets’ General settings tab, in the Embedded Binaries section, drag and drop each xcframework you want to use from the Carthage/Build folder on disk.

#### Frameworks with "fat libraries" (not recommended)

To build platform-specific framework bundles with multiple architectures in the binary, (Xcode 11 and below)

1. Install the latest version of [Carthage](https://github.com/Carthage/Carthage#installing-carthage).

2. Add the following to your `Cartfile`:

        github "aws-amplify/aws-sdk-ios"

3. Then run the following command:
    
        $ carthage update

4. With your project open in Xcode, select your **Target**. Under **General** tab, find **Frameworks, Libraries, and Embedded Content** and then click the **+** button.

5. Click the **Add Other...** button, then "Add Files..." in the popup menu, then navigate to the `AWS<#ServiceName#>.framework` files under `Carthage` > `Build` > `iOS` and select them. Do not check the **Destination: Copy items if needed** checkbox if prompted.  Add the frameworks that you need for you specific use case.  For example, if you are using AWSMobileClient and AWSPinpoint, you will want to add the following frameworks:

    * `AWSAuthCore.framework`
    * `AWSCognitoIdentityProvider.framework`
    * `AWSCognitoIdentityProviderASF.framework`
    * `AWSCore.framework`
    * `AWSMobileClient.framework`
    * `AWSPinpoint.framework`

6. Under the **Build Phases** tab in your **Target**, click the **+** button on the top left and then select **New Run Script Phase**. Then setup the build phase as follows. Make sure this phase is below the `Embed Frameworks` phase.

        Shell /bin/sh
        
        bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/AWSCore.framework/strip-frameworks.sh"
        
        Show environment variables in build log: Checked
        Run script only when installing: Not checked
        
        Input Files: Empty
        Output Files: Empty

> Note: Currently, the AWS SDK for iOS builds the Carthage binaries using the latest released version of Xcode. To consume the pre-built binaries your Xcode version needs to be the same, else you have to build the frameworks on your machine by passing `--no-use-binaries` flag to `carthage update` command.

### Frameworks

#### XCFramework setup

Starting AWS SDK iOS version 2.22.1, SDK binaries are released as XCFrameworks. Follow the steps below to install XCFramework.

1. Download the [latest SDK](https://releases.amplify.aws/aws-sdk-ios/latest/aws-ios-sdk.zip). Older SDK versions can be downloaded from `https://releases.amplify.aws/aws-sdk-ios/aws-ios-sdk-#.#.#.zip`, where `#.#.#` represents the version number. So for version 2.23.3, the download link is [https://releases.amplify.aws/aws-sdk-ios/aws-ios-sdk-2.23.3.zip](https://releases.amplify.aws/aws-sdk-ios/aws-ios-sdk-2.23.3.zip).
> Note1: If you are using version < 2.22.1 please refer to the "Legacy framework setup" section below.
> Note2: To download version < 2.23.3 use this link `https://sdk-for-ios.amazonwebservices.com/aws-ios-sdk-#.#.#.zip`

2. Uncompress the ZIP file
3. On your application targets’ General settings tab, in the Embedded Binaries section, drag and drop each xcframework you want to use from the downloaded folder.

#### Legacy framework setup

1. Download the required SDK using `https://sdk-for-ios.amazonwebservices.com/aws-ios-sdk-#.#.#.zip`, where `#.#.#` represents the version number. So for version 2.10.2, the download link is [https://sdk-for-ios.amazonwebservices.com/aws-ios-sdk-2.10.2.zip](https://sdk-for-ios.amazonwebservices.com/aws-ios-sdk-2.10.2.zip).
> Note: If you are using version > 2.22.0 please refer to the "XCFramework setup" section above. 

2. With your project open in Xcode, select your **Target**. Under **General** tab, find **Embedded Binaries** and then click the **+** button.

3. Click the **Add Other...** button, navigate to the `AWS<#ServiceName#>.framework` files and select them. Check the **Destination: Copy items if needed** checkbox when prompted.  Add the frameworks that you need for you specific use case.  For example, if you are using AWSMobileClient and AWSPinpoint, you will want to add the following frameworks:

    * `AWSAuthCore.framework`
    * `AWSCognitoIdentityProvider.framework`
    * `AWSCognitoIdentityProviderASF.framework`
    * `AWSCore.framework`
    * `AWSMobileClient.framework`
    * `AWSPinpoint.framework`

4. Under the **Build Phases** tab in your **Target**, click the **+** button on the top left and then select **New Run Script Phase**. Then setup the build phase as follows. Make sure this phase is below the `Embed Frameworks` phase.

        Shell /bin/sh
        
        bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/AWSCore.framework/strip-frameworks.sh"
        
        Show environment variables in build log: Checked
        Run script only when installing: Not checked
        
        Input Files: Empty
        Output Files: Empty

## Update the SDK to a Newer Version

When we release a new version of the SDK, you can pick up the changes as described below.

### CocoaPods

1. Run the following command in your project directory. CocoaPods automatically picks up the new changes.

        $ pod update

    **Note**: If your pod is having an issue, you can delete `Podfile.lock` and `Pods/` then run `pod install` to cleanly install the SDK.
    
    ![image](readme-images/cocoapods-setup-03.png?raw=true)

### Carthage

1. Run the following command in your project directory. Carthage automatically picks up the new changes.

        $ carthage update

### Frameworks

1. In Xcode's **Project Navigator**, type "AWS" to find the AWS Frameworks or XCFrameworks that you manually added to your project. Select all of the AWS Frameworks and hit **Delete** on your keyboard. Then select **Move to Trash**. 

2. Follow the installation process above to include the new version of the SDK.

## Getting Started with Swift

1. Import the AWSCore header in the application delegate.

    ```swift
    import AWSCore
    ```

2. Create a default service configuration by adding the following code snippet in the `application:didFinishLaunchingWithOptions:` application delegate method.

    ```swift
    let credentialsProvider = AWSCognitoCredentialsProvider(
        regionType: CognitoRegionType,
        identityPoolId: CognitoIdentityPoolId)
    let configuration = AWSServiceConfiguration(
        region: DefaultServiceRegionType,
        credentialsProvider: credentialsProvider)
    AWSServiceManager.default().defaultServiceConfiguration = configuration
    ```

3. In Swift file you want to use the SDK, import the appropriate headers for the services you are using. The header file import convention is `import AWSServiceName`, as in the following examples:

    ```swift
    import AWSS3
    import AWSDynamoDB
    import AWSSQS
    import AWSSNS
    ```
        
4. Make a call to the AWS services.

    ```swift
    let dynamoDB = AWSDynamoDB.default()
    let listTableInput = AWSDynamoDBListTablesInput()
    dynamoDB.listTables(listTableInput!).continueWith { (task:AWSTask<AWSDynamoDBListTablesOutput>) -> Any? in
        if let error = task.error as? NSError {
        print("Error occurred: \(error)")
            return nil
        }
    
        let listTablesOutput = task.result
    
        for tableName in listTablesOutput!.tableNames! {
            print("\(tableName)")
        }
    
        return nil
    }
    ```
        
**Note**: Most of the service client classes have a singleton method to get a default client. The naming convention is `+ defaultSERVICENAME` (e.g. `+ defaultDynamoDB` in the above code snippet). This singleton method creates a service client with `defaultServiceConfiguration`, which you set up in step 5, and maintains a strong reference to the client.

## Getting Started with Objective-C

1. Import the AWSCore header in the application delegate.
        
    ```objective-c
    @import AWSCore;
    ```

2. Create a default service configuration by adding the following code snippet in the `application:didFinishLaunchingWithOptions:` application delegate method.

    ```objective-c
    AWSCognitoCredentialsProvider *credentialsProvider = [[AWSCognitoCredentialsProvider alloc] initWithRegionType:CognitoRegionType
                                                                                                    identityPoolId:CognitoIdentityPoolId];
    AWSServiceConfiguration *configuration = [[AWSServiceConfiguration alloc] initWithRegion:DefaultServiceRegionType
                                                                         credentialsProvider:credentialsProvider];
    AWSServiceManager.defaultServiceManager.defaultServiceConfiguration = configuration;
    ```

3. Import the appropriate headers for the services you are using. The header file import convention is `@import AWSServiceName;`, as in the following examples:

    ```objective-c
    @import AWSS3;
    @import AWSDynamoDB;
    @import AWSSQS;
    @import AWSSNS;
    ```

4. Make a call to the AWS services.

    ```objective-c
    AWSSNS *sns = [AWSSNS defaultSNS];
    AWSSNSListTopicsInput *listTopicsInput = [AWSSNSListTopicsInput new];
    [[sns listTopics:listTopicsInput] continueWithBlock:^id(AWSTask *task) {
        // Do something with the response
        return nil;
    }];
    ```

**Note**: Most of the service client classes have a singleton method to get a default client. The naming convention is `+ defaultSERVICENAME` (e.g. `+ defaultS3SNS` in the above code snippet). This singleton method creates a service client with `defaultServiceConfiguration`, which you set up in step 5, and maintains a strong reference to the client.

## Working with AWSTask

The SDK returns `AWSTask` objects when operating on asynchronous operations to avoid blocking the UI thread.

The AWSTask class is a renamed version of BFTask from the Bolts framework. For complete documentation on Bolts, see the [Bolts-iOS repo](https://github.com/BoltsFramework/Bolts-ObjC)

## Logging

As of version 2.5.4 of this SDK, logging utilizes [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack), a flexible, fast, open source logging framework. It supports many capabilities including the ability to set logging level per output target, for instance, concise messages logged to the console and verbose messages to a log file.

CocoaLumberjack logging levels are additive such that when the level is set to verbose, all messages from the levels below verbose are logged. It is also possible to set custom logging to meet your needs. For more information, see [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack/blob/master/Documentation/CustomLogLevels.md)

### Changing Log Levels

**Swift**

```swift
AWSDDLog.sharedInstance.logLevel = .verbose
```

The following logging level options are available:

* `.off`
* `.error`
* `.warning`
* `.info`
* `.debug`
* `.verbose`

**Objective-C**

```objective-c
[AWSDDLog sharedInstance].logLevel = AWSDDLogLevelVerbose;
```

The following logging level options are available:

* `AWSDDLogLevelOff`
* `AWSDDLogLevelError`
* `AWSDDLogLevelWarning`
* `AWSDDLogLevelInfo`
* `AWSDDLogLevelDebug`
* `AWSDDLogLevelVerbose`

We recommend setting the log level to `Off` before publishing to the Apple App Store.

### Targeting Log Output

CocoaLumberjack can direct logs to file or used as a framework that integrates with the Xcode console.

To initialize logging to files, use the following code:

**Swift**

```swift
let fileLogger: AWSDDFileLogger = AWSDDFileLogger() // File Logger
fileLogger.rollingFrequency = TimeInterval(60*60*24)  // 24 hours
fileLogger.logFileManager.maximumNumberOfLogFiles = 7
AWSDDLog.add(fileLogger)
```

**Objective-C**

```objective-c
AWSDDFileLogger *fileLogger = [[AWSDDFileLogger alloc] init]; // File Logger
fileLogger.rollingFrequency = 60 * 60 * 24; // 24 hour rolling
fileLogger.logFileManager.maximumNumberOfLogFiles = 7;
[AWSDDLog addLogger:fileLogger];
```

To initialize logging to your Xcode console, use the following code:

**Swift**

```swift
AWSDDLog.add(AWSDDOSLogger.sharedInstance) // Apple's unified logging
```

**Objective-C**

```objective-c
[AWSDDLog addLogger:[AWSDDOSLogger sharedInstance]]; // Apple's unified logging
```

## Open Source Contributions

We welcome any and all contributions from the community! Make sure you read through our contribution guide [here](./CONTRIBUTING.md) before submitting any PR's. Thanks! <3

## Talk to Us

Visit our GitHub [Issues](https://github.com/aws-amplify/aws-sdk-ios/issues) to leave feedback and to connect with other users of the SDK.

## Author

Amazon Web Services

## License

See the **LICENSE** file for more info.

