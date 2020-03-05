## 阿里云的DataV的简单应用

## 1，登陆DataV账户

## 2，新建可视化应用

## 3，修改组件样式

​      

## 4，修改数据

####     DataV 支持以下几类数据源的接入。

- [数据库类](https://help.aliyun.com/document_detail/53844.html?spm=a2c4g.11186623.2.16.fa7d63d1cAfrVy#section-lgm-lkp-p2b)
- [文件类](https://help.aliyun.com/document_detail/53844.html?spm=a2c4g.11186623.2.16.fa7d63d1cAfrVy#section-dhc-bbq-p2b)
- [API类](https://help.aliyun.com/document_detail/53844.html?spm=a2c4g.11186623.2.16.fa7d63d1cAfrVy#section-vts-lbq-p2b)
- [其他](https://help.aliyun.com/document_detail/53844.html?spm=a2c4g.11186623.2.16.fa7d63d1cAfrVy#section-myj-l2q-p2b)

####      DataV支持以下几种数据库作为数据源。

- [Analytic DB](https://help.aliyun.com/document_detail/59715.html#concept-jmg-1lp-p2b)
- [RDS for MySQL](https://help.aliyun.com/document_detail/59725.html#concept-xdt-blp-p2b)
- [RDS for PostgreSQL](https://help.aliyun.com/document_detail/59742.html#concept-dnv-clp-p2b)
- [RDS for SQLServer](https://help.aliyun.com/document_detail/59750.html#concept-e5r-2lp-p2b)
- [AnalyticDB for PostgreSQL](https://help.aliyun.com/document_detail/59764.html#concept-ylx-hlp-p2b)
- [TableStore](https://help.aliyun.com/document_detail/97683.html#concept-e42-c5h-wfb)
- [Oracle](https://help.aliyun.com/document_detail/59771.html#concept-bmw-3lp-p2b)
- [兼容MySQL数据库](https://help.aliyun.com/document_detail/59772.html#concept-vt5-flp-p2b)
- [对象存储OSS](https://help.aliyun.com/document_detail/97501.html#concept-bgl-qcv-vfb)



如果您在其他地域，或者没有使用阿里云数据库，想连接自建数据库，那就需要暴露数据库的公网IP进行连接。DataV当前不支持IP白名单，如果您担心安全性问题，可以使用阿里云提供的[数据库连接代理工具](https://help.aliyun.com/document_detail/53844.html?spm=a2c4g.11186623.2.16.fa7d63d1cAfrVy#section-myj-l2q-p2b)来连接。

**本文档为您介绍在DataV中添加AnalyticDB for MySQL数据源的方法。**

##### 操作步骤

1. 登录[DataV控制台](http://datav.aliyun.com/)。
2. 选择**我的数据 > 添加数据**。
3. 单击**类型**下拉菜单，选择**AnalyticDB for MySQL**。
4. 填写数据库信息。

![1569548274584](C:\Users\83759\AppData\Roaming\Typora\typora-user-images\1569548274584.png)

![1569548286478](C:\Users\83759\AppData\Roaming\Typora\typora-user-images\1569548286478.png)



#### DataV支持以下两种文件类的数据源。

- [CSV文件](https://help.aliyun.com/document_detail/59775.html#concept-tdw-llp-p2b)
- [静态JSON](https://help.aliyun.com/document_detail/59776.html#concept-lz2-jbq-p2b)

DataV目前不支持从其他文件存储中读取大型的数据文件。

#### DataV支持以下种类的API接口作为数据源。

- [POP API](https://help.aliyun.com/document_detail/97927.html#concept-cx4-4rw-wfb)

- 阿里云API网关

  您可以在配置页面的**数据**面板中直接粘贴API地址。如果您的API有鉴权，需要在[阿里云API网关](https://cn.aliyun.com/product/apigateway/)中进行封装后，再通过阿里云API网关的配置来接入。

## 5，预览并发布大屏

https://help.aliyun.com/document_detail/87793.html?spm=a2c4g.11186623.6.550.53c51cd2FswGqZ

