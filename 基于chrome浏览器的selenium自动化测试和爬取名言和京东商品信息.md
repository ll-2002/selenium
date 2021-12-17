@[toc]

# 一、selenium介绍

Selenium是一系列基于Web的自动化工具，提供一套测试函数，用于支持Web自动化测试。函数非常灵活，能够完成界面元素定位、窗口跳转、结果比较。具有如下特点：


   一、多浏览器支持

           可以对多浏览器进行测试，如IE、Firefox、Safari、Chrome、Android手机浏览器等。

   二、支持多种语言

          如Java、C#、Python、Ruby、PHP等。

  三、支持多种操作系统


         如Windows、Linux、IOS、Android等。

  四、开源免费

         官网：http://www.seleniumhg.org/

# 二、准备工作

## 1、下载selenium

可以直接用 pip install selenium下载
![在这里插入图片描述](https://img-blog.csdnimg.cn/d221dcf4a19a46b8ab9b1fb6ce75e62b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_20,color_FFFFFF,t_70,g_se,x_16)

## 2、下载chrome浏览器驱动

查看chrome的版本
![在这里插入图片描述](https://img-blog.csdnimg.cn/4bf6a317bbd54c489bfc964f65a46b65.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_20,color_FFFFFF,t_70,g_se,x_16)
在[https://npm.taobao.org/mirrors/chromedriver/](https://npm.taobao.org/mirrors/chromedriver/)下载对应的驱动
![在这里插入图片描述](https://img-blog.csdnimg.cn/b3e59b021c2c45fe960044aa74573150.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_20,color_FFFFFF,t_70,g_se,x_16)

# 三、自动化测试

 - 测试一下是否安装成功

```python
from selenium import webdriver
driver=webdriver.Chrome('D:\\py\\chromedriver_win32\\chromedriver.exe')
#进入网页
driver.get("https://www.baidu.com/")

```
可以看到成功运行谷歌浏览器
![在这里插入图片描述](https://img-blog.csdnimg.cn/2bc28724577744dd9bffadd798197dcb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_20,color_FFFFFF,t_70,g_se,x_16)

# 四、爬取名言

```python
from bs4 import BeautifulSoup as bs
from selenium import webdriver
import csv
from selenium.webdriver.chrome.options import Options
driver=webdriver.Chrome('D:\\py\\chromedriver_win32\\chromedriver.exe')
driver.get('http://quotes.toscrape.com/js/')
```

 - 定义csv表头和文件路径，以及存放内容的列表

```python
#定义csv表头
quote_head=['名言','作者']
#csv文件的路径和名字
quote_path='..\\source\\mingyan_csv.csv'
#存放内容的列表
quote_content=[]

```

 - 进入开发者模式，定位名言的位置，观察名言的位置

![在这里插入图片描述](https://img-blog.csdnimg.cn/6ccbb58dc6a04029851920f18da0f8b8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_20,color_FFFFFF,t_70,g_se,x_16)

 - 爬取信息并存入

```python
def write_csv(csv_head,csv_content,csv_path):
    with open(csv_path, 'w', newline='',encoding='utf-8') as file:
        fileWriter =csv.writer(file)
        fileWriter.writerow(csv_head)
        fileWriter.writerows(csv_content)
        print('爬取信息成功')
        quote=driver.find_elements_by_class_name("quote")
#将要收集的信息放在quote_content里
for i in tqdm(range(len(quote))):    
    quote_text=quote[i].find_element_by_class_name("text")
    quote_author=quote[i].find_element_by_class_name("author")
    temp=[]
    temp.append(quote_text.text)
    temp.append(quote_author.text)
    quote_content.append(temp)
write_csv(quote_head,quote_content,quote_path)


```

 - 爬取成功

![在这里插入图片描述](https://img-blog.csdnimg.cn/e5949bfd7c3642f8820de7246653b3c8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_17,color_FFFFFF,t_70,g_se,x_16)

# 五、爬取淘宝商品信息

 - 导入包并进入淘宝首页

```python
from selenium import webdriver
import time
import csv
driver=webdriver.Chrome('D:\\py\\chromedriver_win32\\chromedriver.exe')
driver.get("https://www.taobao.com/")

```

 - 定义存放爬取信息的路径和存放内容的列表以及爬取数量

```python
#定义存放图书信息的列表
goods_info_list=[]
#爬取200本
goods_num=200
#定义表头
goods_head=['价格','名字','链接']
#csv文件的路径和名字
goods_path='..\\source\\goods.csv'

```

 - 进入开发者模式，定位搜索框，可以看到搜索框的id为key，获取按钮输入信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/0b438ec5713f47e6a47fac12c4dba8a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_14,color_FFFFFF,t_70,g_se,x_16)
 - 获取输入框并输入信息

```python
#向输入框里输入薯片
p_input = driver.find_element_by_id("key")
p_input.send_keys('薯片')

```

 - 获取商品价格、名称、链接的函数

```python
#获取商品价格、名称、链接
def get_prince_and_name(goods):
    #直接用css定位元素
    #获取价格
    goods_price=goods.find_element_by_css_selector('div.p-price')
    #获取元素
    goods_name=goods.find_element_by_css_selector('div.p-name')
    #获取链接
    goods_herf=goods.find_element_by_css_selector('div.p-img>a').get_property('href')
    return goods_price,goods_name,goods_herf

```

 - 定义滑动到页面底部函数，滑动到页面底部会刷出下一页

```python
def  drop_down(web_driver):
    #将滚动条调整至页面底部
    web_driver.execute_script('window.scrollTo(0, document.body.scrollHeight)')
```

 - 定义爬取一页的函数

```python
#获取爬取一页
def crawl_a_page(web_driver,goods_num):
    #获取图书列表
    drop_down(web_driver)
    goods_list=web_driver.find_elements_by_css_selector('div#J_goodsList>ul>li')
    #获取一个图书的价格、名字、链接
    for i in tqdm(range(len(goods_list))):
        goods_num-=1
        goods_price,goods_name,goods_herf=get_prince_and_name(goods_list[i])
        goods=[]
        goods.append(goods_price.text)
        goods.append(goods_name.text)
        goods.append(goods_herf)
        goods_info_list.append(goods)
        if goods_num==0:
            break
    return goods_num

```

 - 运行结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/003ba15adbb6482ab2fc7fe3489a9bd5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c5b6Q5Z2kMDAx,size_10,color_FFFFFF,t_70,g_se,x_16)

# 六、总结

Selenium是一系列基于Web的自动化工具，提供一套测试函数，用于支持Web自动化测试。函数非常灵活，能够完成界面元素定位、窗口跳转、结果比较。

# 七、参考链接
[https://blog.csdn.net/wanglian2017/article/details/72843984?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163974182016780269820611%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163974182016780269820611&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-72843984.pc_search_result_cache&utm_term=selenium%E4%BB%8B%E7%BB%8D&spm=1018.2226.3001.4187](https://blog.csdn.net/wanglian2017/article/details/72843984?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163974182016780269820611%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=163974182016780269820611&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-72843984.pc_search_result_cache&utm_term=selenium%E4%BB%8B%E7%BB%8D&spm=1018.2226.3001.4187)
[https://blog.csdn.net/weixin_44019406/article/details/88681312](https://blog.csdn.net/weixin_44019406/article/details/88681312)
