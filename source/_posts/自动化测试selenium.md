title: 自动化测试selenium
date: 2016-09-12 00:00:00
tags: [ 自动化测试 ]
 
---


# 官方
`官网` 
http://www.seleniumhq.org/
http://www.seleniumhq.org/download/


`github`
https://github.com/SeleniumHQ/selenium


`独立服务器(Selenium Standalone Server)`
http://selenium-release.storage.googleapis.com/3.0-beta3/selenium-server-standalone-3.0.0-beta3.jar
>java -jar selenium-server-standalone-2.45.0.jar


`Selenium grid for selenium1 and webdriver`
https://github.com/SeleniumHQ/selenium/wiki/Grid2


---
# 开发指导
`github示例`
https://github.com/SeleniumHQ/selenium/tree/c10e8a955883f004452cdde18096d70738397788/javascript/node/selenium-webdriver


`javaScript(node) API docs`
http://seleniumhq.github.io/selenium/docs/api/javascript/index.html


`javaScript开发指导`
https://www.npmjs.com/package/selenium-webdriver


`基于chromedriver 开发指导`
https://sites.google.com/a/chromium.org/chromedriver/getting-started/getting-started---android


---


# 准备
- 安装selenium
```
cnpm install selenium-webdriver
```


- 下载driver
http://chromedriver.storage.googleapis.com/index.html
(已下载,见:`bin/chromedriver.exe`)


- 添加环境变量Path
```
path=%path%;%cd%\bin\chromedriver.exe
```


- 安装node


# 使用
- 行为指定(脚本-js语言) `baidu.js`
```
var webdriver = require('selenium-webdriver'),
    By = webdriver.By,
    until = webdriver.until;


var driver = new webdriver.Builder()
    .forBrowser('chrome')
    .build();


driver.get('https://www.baidu.com');
driver.findElement(By.id('kw')).sendKeys('webdriver');
driver.findElement(By.id('su')).click();
driver.wait(until.titleIs('webdriver_百度搜索'), 1000);
driver.quit();
```


- 执行
```
node baidu.js
```
---
# webdriver通过class获取元素

```
通过webdriver 取得页面元素的时候，有时候由于某些元素只有样式类，没有ID和NAME。这个时候我们就需要通过特别的方式获取该元素了。
1：当元素只有一个样式，比如 class="style1" ，这个时候可以通过：
find_element_by_class_name("style1")   获取
2：当元素多个样式的时候，比如 class="style1 style2 style3" ，这个时候可以通过：
 driver.find_element_by_css_selector(".style1.style2.style3")   获取
```
-  cssSelector
```
driver.findElement(By.className("current  time")).click();


WebElement element = driver.findElement(By.cssSelector("input[class='current time']"));
element.click();
```
-  xpath
```
WebElement element = driver.findElement(By.xpath("//a[@class='current time']"));
element.click();


WebElement element = driver.findElement(By.xpath("//a[text() = 'url']"));
element.click();
```


---
# error Info
```
WebDriverError: unknown error: cannot parse capability: chromeOptions
```
>webdriver版本问题


---
`JavaScript（Node.js）+ Selenium自动化测试 - 推酷`
http://www.tuicool.com/articles/ieqiiq3


`Selenium + Python 自动化测试环境搭建 - 推酷`
http://www.tuicool.com/articles/QRrQJzV


`Selenium Webdriver 基于浏览器的自动化测试 - 推酷`-java
http://www.tuicool.com/articles/amuuEve