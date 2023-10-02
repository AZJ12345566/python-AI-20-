# python-AI-20-
python+AI笔记(20)
# 一阶-十一章-全国疫情地图构建
import json
from pyecharts import Map
from pyecharts.options import *
#读取数据文件
f = open("D:/疫情.txt","r",encoding = "UTF-8")
data = f.read()
#关闭文件
f.close()
#取到各省数据
#将字符串json转换为python字典
data_dict = json.loads(data)
#从字典中取出省份的数据
province_data_list = data_dict["areaTree"][0]["children"]

#组装每个确诊人数为元组，并各个省的数据都封装入列表内
data_list = []  #绘图需要用的数据
for province_data in province_data_list:
    province_name = province_data["name"]  #省份名称
    province_confirm = province_data["total"]["confirm"]。#确诊人数
    data_list.append(province_name,province_confirm)
print(data_list)

#构建地图对象
map = Map()

#添加数据
map.add("各省份确诊人数"，data_list,"china")
#设置全局配置,定制分段的视觉映射
map.set_global_opts(
    title_opts = TitleOpts(title = "全国疫情地图"),
    visualmap_opts = VisualMapOpts(
        is_show = True,  #是否显示
        is_pieceswise = True,  #是否分段
        pieces=[
            {"min":1,"max":99,"label":"1-99人","color":"#CCFFFF"},
            {"min":100,"max":990,"label":"100-999人","color":"#FFFF99"},
            {"min":1000,"max":4999,"label":"1000-4999人","color":"#FF9966"},
            {"min":5000,"max":9999,"label":"5000-9999人","color":"#FF6666"},
            {"min":10000,"max":99999,"label":"10000-99999人","color":"#CC3333"},
            {"min":100000,"label":"100000+","color":"#990033"}
        ]
    )
)

map.render("全国疫情地图.html")

# 一阶-十一章-河南省疫情地图绘制
import json
from pyecharts.charts import Map
from pyecharts.options import *
#读取文件
open("D:/疫情.txt","r",encoding="UTF-8")
data = f.read()
f.close()
#获取河南省数据
#现将json数据转为字典
data_dict = json.loads(data)
#取到河南省数据
cities_data = data_dict["areaTree"][0]["children"][3]["children"]

#准备数据为元组并放入list
data_list = []
for city_data in cities_data:
    city_name = city_data["name"] + "市"
    city_confirm = city_data["total"]["confirm"]
    data_list.append((city_name,city_confirm))

#手动添加济源市的数据
data_list.append(("济源市",5))

#构建地图
map = Map()
map.add("河南省疫情发布",data_list,"河南")

#设置全局变量
map.set_global_opts(
    title_opts = TitleOpts(title = "河南省疫情地图"),
    visualmap_opts = VisualMapOpts(
        is_show = True,  #是否显示
        is_pieceswise = True,  #是否分段
        pieces=[
            {"min":1,"max":99,"label":"1-99人","color":"#CCFFFF"},
            {"min":100,"max":990,"label":"100-999人","color":"#FFFF99"},
            {"min":1000,"max":4999,"label":"1000-4999人","color":"#FF9966"},
            {"min":5000,"max":9999,"label":"5000-9999人","color":"#FF6666"},
            {"min":10000,"max":99999,"label":"10000-99999人","color":"#CC3333"},
            {"min":100000,"label":"100000+","color":"#990033"}
        ]
    )
)

map.render("河南省疫情地图.html")

# 一阶-十一章-基础柱状图构建
#导包
from pyecharts.charts import Bar
from pyecharts.options import LabelOpts
#使用Bar构建基础柱状图
bar = Bar()
#添加x轴的数据
bar.add_xaxis(["中国","美国","英国"])
#添加y轴数据
bar.add_yaxis("GDP",[30,20,10],label_opts = LabelOpts(position = "right"))

#反转x轴和y轴
bar.reversal_axis()

#绘图
bar.render("基础柱状图.html")

# 一阶-十一章-基础时间线柱状图绘制
from pyecharts.charts import Bar,Timeline
from pyecharts.options import LabelOpts
from pyecharts.globals import ThemeType

bar1 = Bar()
bar1.add_xaxis(["中国","美国","英国"])
bar1.add_yaxis("GDP",[30,30,20],label_opts = LabelOpts(position = "right"))
bar1.reversal_axis()

bar2 = Bar()
bar2.add_xaxis(["中国","美国","英国"])
bar2.add_yaxis("GDP",[50,50,50],label_opts = LabelOpts(position = "right"))
bar2.reversal_axis()

bar3 = Bar()
bar3.add_xaxis(["中国","美国","英国"])
bar3.add_yaxis("GDP",[70,60,60],label_opts = LabelOpts(position = "right"))
bar3.reversal_axis()

#构建时间线对象
timeline = Timeline({"theme":ThemeType.LIGHT})  #这里是给timeline构建一个主题，主题类型就是切换时间线的颜色
#在时间线内添加柱状图对象
timeline.add(bar1,"点1")
timeline.add(bar2,"点2")
timeline.add(bar3,"点3")

#绘图是用时间线对象绘图，而不是bar对象了
timeline.render("基础时间线柱状图.html")

#设置自动播放
timeline.add_schema(
    play_interval = 1000,
    is_timeline_show = True,
    is_auto_play = True,
    is_loop_play = true
)

timeline.render("基础时间线柱状图.html")

