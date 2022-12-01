# Appium Architecture/Ecosystem Diagram

Mobile Automation Framework Tool

[View Diagram Here](https://miro.com/app/board/o9J_lIIoGxA=/?share_link_id=28667859121)
![image](https://user-images.githubusercontent.com/70295997/205165870-65be474a-0518-4e94-9dde-36e50408b77f.png)

Mobile Automation for Native, Hybrid (Cross-Platform) or Web apps.

Appium has a typical Client-Server architecture.
Appium Server runs on the node server (is written in Node.js).

Client send Request to Server via HTTP in the forms of JSON Wire Protocol (WP) or a typical W3C WebDriver JSON. Along with the Request, Client sends Desired Capabilities (DC), which the Server is smart enough to interpret. DC can be anything, eg, types of devices, OS platforms, browsers, packages info, etc.

![image](https://user-images.githubusercontent.com/70295997/205171677-17d25555-8e14-4ca3-845b-cf147052fc17.png)

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


![image](https://user-images.githubusercontent.com/70295997/205173717-73da3dd6-f913-43c7-9fb1-21a0a6d4d42e.png)





