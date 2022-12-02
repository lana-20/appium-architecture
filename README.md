# Appium Architecture/Ecosystem Diagram

Mobile Automation Framework Tool

[View Diagram Here](https://miro.com/app/board/o9J_lIIoGxA=/?share_link_id=28667859121)
![image](https://user-images.githubusercontent.com/70295997/205180266-f97c9354-ea31-462b-9c9c-21e1581612f8.png)

Mobile Automation for Native, Hybrid (Cross-Platform) or Web apps.

Appium has a typical Client-Server architecture.
Appium Server runs on the node server (is written in Node.js).

Client send Request to Server via HTTP in the forms of JSON Wire Protocol (WP) or a typical W3C WebDriver JSON. Along with the Request, Client sends Desired Capabilities (DC), which the Server is smart enough to interpret. DC can be anything, eg, types of devices, OS platforms, browsers, packages info, etc.

![image](https://user-images.githubusercontent.com/70295997/205179816-c54b1afc-c33e-48b4-8101-c10ab26229c3.png)

For iOS version < 9.3, use UIAutomation module. No need for WebDriver Agent, it directly connects to the app on a device/simulator.

After iOS >= 9.3, there is another component to know -  XCUITest Driver - which acts as my driver. For iOS >= 10, the XCUITest Driver is used internally. XCUITest Driver Libraries were developed by Apple.

XCUITest Driver sends Request to WebDriver Agent. WebDriver Agent was developed by Facebook/Meta specifically for iOS devices.

UIAutomation Module and XCUITest Driver are provided/propagated by Apple, not Selenium or Appium.

In scenarios with iOS < 9.3, use Bootstrap.js. Appium Server injects Bootstrap.js into the device/simulator side to perform actions such as .click(), .send_keys(), .tap(), .scroll(), or kill the app. The actions happen through UIAutomation module, which transfers these commands to Bootstrap.js.

In scenarios with iOS >= 9/3, use WebDriver Agent to perform some actions (tap, type, kill app, etc.). The actions are done by WebDriver Agent om a particular iOS device/simulator.

Appium 1.6 started implemeting the new flow of XCUITest for iOS >= 9.3. (Current version is 1.22.3-4, as of 12/01/22).

How does Appium [Server] decide which path to take? It's decided from the Client script, in the kinds of DC I pass.

From the DC point of view, when I pass __Desired Capabilites: {"AutomationName" : "XCUITest"}__, it means to take the XCUITest Driver flow for iOS >= 9.3. Most users have migrated to iOS 15 and 16, as of 12/01/22.

__Desired Capabilites: {"AutomationName" : "UIAutomation"}__ means to take the UIAutomation Module flow for iOS < 9.3.

When I initialize the WebDriver, I create an object of DC and pass it to Appium Server, which runs on some machine. Eg, 192.168.1.10:9268 stands for IP address:Port number. 
Through DC/RemoteWebDriver I connect to a particular Node Server and pass the DC object to the server.

![image](https://user-images.githubusercontent.com/70295997/205179971-40132ac2-9cfc-43d7-bc2b-f801a1a2bff3.png)

UIAutomator1 is the older version, which is deprecated.

Request is sent to UIAutomator2, which is an Appium module provided by Google. Request is then sent to an Android device/emulator. Through UIAutomator2 module, I perform actions like click, tap, scroll, etc. I use __Desired Capabilites: {"AutomationName" : "UIAutomator2"}__.

Appium-Android-Bootsrtap jar file helps UIAutomator perform actions on a particular app/device.

Espresso Driver was specially designed for Android OS by Google. For Android automation, I can use Espresso Driver in addition to UIAutomator2. 

Espesso Driver:
- extends Appium classes -> becomes available
- starts Espresso Server
- internally uses Appium Client libraries only

Espresso Server uses Espresso Modules, which are only used for Android devices.

-------

There is support for Windows. Microsoft provides Windows Driver, which gives me Win App Driver for Windows apps.

There is support for Mac. Apple provides Mac Driver, which gives me Appium Mac module to use for apps on Mac machines.

youI Driver is customized implementation available for youI-enabled apps.

Taizen Driver is for Samsung devices.

7 main Drivers (for Native or Hybrid/Cross-Platform apps):
1. UIAutomator2
2. Espresso
3. XCUITest
4. UIAutomation
5. Taizen
6. Windows
7. Mac

There is support for Browser. Eg, I want to launch Chrome browser on Android. There are 2 ways to do it. I can use (1) UIAutomator2 or (2) Espresso Driver. Both provide one concept - ChromeDriver. I use ChromeDriver to launch Chrome browser on my Android devices.

-------

Appium ecosystem is huge. Most of the time, I work with UIAutomator2 Driver (for Android devices) and XCUITest Driver (for iOS devices).

For iOS app automation, Appium created some wrappers to use on top of XCUITest libraries. That's why Appium is slightly slow on iOS devices. The iOS driver is provided by Appium to automate apps on iOS.

To make is faster, I can omit Appium and directly write my test cases using XCUITest library, which helps me connect to the app running on an iOS device.
XCUITest Library is separate thing for iOS, but I have to write code in Swift (or Obj C).


When automating for Android, I use a combo of both Appium [module] and UIAutomator2 Driver.
Eg, my .click() or .title is not working with the help of Appium, or some element is hidden when I try to tap on it. I use UIAutomator2 Driver which directly interacts with Android [OS] native APIs.


I can also create my own customized "Lana" driver. All drivers are modules, which are implementations of the core Appium [module]. The concept is the same -> start the server, connect the respective driver. In case of Espesso Driver, I can use Espresso Server.
Eg, I want to create a customized library on top of Appium, such as LanaAppiumDriver for Android and iOS.
![image](https://user-images.githubusercontent.com/70295997/205183692-a25ff1cc-fe8a-4f2a-920d-8d9969037b46.png)

--------

Because Appium ecosystem is so vast, I pick a particular driver based on requirements.






