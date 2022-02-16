<p align="center">
  <img src="images/pg-logo.png"/>
</p>

# Pubguard Android Framework

A guide to installing Pubguard on your Android Google Play Store application

The Pubguard framework is a solution that monitors the advertising content flowing through your mobile app, protecting user against unwanted ad content.

The data from the framework is then accessible via your account on the Pubguard dashboard where you can set up preferences, alerts and browse the gallery.

---

# Table of contents

- [Getting Started](#getting-started)
- [Requirements](#requirements)
- [Integrating](#integrating)
- [Change Log](#changelog)
- [SDK support](#support)
- [Versioning](#versioning)
- [License](#license)

---

# Getting Started

The following instructions will enable you to get the Pubguard framework running on your Android app.  
- iOS instructions can be found [here](https://github.com/bidstack-group/pubguard-sdk-ios)
- Unity instructions can be found [here](https://github.com/bidstack-group/pubguard-unity-plugin)

## Prerequisites

Before installing the Pubguard framework you will need an API key which is available through your Pubguard account. To retrieve an API key, please access http://dashboard.pubguard.com or email support@pubguard.com. The key is used in both the iOS and Android installations. Please note that separate keys are used for Android Google Play and iOS installations.
If you do not have a Pubguard account, please sign up on https://pubguard.com/

## Requirements

- Android Studio 3.3 and up
- Android platform 4.4 KitKat (API 19) and up
- Android Gradle Plugin 3.3.0 and up
- Gradle Wrapper 5.0 and up
- (if Kotlin is used) Kotlin Gradle Plugin 1.3.50 and up

---

# Integrating

### Installing

1. Add `classpath` and make sure that you have `mavenCentral()` included in project level `build.gradle`:

    ```groovy
    buildscript {
        repositories {
            mavenCentral()
        }
        dependencies {
            classpath 'com.pubguard:pubguard-gradle-plugin:1.3.0'
        }
    }

    allprojects {
        repositories {
            mavenCentral()
        }
    }
    ```
  
2. Add dependency for the Pubguard to your module's `build.gradle`:

    ```groovy
    plugins {
        ...
        id 'com.pubguard.pubguard-gradle-plugin'
    }
    
    dependencies {
        implemetation 'com.pubguard:pubguard:1.3.0'
    }
    ```
#### Installing locally
 
<details>
  <summary>Click to expand</summary> 
  
  1. Download pubguard-1.3.0.aar and pubguard-gradle-plugin-1.3.0.jar from [releases](https://github.com/bidstack-group/pubguard-sdk-android-dev/releases)
  2. Copy pubguard-1.3.0.aar and pubguard-gradle-plugin-1.3.0.jar into your main app module libs folder
  3. Into project's `build.gradle` add:

        ```groovy
        buildscript {
            repositories {
                google()
                mavenCentral()
            }
            dependencies {
                classpath files('app/libs/pubguard-gradle-plugin-1.3.0.jar')
                classpath "org.javassist:javassist:3.28.0-GA"
            }
        }
        allprojects {
            repositories {
                google()
                mavenCentral()
                flatDir {
                    dirs "libs"
                }
            }
        }
        ```

  4. Into module's `build.gradle` add:

        ```groovy
        plugins {
            id 'com.android.application'
            id 'com.pubguard.pubguard-gradle-plugin'
        }

        dependencies {

            // Pubguard
            implementation files('libs/pubguard-1.3.0.aar')

            // android
            implementation 'androidx.core:core-ktx:1.3.2'
            implementation 'com.google.android.gms:play-services-basement:17.6.0'
            implementation 'androidx.webkit:webkit:1.4.0'

            // kotlin
            implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
            implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.3'
            implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.4.3'

            // networking
            implementation 'com.squareup.retrofit2:retrofit:2.6.4'
            implementation 'com.squareup.retrofit2:converter-protobuf:2.6.4'
            implementation 'com.google.protobuf:protobuf-java:3.19.3'

            // other 
            implementation 'org.apache.commons:commons-text:1.9'
            implementation 'org.jsoup:jsoup:1.14.2'
            implementation 'org.aspectj:aspectjrt:1.9.7'

        }
        ```

</details>

### Initialising the Framework

The Pubguard SDK should be initialized once at app launch. It is recommended to initialize the SDK in the Application subclass. Here's an example of how to call the init method in the Application subclass:

```java
import com.pubguard.Pubguard;
…

public class MyApplication extends Application {

    @Override public void onCreate() {
        super.onCreate();

        try {
            Pubguard.init(application, "YOUR_API_KEY");
        } catch (Exception e) {
            Log.e(TAG, "Pubguard Init exception: " + e.getMessage());
        }
    }
}
```

**Note** All initialization parameters are mandatory and an exception will be thrown if null or empty string is passed.

- `application` is your apps `Application` class
- `YOUR_API_KEY` is a `String` of your **Android API Key** that can be found in the Pubguard dashboard

---

# Changelog

For all release notes and previous versions please see our [changelog](ChangeLog.md).

---

# Support

### Advertising SDK support

These are ad network SDKs specifically supported by Pubguard to analyse advertising content in your app, if you would like information on a version or vendor that is not on this list please email support@pubguard.com

| Company/Product           | Android SDK identifier                        | Android versions                                              |
| ------------------------- | --------------------------------------------- | ------------------------------------------------------------- |
| AdColony                  | com.adcolony:sdk                              | 4.1.0 - 4.3.0;<br/> 4.6.1 - 4.6.5                             |
| AdMob                     | com.google.android.gms:play-services-ads      | 19.0.0 - 20.5.0                                               |
| AppLovin                  | com.applovin:applovin-sdk                     | 9.14.4 - 9.14.5;<br/> 10.3.1 - 11.0.0                         |
| Chartboost                | com.chartboost:chartboost-sdk                 | 7.5.0;<br/> 8.1.0 - 8.3.1                                     |
| Fyber Marketplace         | com.fyber.vamp:core-sdk/:video-kit/:mraid-kit | 7.7.2;<br/> 7.8.2 - 7.8.4                                     |
| InMobi                    | com.inmobi.monetization:inmobi-ads            | 9.0.1;<br/> 9.1.0;<br/> 9.1.7 - 10.0.1                        |
| ironSource                | com.ironsource.sdk:mediationsdk               | 6.14.0.1;<br/>6.16.1;<br/> 6.18.0 - 7.0.3;<br/>7.1.1 - 7.1.5.1;<br> 7.1.8 - 7.1.14 |
| Mintegral                 | com.mbridge.msdk:mbbanner/:reward/:interstitial/...| 15.6.21 - 15.8.01                                        |
| MoPub                     | com.mopub:mopub-sdk                           | 5.14.0;<br/>5.16.4-5.18.0                                     |
| Smaato                    | com.smaato.android.sdk:smaato-sdk             | 21.5.3 - 21.5.4;<br/>21.6.1 - 21.6.9                          |
| Tapjoy                    | com.tapjoy:tapjoy-android-sdk                 | 12.6.1 - 12.9.0                                               |
| Unity Ads                 | com.unity3d.ads:unity-ads                     | 3.4.2 - 3.5.0; 3.6.0;<br/>3.7.2 - 4.0.0                       |
| Verizon                   | com.verizon.ads:android-vas-standard-edition  | 1.2.0;<br/>1.8.0 - 1.8.2;<br/>1.12.0 - 1.14.0                 |
| Vungle                    | com.vungle:publisher-sdk-android              | 6.7.1 - 6.8.1;<br/>6.9.1 - 6.10.3                             |
| Verve HyBid               | net.pubnative:hybid.sdk                       | 2.5.2 - 2.9.0;                                                |

---

# Versioning

Please use the most up to date version at all times to ensure maximum support.

---

# License

_© 2021 Minimised Media Limited (Pubguard© 2019 All Rights Reserved)_
