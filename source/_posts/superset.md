---
title: superset安装
date: 2019-11-05 18:08:46
tags:
---
## win10本地安装
* python安装
 <!-- more -->   
* pip升级
    ```shell
    python -m pip install --upgrade pip
    ```

    报错：

    ![报错](/img/posts/supertset_pip.png)

   解决方案：
    ```
    easy_install -U pip 
    或 
    python -m pip install -U --force-reinstall pip
    ```
 

 
* superset安装
    ```
    pip install superset
    ```
  1. flask报错
     ![报错](/img/posts/superset_flask.png)
     解决方案：
     ```
     #指定版本安装
     pip install flask-jwt-extended==3.20.0
     pip install pandas==0.23.1
     pip install  flask==0.12.2
     ```
  2. _geohash报错 
     ![报错](/img/posts/superset_geohash.png)
     解决方案：
     ```
     https://www.lfd.uci.edu/~gohlke/pythonlibs/ 下载包，手动安装
     pip install python_geohash-0.8.5-cp36-cp36m-win_amd64.whl
     ```
* superset db 升级: superset db upgrade

    ![报错](/img/posts/superset_db.png)
    
    解决：

        ```
         python venv\Scripts\superset db upgrade
        ```
    > ### 后面的superset命令 都需要 python venv\Scripts\superset 替代！！！

* 安装mysql时: pip install mysqlclient   

    ![报错](/img/posts/superset_mysql.png)
    ```
    #手动下载安装
    mysqlclient‑1.4.4‑cp36‑cp36m‑win_amd64.whl
    ```


## linux本地安装
```
###参考：https://superset.incubator.apache.org/installation.html#superset-installation-and-initialization
1.基本依赖
sudo yum upgrade python-setuptools
sudo yum install gcc gcc-c++ libffi-devel python-devel python-pip python-wheel openssl-devel libsasl2-devel openldap-devel


2.安装python3.6.8
cd /home/roy/src/
wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tgz
tar -xzf Python-3.6.8.tgz
cd Python-3.6.8.tgz
mkdir -p /home/roy/software/python3
# 编译安装
./configure --prefix="/home/roy/software/python3"
make
make install        


##添加环境变量：注意python3和 pip3都在bin目录下
vim ~/.bash_profile
PATH=$PATH:$HOME/bin:/home/roy/software/python3/bin;


2.官方推荐使用 Python virtualenv,执行安装：
cd /home/roy/software
pip3 install virtualenv
python3 -m venv venv
. venv/bin/activate


pip3 install --upgrade setuptools pip
##以下2步问题会比较多，请先跳过看  第3点
pip3 install superset
pip3 install mysqlclient
superset db upgrade


export FLASK_APP=superset
flask fab create-admin
superset load_examples
superset init
superset run -p 8080 --with-threads --reload --debugger &




3.重点说明：
安装superset版本为 v0.28.1，默认安装过程出现最多的问题就是各种依赖库版本不对。请具体查看错误信息
主要会出现几个版本问题请核对：
flask=0.12.2
pandas=0.23.1
sqlalchemy=1.2.2
3.1.版本问题，可pip3 install  pandas==0.23.1 指定版本安装
3.2.还有些基础python的依赖，可能需要使用yum装好后，重新编译安装一次python了
3.3.其他问题 请自行google，目前碰到的少
############superset v0.28.1 requires #####################
alembic==1.0.0            # via flask-migrate
amqp==2.3.2               # via kombu
asn1crypto==0.24.0        # via cryptography
babel==2.6.0              # via flask-babel, flower
billiard==3.5.0.4         # via celery
bleach==2.1.2
boto3==1.4.7
botocore==1.7.48
cchardet==1.1.3           # via tabulator
celery==4.2.0
certifi==2018.8.24        # via requests
cffi==1.11.5              # via cryptography
chardet==3.0.4            # via requests
click==6.7                # via flask, flask-appbuilder, tableschema, tabulator
colorama==0.3.9
contextlib2==0.5.5
cryptography==1.9
defusedxml==0.5.0         # via python3-openid
docutils==0.14            # via botocore
et-xmlfile==1.0.1         # via openpyxl
flask-appbuilder==1.12.0
flask-babel==0.11.1       # via flask-appbuilder
flask-caching==1.4.0
flask-compress==1.4.0
flask-login==0.4.1        # via flask-appbuilder
flask-migrate==2.1.1
flask-openid==1.2.5       # via flask-appbuilder
flask-sqlalchemy==2.1     # via flask-appbuilder, flask-migrate
flask-wtf==0.14.2
flask==0.12.2
flower==0.9.2
future==0.16.0
futures==3.1.1            # via flower
geopy==1.11.0
gunicorn==19.8.0
html5lib==1.0.1           # via bleach
humanize==0.5.1
idna==2.6
ijson==2.3                # via tabulator
isodate==0.6.0
itsdangerous==0.24        # via flask
jdcal==1.4                # via openpyxl
jinja2==2.10              # via flask, flask-babel
jmespath==0.9.3           # via boto3, botocore
jsonlines==1.2.0          # via tabulator
jsonschema==2.6.0         # via tableschema
kombu==4.2.1              # via celery
linear-tsv==1.1.0         # via tabulator
mako==1.0.7               # via alembic
markdown==3.0
markupsafe==1.0           # via jinja2, mako
numpy==1.15.2             # via pandas
openpyxl==2.4.11          # via tabulator
pandas==0.23.1
parsedatetime==2.0.0
pathlib2==2.3.0
polyline==1.3.2
pycparser==2.19           # via cffi
pydruid==0.4.4
pyhive==0.5.1
python-dateutil==2.6.1
python-editor==1.0.3      # via alembic
python-geohash==0.8.5
python3-openid==3.1.0     # via flask-openid
pytz==2018.5              # via babel, celery, flower, pandas
pyyaml==3.12
requests==2.18.4
rfc3986==1.1.0            # via tableschema
s3transfer==0.1.13        # via boto3
sasl==0.2.1               # via thrift-sasl
simplejson==3.15.0
sqlalchemy-utils==0.32.21
sqlalchemy==1.2.2
sqlparse==0.2.4
tableschema==1.1.0
tabulator==1.15.0         # via tableschema
thrift-sasl==0.3.0
thrift==0.11.0
tornado==5.1.1            # via flower
unicodecsv==0.14.1
unidecode==1.0.22
urllib3==1.22             # via requests
vine==1.1.4               # via amqp
webencodings==0.5.1       # via html5lib
werkzeug==0.14.1          # via flask
wtforms==2.2.1            # via flask-wtf
xlrd==1.1.0  
```






            