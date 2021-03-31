#下载彼岸图片
# 导入必要的包
from selenium import webdriver
from bs4 import BeautifulSoup
import requests
import os


'''
#爬取的资源类型
pic_type 4kmeinv   4kfengjing   4kyouxi   4kyingshi 4kdongman 4kmingxing 4kqiche 4krenwu 4kdongwu 4kmeishi 4kzongjiao 4kbeijing
'''
pic_type = '4kdongman'
# 初始化一个引用计数，用于后面的图片简单命名
index = 1
#开始的页数
start_page=1
#结束的页数
end_page=2
#扫描的图片地址
image_list=[]
#解析到的源图片地址
source_images=[]

# 定义爬虫方法
def getImage():
    '''
        扫描获取图片地址
    '''
    # 将index置为全局变量
    global index
    # 循环爬取，循环多少次爬取多少页的图片
    print('==============开始搜索符合条件的图片==============')
    for i in range(start_page, end_page):
        # 模拟点击下一页，因为爬取完一页需要点击下一页爬取
        # driver.find_element_by_link_text("下一页").click()
        # https://pic.netbian.com/4kmeinv/index_2.html
        if i == 1:
            imageUrl = 'https://pic.netbian.com/{0}'.format(pic_type)
        else:
            imageUrl = 'https://pic.netbian.com/{0}/index_{1}.html'.format(pic_type,i)
        r = requests.get(imageUrl)
        # 解析网页
        html = BeautifulSoup(r.text, 'html.parser')
        # 获取原图的url链接
        links =html.find('div', {'class': 'slist'}).find_all('a')
        for link in links:
            # 遍历当页获得的所有原图链接
            imageUrl = "http://pic.netbian.com"  + link.get('href')
            if not str(imageUrl).__contains__(pic_type) and imageUrl not in image_list:
                image_list.append(imageUrl)
                print('🌈 扫描资源 =>' + imageUrl)
    print('==============图片资源搜索完毕==============')
    parseImageList()



def parseImageList():
    '''
    解析图片的操作
    '''
    print('=============正在解析搜索到的图片资源=============')
    for imageUrl in image_list:
        r = requests.post(imageUrl)
        html = BeautifulSoup(r.text, 'html.parser')
        # 获取原图的url链接
        links = html.find('div', {'class': 'photo-pic'}).find_all('img')
        for link in links:
            source_image = "https://pic.netbian.com" + link.get('src')
            if source_image not in source_images:
                source_images.append(source_image)
                print('🍉 解析到资源=>' + source_image)
    print('============图片资源解析完毕==========')
    saveImage(source_images)


# 保存图片
def saveImage(links):
    '''
    保存图片操作
    :param links: 源图片地址
    :return: 返回保存的数据
    '''
    print('==================正在保存扫描到的图片====================')
    images_file = os.path.join(os.path.expanduser("~"), 'Desktop') + "/images_file/"
    if not os.path.exists(images_file):
        os.makedirs(images_file)

    for index in range(0, len(links)):
        link = links[index]
        print("✅ 正在爬取图片 => %s" %link)
        # 将原图存至当前目录下的test/img 文件夹，以index命名，后缀名为图片原名的后三位，即jpg或者gif
        with open(images_file + '{}.{}'.format(index, link[len(link)-3: len(link)]), 'wb') as jpg:
            jpg.write(requests.get(link).content)
        index += 1
    print('============所有图片保存完毕==========')


# 定义主函数
if __name__ == '__main__':
    '''
    主函数：调用爬取操作
    '''
    getImage()
