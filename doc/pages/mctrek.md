# mctrek

## advicer

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/mctrek/advicer.1.py) -->
<!-- The below code snippet is automatically added from ../../python/mctrek/advicer.1.py -->
```py
from pprint import pprint
# pip install selenium
# wget `curl -s https://api.github.com/repos/mozilla/geckodriver/releases/latest | grep browser_download_url | grep linux64 | cut -d '"' -f 4`
# export PATH="$PATH:/path/to/geckodriver/folder"
from selenium import webdriver
from xvfbwrapper import Xvfb

start_page = "http://www.mctrek.de/shop/sale?hersteller[]=Jack+Wolfskin&suchevon="

# list_of_size = page.xpath('//*[@id="wk_addItem"]/option/text()')
# print(list_of_size)
# list_of_size = 
# print(list_of_size[0].value_options)


def next_page_generator():
    index = 0
    while True:
        yield start_page + str(index*24)
        index = index+1

next_page = next_page_generator()
html_url = next_page.next()
xpath = '/html/body/div[9]/div/div[2]/div[2]/div[6]/div[4]/a/div[1]'

# virtual display init
vdisplay = Xvfb()
vdisplay.start()
response = webdriver.Firefox()
response.get(html_url)
search_element = response.find_elements_by_xpath(xpath)
# pprint(search_element)
# print(dir(search_element))
for each in search_element:
	print(each.get_attribute('innerHTML').strip())
vdisplay.stop()
```
<!-- MARKDOWN-AUTO-DOCS:END -->



## advicer

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/mctrek/advicer.py) -->
<!-- The below code snippet is automatically added from ../../python/mctrek/advicer.py -->
```py
from lxml import html

start_page = "http://www.mctrek.de/shop/sale?hersteller[]=Jack+Wolfskin&suchevon="

# list_of_size = page.xpath('//*[@id="wk_addItem"]/option/text()')
# print(list_of_size)
# list_of_size = 
# print(list_of_size[0].value_options)


def next_page_generator():
    index = 0
    while True:
        yield start_page + str(index*24)
        index = index+1

next_page = next_page_generator()
html_url = next_page.next()
print(html_url)
page = html.parse(html_url)
# found_elements = page.xpath('/html/body/div[9]/div/div[2]/div[2]/div[6]')
found_elements = page.xpath('/html/body/div[9]/div/div/div')
print(dir(found_elements[0]))
print(found_elements[0].attrib)
for element in found_elements[0].getchildren():
    print (">>>> %s   %s" % (element.tag, element.attrib))
print(found_elements)
```
<!-- MARKDOWN-AUTO-DOCS:END -->


