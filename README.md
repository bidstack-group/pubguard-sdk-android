![alt text](images/pg-logo.png)

# Pubguard Android Library

A guide to installing Pubguard on your Android application

The Pubguard Library is solution that monitors the advertising content flowing through your mobile app, protecting against unwanted content and optimising revenues.

The data from the library is then accessible via your account on the Pubguard interface where you can set up preferences, alerts and browse the gallery.

---

Table of contents
=================

<!--ts-->
* [Getting Started](#getting-started)
* [Integrating](#integrating)
* [Change Log](#change-log)
* [Library Size](#library-size)
* [SDK support](#support)
* [Requirements](#requirements)
* [Versioning](#versioning)
* [License](#license)
<!--* [FAQs](https://github.com/bidstack-group/pubguardSDK/wiki/FAQs)-->

<!--te-->

# Getting Started

These instructions will enable you to get the Pubguard library running on your Android app.

## Prerequisites

Before installing the pubguard library you will need an application key which is available from your account, in order to receive a key please signup from http://dashboard.pubguard.com or email support@pubguard.com. The key is used in both the iOS and Android installations.

```
pubguardKey = "xxxxxxxxxxxxxxxxxxx"
```

---

# Integrating


The latest version of the Android Pubguard Library is **1.0.0**

Pubguard supports Android Gradle Plugin 3.4.0 - 4.1.1 and Gradle Wrapper 5.6 - 6.8

**Note: do not minify the Pubguard library**



### Installing locally

* Copy pubguard.aar and aspectj.jar into your main app module libs folder
* Into project's `build.gradle`  add:

  ```groovy
  buildscript {
      dependencies {
          classpath files('app/libs/aspectj.jar')
          classpath "org.aspectj:aspectjtools:1.9.4"
          classpath "org.aspectj:aspectjrt:1.9.4"
          classpath "org.javassist:javassist:3.27.0-GA"
      }
  }
  allprojects {
    repositories {
        flatDir {
            dirs "libs"
        }
    }
  }
  ```

* Into module's `build.gradle` add:

  ```groovy
  apply plugin: 'com.bidstack.pubguard.aspectj-ext'

  android {
      compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
  }

  dependencies {

    // Pubguard
    implementation files('libs/pubguard.aar')

    // android
    implementation "androidx.core:core:1.3.1"
    implementation 'com.google.android.gms:play-services-basement:[GOOGLE_AD_VERSION]'
    implementation 'androidx.webkit:webkit:1.2.0'

    // kotlin
    implementation "androidx.core:core-ktx:1.3.1"
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.7'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.3'

    // networking
    implementation 'com.squareup.retrofit2:retrofit:2.6.3'

    // networking  converters
    implementation 'com.squareup.retrofit2:converter-gson:2.6.2'
    implementation 'com.google.code.gson:gson:2.8.5'
    implementation 'com.squareup.retrofit2:converter-protobuf:2.6.3'
    implementation 'com.google.protobuf:protobuf-java:3.6.1'

  }
  ```


**Note** that we require `implementation 'com.google.android.gms:play-services-basement:[GOOGLE_AD_VERSION]'`
[GOOGLE_AD_VERSION] Version is based on your google ads version as per above. Even if you are not using Google ads, you still need to add support for Google services.



#### Initialising the Library

The Pubguard Library should be initialized once at app launch. Here's an example of how to call the init method in Application class:
```
import com.bidstack.pubguard.Pubguard;
…

public class MyApplication extends Application {

    @Override public void onCreate() {
        super.onCreate();

        try {
            Pubguard.init(application, "YOUR_PUBGUARD_KEY");
        } catch (Exception e) {
            Log.e(TAG, "Pubguard Init exception: " + e.getMessage());
        }
    }
}
```

**Note** All initialization parameters are mandatory and exception will be thrown if null or empty string is passed.
  - `application` is your apps `Application` class
  - `YOUR_PUBGUARD_KEY` is a `String` of your publisher key that can be found in Pubguard console

---

## Changelog


For all release notes and previous versions please see our [changelog](changelog.md).

---

## Library Size

The Pubguard team understands the importance of having a small footprint and our library is optimised to be as lightweight as possible on both iOS and Android.

Here is a guide based based on our compiling with our test apps, please bear in mind the size may increase or reduce based on the amount of SDKs you use and the amount of shared dependancies.

| Dependencies | Size |
| --- | --: |
| Pubguard SDK | ~586KB |
| Play Services Basement | ~324KB |
| Core Kotlin Extensions | ~159KB |
| Kotlin Stdlib Jdk7 | ~22KB |
| Kotlin Coroutines Core | ~1,7MB |
| Kotlin Coroutines Android | ~20KB |
| Retrofit2 Converter GSON | ~5KB |
| GSON | ~240KB |
| Retrofit2 Converter Protobuf | ~5KB |
| Protobuf Java | ~1,4MB |
| **Total** | ~4.5MB |

---

## Support

### Advertising SDK support

These are SDKs designed specifically for serving advertising content into your app, if you would like information on a version or vendor that is not on this list please email support@pubguard.com

| Company/Product | Android SDK identifier | Android versions |
| --- | --- | --- |
| AdColony | com.adcolony:sdk |  4.1.0 - 4.3.0  |
| AdMob | com.google.android.gms:play-services-ads | 19.0.0 - 19.5.0 |
| Amazon Mobile Ads | com.amazon.android:mobile-ads | 6.0.0 |
| AppLovin | com.applovin:applovin-sdk | 9.14.4 - 9.14.5 |
| Chartboost | com.chartboost:chartboost-sdk | 7.5.0;<br/> 8.1.0 - 8.2.0 |
| Facebook Audience Network | com.facebook.android:audience-network-sdk | 5.8.0 - 6.1.0 |
| Fyber Marketplace | com.fyber.vamp:core-sdk/:video-kit/:mraid-kit | 7.7.2 |
| InMobi | com.inmobi.monetization:inmobi-ads | 9.0.1;<br/> 9.1.0 |
| ironSource | com.ironsource.sdk:mediationsdk | 6.14.0.1;<br/> 6.16.1;<br/> 6.18.0 - 7.0.3 |
| MoPub | com.mopub:mopub-sdk | 5.14.0 |
| Smaato | com.smaato.android.sdk:smaato-sdk | 21.5.3 - 21.5.4 |
| Unity Ads | com.unity3d.ads:unity-ads | 3.4.2 - 3.5.0 |
| Verizon | com.verizon.ads:android-vas-standard-edition | 1.2.0; 1.8.0 - 1.8.2 |
| Vungle | com.vungle:publisher-sdk-android | 6.7.0 - 6.8.1 |

---

## Requirements

+ Android 4.4 (API 19) and up
+ Android Gradle Plugin 3.4.0 and up
+ Gradle Wrapper 5.6 and up

---

## Versioning

Please use the most up to date version at all times to ensure maximum support.

---

## License

*© 2019 Minimised Media Limited (Pubguard© 2019 All Rights Reserved)*
