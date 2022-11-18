 # Table of Contents
   
- [Overview](#overview)
  - [Requirements](#requirements)
  - [AndroidManifest](#androidmanifest)
- [Android Studio](#android-studio)
  - [How to import the SDK](#how-to-import-the-sdk)
  - [How to use the .aar file](#how-to-use-the-aar-file)
- [Usage](#usage)
- [Additional settings](#additional-settings)
- [Design configuration](#design-configuration)
    - [Colors](#colors)
  

## Overview

This SDK supports AndroidStudio.

### Requirements

- minSdkVersion: 21  (Android 5.0)
- targetSdkVersion:	31 (Android 12.0 Snow Cone)

## Android Studio

### How to import the SDK

In your top-level build.gradle project file add the following url under repositories block:

```
allprojects {
repositories {
..
        maven {
            url "https://raw.githubusercontent.com/idnow/de.idnow.android/sdk_5.2.x"
        }
        maven {
            url "https://raw.githubusercontent.com/idnow/de.idnow.android.sdk/master/"
        }
        maven {
            url = uri("https://repo.authada.de/public/")
            authentication {
                basic(BasicAuthentication)
            }
            // Authada Credentials needs to be requested
            credentials {
                username "*******"
                password "*******"
            }
        }
        // Docker Credentials needs to be requested
        maven {
            url "https://docker.dev.idnow.de/artifactory/de.idnow.android"
            credentials {
                username = "*******"
                password = "*******"
            }
        }
..
}
}
```

and in the dependencies part of your app.gradle add:   

```
dependencies {
..
implementation 'de.idnow.platform_v1.android:idnow-android-platform-sdk-v1:1.0.0'
implementation 'de.idnow.sdk:idnow-android-sdk:5.2.0.2'
implementation 'de.idnow.android.eid:idnow-android-eid-sdk:2.5.1.1'
..
}
```

### How to use the .aar file:

Copy the idnow-android-platform-1.0.0.aar into the apps libs folder.

In your app.gradle add:

```
repositories {
	
maven {
  url "https://raw.githubusercontent.com/idnow/de.idnow.android/master"
       }
	
flatDir {
dirs 'libs' //this way we can find the .aar file in libs folder
}
}
```

Additional dependencies to add in your app.gradle:

``` 
dependencies {
    implementation 'de.idnow.platform_v1.android:idnow-android-platform-1.0.0@aar'
    // all dependencies used in each subproduct
    }
```


## Usage

The ONLY class in the SDK designated for user access is the UnifiedSDK class.

To create an instance of the UnifiedSDK, perform the following call and provide the companyid, which was provided to you during setup as well as an activity that receives callbacks. UnifiedSDK is a singleton class, so whenever you need to call it just do so with UnifiedSDK.getInstance().

```
UnifiedSDK.getInstance().initialize(<Activity>, "<companyid>");
```

Set the static parameters for the SDK usage. Context has to be passed, as parameters are persisted in Preferences.

To actually start the identification pass your transaction token.

```
UnifiedSDK.getInstance().start(<Your transaction token>);
```

Here is the full example:


```java
try {
	// Initialize with your activity which will handle the SDK callback and pass the id of your company.
	//	UnifiedSDK is a singleton class, so just call it with UnifiedSDK.getInstance()
	UnifiedSDK.getInstance().initialize(StartActivity.this, “demounifiedident);

	// Set the transactionToken, for example from a TextField
	UnifiedSDK.setTransactionToken(editTextToken.getText().toString(), context);

	// To actually start the identification process, pass the transactionToken.
	UnifiedSDK.getInstance().start(UnifiedSDK.getTransactionToken(context));
	
	
} catch (Exception e) {
	// The SDK checks the input parameters and throws an exception if they don't seem right.
	e.printStackTrace();
}
```


## Additional settings

You can set the new branding (Circular background for the buttons)

```
IDnowSDK.setNewBrand(TRUE);
	
```

## Design configuration

### Colors & Text

The SDK is designed with colors and texts following the IDnow corporate design. You can use the SDK without making any adaptions to the colors at all. If, however you want the SDK screens to appear in different colors and different texts could be changed via customer config.




