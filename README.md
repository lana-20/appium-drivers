# Appium Drivers

*Just like Selenium, Appium has structured its automation around the concept of drivers that unlock automation capabilities for a certain platform. There are a few differences between Appium and Selenium drivers, though.*

### Appium Driver History
<img width="400" src="https://user-images.githubusercontent.com/70295997/226083418-13f51af6-a753-4676-ba22-2475baa8b24d.png">

Appium provides support for different platforms in much the same way that Selenium does, namely by the use of independent bits of software that know how to target one particular platform for automation. These bits are called drivers in the Appium world, just like they are for Selenium.

Appium started out with one driver, for iOS. Then Android support was added, bringing the driver count to two. Eventually, many more drivers were created, to target platforms beyond even mobile devices. Originally, just as with Selenium, all drivers lived inside the Appium codebase, and were a part of the Appium software whenever you downloaded Appium.

### Appium Drivers Today

<img width="700" src="https://user-images.githubusercontent.com/70295997/226083468-c9c41392-2206-43c8-aa1f-f70607f2bd0d.png">

With Appium 2.0, the Appium project has adopted a model a little closer to the WebDriver ecosystem, where drivers are actually now independent from the main Appium server. There are still some differences between the two projects and how they organize drivers, however. Web browser drivers are maintained by the browser vendors, whereas all the mobile drivers are still unofficial, third-party projects from the position of Apple and Google.

Another difference is that Selenium doesn't manage web drivers for you. Even if you want to use Selenium, you need to figure out how to download and maintain versions of the drivers that you need. With Appium, however, you get a command line tool as part of the Appium software that lets you manage which drivers you have installed.

On top of this, Appium actually has a bit more robust ecosystem for drivers. Appium itself provides tools for anyone in the world to write their own Appium driver, to connect up automation for an existing or brand new software platform. And this makes sense. The goal of Selenium was to automate web browsers, and now each browser has its own driver maintained by the makers of the browser itself. Appium's goal is a bit broader, namely to bring WebDriver style automation to any platform. There's no way the Appium team can do that on their own, so instead they provide tools that make it easy for any developer to create a WebDriver implementation for a new platform. These new drivers can then easily be installed and shared using Appium's driver interface.

Well, what drivers currently exist for Appium? Later on, we'll talk about ways to get a current list from the Appium software itself. But for now, we can say that there are 8 drivers that the Appium team officially recognizes: 2 for Android, 1 for iOS, and the rest for a variety of other platforms. As the driver ecosystem grows, this number will obviously also increase.

1. Let's talk about the iOS driver first. It's called the Appium XCUITest driver, because it is based on the XCUITest automation technology from Apple that we discussed in the unit on mobile automation tools.

2. Next is the first of the two Android drivers. It's called the "UIAutomator 2" driver, because it is built on top of Android's UiAutomator 2 automation APIs.

3. Third is a newer Android driver, called the Espresso driver. Even though Espresso has an automation model quite different from UiAutomator 2, which allows for gray box testing, the Appium team figured out a way to wrap up it up in a WebDriver compatible interface.

4. Fourth is the Appium Windows driver, which provides automation access to Windows desktop environments. It's powered by a piece of technology produced by Microsoft called WinAppDriver, which is already itself a WebDriver-compatible automation server! In this case we do actually have something similar to the situation with browsers, where the platform developer itself has put together a WebDriver compatible automation tool. Great job, Microsoft!

5. Fifth is the Appium Mac driver, which is built on top of macOS's accessibility protocols for the purpose of automating macOS desktop apps. Unfortunately, it's not maintained by Apple itself, and has a pretty limited feature set at the moment.

6. Sixth, we have the Tizen driver. Tizen is a mobile operating system, competitive with Android and iOS, produced by Samsung. It's used mostly in wearables and smart TVs. In this case, Samsung itself developed the Tizen driver, so it has great support for automating the Tizen platform. Thanks, Samsung!

7. Seventh, we have the Flutter driver. Flutter is a native app development framework from Google, similar to Facebook's React Native framework. Flutter allows developers to write apps in the programming language Dart, and have the framework handle conversion to native code for iOS and Android. The Appium Flutter driver enables working with elements as defined in the Flutter layer, rather than only the ultimately produced native elements.

8. Finally, as another example of a third-party driver, we have the Youi.tv driver, which makes automation easy for developers using the Youi.tv cross-platform TV app development framework.

In this course, we are obviously focusing on the use of Appium for testing mobile apps, which is far and away the most popular use for Appium. By and large, what you learn here will also be relevant for other platform drivers too! But each driver has its own subset of the WebDriver protocol that it supports, and each driver might additionally support some features which are specific to that platform or which only make sense on that platform. So it's always a good idea to familiarize yourself with the documentation for a particular driver when you begin to use it.

In a perfect world, each Appium driver would implement the WebDriver protocol in exactly the same way across all platforms. But unfortunately this is not always possible. The differences between the iOS and Android platforms, for example, are vast, compared to the differences between Safari and Chrome. Safari and Chrome both render webpages according to a certain standard, but there is no standard mobile app language or framework which iOS and Android both support.

Because of this, the automation tools available for all the various Appium platforms differ in which features they provide, and in some cases, how they implement the same basic features.

Of course, the Appium team works as hard as they can to make sure that the same commands are available on both platforms, and the same commands across both platforms cause the same behavior. And sometimes this takes a lot of doing!

But one consequence of the fact of these differences is that cross-platform test code is not always as easy to write as it is for the web. It's true that you might wind up in a situation where you need to write a bit of test code that does one thing on iOS and a different thing on Android. In those cases, there are ways to structure your code to make it as clean as possible, which we'll discuss later on.

Currently, there is not more than one supported driver for a given platform, except in the case of Android. So for most platforms, the choice of which driver to use is easy. But what about Android? How do you know whether to use the Espresso driver or the UiAutomator 2 driver? Unfortunately, this choice matters quite a lot, since each driver works differently enough that you can't assume your tests will run without modification on both drivers.

The way to pick a driver is to evaluate the underlying technologies used by the driver, as well as the feature set provided by the driver. Ultimately you will want to stick with a driver for your testsuite for quite some time. In the case of Android, you could think about whether you need to automate apps beyond your own, for example. If so, you probably need to use the UiAutomator 2 driver.

In general, the UiAutomator 2 driver supports more features, though the Espresso driver does come with a few features that the UiAutomator 2 driver does not have, like the ability to target and execute code within your application directly.

Before you start a new project, it's always a good idea to go through the available driver features and understand what the situation is.

So, what happens if you find yourself in a position of having written a testsuite using one Appium driver, and now you want to switch drivers for your platform to a newer one, maybe one that's based on newer technology? This happened with iOS in quite a big way some years ago, when Apple deprecated their iOS UIAutomation Javascript API and decided to support only XCUITest. All Appium users had to migrate their driver from the old iOS driver to the new XCUITest driver.

At the end of the day, this was not nearly as big of a headache as if they had written their tests using Apple's tools directly. With Appium, the API stayed stable enough that only a few small changes had to be made. And that is exactly the promise of Appium, that it provides a stable WebDriver compatible API on top of a shifting set of underlying technologies.

And of course, as far as possible, Appium drivers do try to implement the same set of WebDriver commands in the exact same way, so that migration and cross-platform scripts are a reality and not an impossibility. 





