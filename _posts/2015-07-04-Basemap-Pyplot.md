---
layout: post
title: 使用Basemap绘制中国地图
categories: [Coder]
tags: [Pythonic, linux]
---
<h2>{{ page.title }}</h2>

Intro
===

>Basemap能做啥，pyplot能做啥，大家可以自行Google,现在直接拿出今天我们要实现的最终效果,
>把你自己的经纬度锚到地图上。

![img1](https://41.media.tumblr.com/7c35a92a61a356be8480d00b0b3c6f86/tumblr_nqxqeexwAq1r68ev5o1_1280.png)

###1. 安装Basemap的依赖

作为Matplotlib的一个子包，使用之前你必需要安装

* matplotlib
* numpy
* pandas
* Python27+ 33+
* [GEOS](http://trac.osgeo.org/geos/) 这个可以先不安装,Basemap中带了3.3的。

###2. 从SF上下载Basemap的包

Basemap源码包在[here](http://sourceforge.net/projects/matplotlib/files/matplotlib-toolkits/)


###3. 解压后，会找到GEO3.3的安装包，先编译安装GEOS


{% highlight bash %}
tar -xf basemap-1.0.7.tar.gz
cd basemap-1.0.7
cd geos-3.3.3
export GEOS_DIR=/opt/geos  # 指到自己要安装的目录 
./configure --prefix=$GEOS_DIR
make; make install
cd ../
python setup.py install # 记得要选对python版本

>>> from mpl_toolkits.basemap import Basemap  # 打开python prompt 验证成功 

{% endhighlight bash %}

###4. 开始绘图了

{% highlight python %}
from mpl_toolkits.basemap import Basemap
from geolite2 import geolite2
import matplotlib.pyplot as plt
from flask import Flask, send_file
from io import BytesIO
from sqlalchemy import create_engine, MetaData
app = Flask(__name__)

engine = create_engine('postgres://dbuser:dbpass@172.16.160.111/weblog')
metadata = MetaData(bind=engine)


def plot(image):
    import hashlib
    import pickle
    read = geolite2.reader()
    ress = list(engine.execute('select "raddr" from weblog\
    where "Updated" > CURRENT_TIMESTAMP - INTERVAL \'30 days\''))
    print(len(ress))
    iplist = []
    ipset = set()
    for i in ress:
        if i[0] not in ipset:
            iplist.append(i[0])
            ipset.add(i[0])
    print(len(iplist))
    loclist = []
    locsset = set()
    for i in iplist:
        try:
            loc = read.get(i)['location']
        except:
            pass
        hashloc = hashlib.md5(pickle.dumps(loc)).hexdigest()
        if hashloc not in locsset:
            loclist.append(loc)
            locsset.add(hashloc)
    plt.figure(figsize=(10, 10))
    map = Basemap(
        projection='merc', llcrnrlon=70, llcrnrlat=15,
        urcrnrlon=140, urcrnrlat=55, lat_0=15, lon_0=95, resolution='l')
    map.drawcoastlines()
    map.drawcountries()
    map.fillcontinents(color='coral')
    map.drawmapboundary()
    print(len(loclist))
    for loc in loclist:
        x, y = map(loc.get('longitude'), loc.get('latitude'))
        map.plot(x, y, 'bo', markersize=4)
    plt.savefig(image, format='png')


@app.route('/image.png')
def image_png():
    image = BytesIO()
    plot(image)
    image.seek(0)
    return send_file(image,
                     attachment_filename="image.png",
                     as_attachment=True)


@app.route('/')
def index():
    return '<img src="image.png">'


app.run(debug=True)
# geolite2.close()

{% endhighlight python %}

> 数据自己造，我用了Flask来做展示，


###Reference: 
[install](http://matplotlib.org/basemap/users/installing.html)
