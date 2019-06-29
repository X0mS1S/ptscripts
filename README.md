# Auto_Download_Torrents_From_PTSite
A easy way to download torrents from private tracker sites with poor coding.

Update log:
2019/06/29
1. Integrating the function of downloading in NexusPHP sites, NexusPHP but encrypted download url sites and GazellePHP sites in one script. (Only tested in MT(normal nexusphp), HDC(encrypted) & DIC(gzphp) , not compatible with PTP)
2. Added an function to record the torrents has downloaded by creating an .log file in monitor path.
3. Perfecting the method of completing Headers' information.

2019/06/04
1. Added some lines to ensure that the script could download torrents correctly.
2. Added a new script to handle the encrypted torrent's download url.



Usage shows below:
用法如下，将根据注释进行解释：

# The requirements shows above, please use these commands to install the requirements in commandline. Using "sudo" in the case of meeting Permission problems.
# 使用前请先在命令行中安装需要的包, 看情况选择要不要用sudo:
pip install bs4
pip install requests
pip install lxml

# Complete the variables below:
# 完成下列的变量
# Some examples: 
#site_name = "M-TEAM"
#site_url = "https://tp.m-team.cc/torrents.php"
#site_cookie = "c_lang_folder=cht; tp=I2ODOGYNDFmZDdASDASODU3ZDA1ZU3ZDAYxNDFmZDdhYWRhZmRlOA%3D%3D"
#url_half = "https://tp.m-team.cc/"

site_name = "xxxxx"
site_url = "https://xxxxxxxxx/torrents.php"
site_cookie = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
# 如何获取cookie请自行学习

# It always would be the first part of your site, like: url_half = "https://tp.m-team.cc/"
# 一般来讲，这个变量就是你的网站的前半部分
url_half = "https://xxxxxxxxxxxxxxxxx/"


# If your site is a Gazelle Site, please change this varible to True, like: is_gazelle = True
# 如果是Gazelle构架，需要改成“is_gazelle = True”，经查发现，JPOP还有需要改其他变量才能使用，PTP无法使用，暂时确定能用的gz网站只有DIC
is_gazelle = False

# If the torrents' download url in your site were encrypted like HDC, please change this varible to True, like: is_encrypted = True
# 如果网站的种子下载地址，鼠标指上去发现跟种子ID无关，或不仅仅是种子ID，还带有加密hash之类的，需要设置成 True, 暂时发现的加密站为HDC，改成True后可用
is_encrypted = False

# 下面这几个现在不需要了
# Check the download url, especially when you are using a https(SSL) url.
# Some torrents' download pages url could be "https://tp.m-team.cc/download.php?id=xxxxx&https=1", in this case, you need to assign the variable of "url_last". Examples:
# url_last = "&https=1"
#url_last = ""


# If you couldn't downlaod the torrents to your directory where the *.py script is, you could just define the variables below. Make sure the format of your path because of the difference between Windows and Linux.
# Example of windows:              monitor_path = r'C:\\Users\\DELL-01\\Desktop\\'       Don't forget the last '\\'
# Example of Linux and MacOS:      monitor_path = r'/home/user/Downloads/'               Don't forget the last '/'
# 设置一个目录，下载软件监控这个目录，目录里的种子自动添加。可以不设置，一般会下载到脚本所在目录，或用户家目录。
# win下和linux下的路径表达方式不同，注意区分。
# win下：   monitor_path = r'C:\\Users\\DELL-01\\Desktop\\'
# linux下： monitor_path = r'/home/user/Downloads/'
monitor_path = r''


# Other informations for safer sites. Complete it if you cannot download torrents.
# 有的网站不需要headers信息，只填了cookie就可以了，有的需要headers，就请完善下列信息。如果你使用chrome的F12来看cookies,那headers就在你找到cookie的周围
# 一般只需要user_agent这一个headers信息，但如果不行，就尽量完善你能找到的headers信息，找不到的不需要管，留空即可
# DIC尽量完善headers信息，信息不够的话无法访问
# Some examples: 
#user_agent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.131 Safari/537.36'
#referer = 'https://tp.m-team.cc/login.php'
#host = 'tp.m-team.cc'

user_agent = ''

# You don't need to define the variables shows below unless you couldn't download the torrents after defined the above one
# 一般不需要定义下面的headers信息，下载失败再考虑完善
referer = ''
host = ''

# You don't need to define the variables shows below unless you couldn't download the torrents after defined the above two
# 优先级更低的headers信息
upgrade_insecure_requests = ''
dnt = ''
accept_language = ''
accept_encoding = ''
accept = ''
cache_control = ''
content_length = ''
content_type = ''
origin = ''
accept_encoding = ''


# Only if you just want to check the first 10 torrents is free in the page and download the free torrents in this small amount, please change it to 10
# 网页一次刷新，首页的种子信息就都抓取了，如果你只想看网页前10条种子信息里有没有免费，就把这个变量设置成10，一般可以配合搜索功能使用
# like: torrents_amount = 10
# We always grab all the torrents in the whole page, but you can define the amount of grabing torrents by defining the variable below 
torrents_amount = 0

# You don't need to change this variables unless you cannot download from your GAZELLE site
# 这是gz架构的网站才看到的东西，一般不用管。不过很多gz站应该都不一样，想抓gz站，要改的东西就比较多了，不介绍了
# check this value from the page source code
colspan = '3'

# You don't need to change this variables unless you cannot find free torrents correctly
# 这些是用来找free和找种子信息的标签，一般的nexusPHP站不用动，HDC已经专门适配，也不需要动
free_tag = 'pro_free'
free_tag2 = 'pro_free2up'
DIC_free_tag = 'torrent_label tooltip tl_free'
#PTP_free_tag = 'torrent-info__download-modifier--free'

torrents_class_name = '.torrentname'
HDC_torrents_class_name = '.t_name'
DIC_torrents_class_name = '.td_info'
#PTP_torrents_class_name = '.basic-movie-list__torrent-row--user-seeding'

download_class_name = '.rowfollow'
HDC_download_class_name = '.torrentdown_button'




#####
#####
# Showing the main script below:
# 下面是脚本中的主要执行部分，一般不用动，除非有特别的改动
#####
my_headers = get_my_headers(my_headers = {})

if not is_gazelle:
    if not is_encrypted:
        task = NexusPage(torrents_class_name)   ## The site would inform you that you have loged in this site when you run Page() at the very beginning.
        task = NexusPage(torrents_class_name)   ## So just run this command again to make sure that you can get the informations of torrents page.
        task_list = task.find_free(free_tag,free_tag2)
        download_free(torrents_amount, task_list, monitor_path)
    else:
        task = NexusPage(HDC_torrents_class_name)   ## The site would inform you that you have loged in this site when you run Page() at the very beginning.
        task = NexusPage(HDC_torrents_class_name)   ## So just run this command again to make sure that you can get the informations of torrents page.
        task_list = task.find_free(free_tag,free_tag2)
        download_encrypted_free(torrents_amount, task_list, monitor_path, HDC_download_class_name)
#####
else:
    task = GazellePage(DIC_torrents_class_name)   ## The site would inform you that you have loged in this site when you run Page() at the very beginning.
    task = GazellePage(DIC_torrents_class_name)   ## So just run this command again to make sure that you can get the informations of torrents page.
    task_list = task.find_free(DIC_free_tag)
    download_free(torrents_amount, task_list, monitor_path)
