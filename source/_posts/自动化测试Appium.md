title: 自动化测试Appium
date: 2016-09-12 00:00:01
tags: [自动化测试]


---

# 平台信息
官网  http://appium.io/index.html?lang=zh
github https://github.com/appium/appium



# 使用
> brew install node      # get node.js
> npm install -g appium  # get appium
> npm install wd         # get appium client
> appium &               # start appium
> node your-appium-test.js


# 下载客户端
http://appium.io/downloads.html
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/43180156.jpg)


 ---
# Appium测试程序范例
>dotnet,java,node,perl,php,python,ruby


https://github.com/appium/sample-code/tree/master/sample-code/examples


# examples/node/android-webview.js
https://github.com/appium/sample-code/blob/master/sample-code/examples/node/android-webview.js


---
# 脚本语言API
`java` `Java io.appium`
https://search.maven.org/#search%7Cga%7C1%7Cg%3Aio.appium%20a%3Ajava-client


`python` `Appium-Python-Client`
https://pypi.python.org/pypi/Appium-Python-Client


`javaScript(node)` `WebDriver/Selenium 2 node.js client (js脚本API)`
https://www.npmjs.com/package/wd


--
# 示例代码
```
#coding=utf-8
from appium import webdriver


desired_caps = {}
desired_caps['platformName'] = 'Android'
desired_caps['platformVersion'] = '4.4.2'
desired_caps['deviceName'] = 'Android Emulator'
desired_caps['appPackage'] = 'com.android.calculator2'
desired_caps['appActivity'] = '.Calculator'


driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
driver.find_element_by_name("1").click()
driver.find_element_by_name("5").click()
driver.find_element_by_name("9").click()
driver.find_element_by_name("delete").click()
driver.find_element_by_name("9").click()
driver.find_element_by_name("5").click()
driver.find_element_by_name("+").click()
driver.find_element_by_name("6").click()
driver.find_element_by_name("=").click()
driver.quit()
```


` Appium移动自动化测试（四）--one demo - 推酷 `
http://www.tuicool.com/articles/jYBRfiu


---

` android 自动化测试 appium - 推酷 `
http://www.tuicool.com/articles/6bAnAb

