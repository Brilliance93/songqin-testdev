# Appium 作业 4 

<br>

- 找到一个安卓设备（没有可以向朋友借用一下）
- 安装华为应用市场apk，进入此目录 http://url.cn/5vUhy2m  下载HiSpace.apk
  注意安装运行的时候会提示更新，一定！！不要！！去更新。新的版本需要华为账号。
  
- 进入排行页面，滚动到 口碑最佳 部分
- 打印出所有 口碑最佳 部分的5个应用名称





## 参考答案，往下翻
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>


```python
# coding=utf8
from appium import webdriver
import time
import traceback


desired_caps = {}
desired_caps['platformName'] = 'Android'
desired_caps['platformVersion'] = '6'
desired_caps['deviceName'] = 'test'
desired_caps['app'] = r'd:\apk\HiSpace.apk'
desired_caps['appPackage'] = 'com.huawei.appmarket'  #app package名,指定了要运行的app
desired_caps['appActivity'] = 'com.huawei.appmarket.MainActivity' #app默认Activity
desired_caps['unicodeKeyboard']  = True
desired_caps['noReset'] = True
desired_caps['newCommandTimeout'] = 6000
driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps) #启动Remote RPC
driver.implicitly_wait(10)

try:
    # ------------------------------
    javaCode = 'new UiSelector().resourceId("com.huawei.appmarket:id/tabLayout").childSelector(new UiSelector().text("排行") )'

    driver.find_element_by_android_uiautomator(javaCode).click()

    javaCode = 'new UiSelector().text("总榜").resourceId("com.huawei.appmarket:id/ItemTitle")'
    ele = driver.find_element_by_android_uiautomator(javaCode)
    destPosY = ele.location['y']
    xPos = ele.location['x']

    driver.implicitly_wait(0.5)
    while True:
        driver.swipe(xPos,destPosY,xPos,destPosY-100, 1000)
        javaCode = 'new UiSelector().text("口碑最佳").resourceId("com.huawei.appmarket:id/ItemTitle")'
        eles = driver.find_elements_by_android_uiautomator(javaCode)
        if not eles:
            continue


        driver.swipe(xPos,eles[0].location['y'],xPos,destPosY,3000)
        break

    driver.implicitly_wait(10)


    eles = driver.find_elements_by_class_name("android.widget.TextView")
    tvs = []
    for ele in eles:
        tvs.append(ele.text)

    tvsStr = '|||'.join(tvs)

    pos1 = tvsStr.find('口碑最佳|||')
    targetStr = tvsStr[pos1:]




    def getName(No): #'1', '2'
        tp1 = targetStr.find(No+'|||')  + 4
        tp2 = targetStr.find('|||',tp1)
        return targetStr[tp1:tp2]


    print('================ finally ++++++++++++ \n')
    for i in range(1,6):
        print(getName(str(i)))



        # ------------------------------

except:
    print(traceback.format_exc())

input('**** Press to quit..')
driver.quit()
```
