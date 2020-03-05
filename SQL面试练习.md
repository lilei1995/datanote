## 考拉运营"小明"负责多个品牌的销售业绩，请完成：

（1）请统计小明负责的各个品牌，在2017年销售最高的3天，及对应的销售额。

销售表 a:

字段：logday（日期，主键组），SKU_ID（商品SKU，主键组），sale_amt(销售额)

商品基础信息表 b:

字段：SKU_ID（商品SKU，主键）,bu_name（商品类目），brand_name(品牌名称)，user_name（运营负责人名称）

（2）请统计小明负责的各个品牌，在2017年连续3天增长超过50%的日期，及对应的销售额。

 

```
(1)
select b.brand_name,a.logday,sum(sale_amt) as sale
from a,b
where a.SKU_ID=b.SKU_ID and user_name='小明' and logday between '2017-01-01' and '2017-12-31' 
group by brand_name,logday having( 
select count(logday) from 
( 
select b1.brand_name,a1.logday,sum(sale_amt) as sale1
from a a1,b b1
where a1.SKU_ID=b1.SKU_ID and b1.user_name='小明' and a1.logday between '2017-01-01' and '2017-12-31'
group by b1.brand_name,a1.logday 
) t 
where b.brand_name=b1.brand_name and sale1>sale 
)<3 

(2)第二问比较复杂，所以要创建几个视图
create view t_view as 
select brand_name,logday,sum(saleamt) as sale
from a,b
where a.SKU_ID=b.SKU_ID and user_name='小明' and logday in ('2017-01-01','2017-12-31')
group by brand_name,logday; 
create view f_view as 
select t1.brand_name,t1.logday,(t1.sale/t2.sale-1) as growth,t1.sale
from t_view t1,t_view t2
where t1.logday-t2.logday=1; 
select distinct f1.brand_name,f1.logday,f1.sale 
from f_view f1,f_view f2,f_view f3
where
f1.brand_name=f2.brand_name
and f2.brand_name=f3.brand_name
((f1.logday-f2.logday=-1
and
f1.logday-f3.logday=-2)
or
(f1.logday-f2.logday=1
and
f1.logday-f3.logday=-1)
or
(f1.logday-f2.logday=1
and
f1.logday-f3.logday=2))
and f1.growth>0.5 and f2.growth>0.5
and f3.growth>0.5;
```

# **好评率**

是会员对平台评价的重要指标。现在需要统计2018年1月1日到2018年1月31日，用户'小明'提交的母婴类目"花王"品牌的好评率（好评率=“好评”评价量/总评价量）:

用户评价详情表：a

字段：id（评价id，主键），create_time（评价创建时间，格式'2017-01-01'）， user_name(用户名称)，goods_id(商品id，外键) ，sub_time（评价提交时间，格式'2017-01-01 23:10:32'），sat_name（好评率类型，包含：“好评”、“中评”、“差评”）

商品详情表：b

字段：good_id（商品id，主键），bu_name（商品类目）, brand_name(品牌名称)

```
SELECT SUM(CASE

        WHEN sat_time = '好评' THEN 1

          ELSE 0

        END)/COUNT(sat_time) AS "好评率"

FROM a JOIN b ON a.good_id = b.good_id

WHERE user_name = '小明' AND sub_time BETWEEN ('2017-1-1') AND ('2017-1-31')

   AND bu_name ='母婴' AND brand_name = '花王';
```





# 有两家单车公司，表里存放的是每天某个时刻投放在某个城市的单车量，尝试寻找按天颗粒度这个城市两家公司投放单车量的拐点（即相等值）时间。

 









# 两张表，用户订单表（tbl_ordr）及用户商品点击明细表（tbl_clk），假设都仅有只有某一天的数据，请根据以下描述写出对应的sql代码 

1).用户在点击某个商品之后产生的订单算作这次点击产生的订单（要求点击及创建订单行为是同一用户操作的，且点击的商品和订单商品是同一商品）

2).如果同一用户多次点击相同商品，并最终产生订单，则订单归属到订单创建前的最后一次点击上 

3).输出有产生订单的商品点击及点击产生的订单号（clk_id，ordr_id）（用sql实现） 

用户订单表字段为：用户id，订单号id（ordr_id），订单商品id，order_time预订时间 

商品点击明细表字段为：点击id（clk_id），用户id，clk_time点击时间，点击的商品id 

思路是首先根据用户id和商品id将用户订单表左连接商品点击明细表，然后group by 字段用户id，商品id求max(clk_time点击时间)对应的clk_id 

这时面试官很亲切的提醒了一句：如果有个客户点击了某个商品点击了5次，但他在第3次点击后就购买了该商品，那我这样取数会把点击id错取为第5次，这时我赶紧反应过来说在left join之后加一层where判断，where clk_time点击时间<order_time预订时间，面试官说OK就过了。 

 

# sql题每天第一篇被点击的笔记类型分布，和每天所有被点击笔记的类型分布。解释指标有什么意义

 

# （1）有一张user_score表，包含每个人的三科成绩，数据样例如下： 

Name         subject         score 

张三            语文                82 

张三            数学                90 

张三            外语                75 

李四            语文                58 

…….. 

1）请写出一段SQL，统计出每个人所有学科中的最高分数 

2）请写出一段SQL，统计每个人分数最高的两门学科" 

 

# 以Python为例，已知列表为：capital = ["A", "B", "E", "A", "T", "Q", "B"]，请统计并打印出以上数据集合中每个字母出现的次数，要求在R或者Python语言中任选其一。 （R中，capital = list("A", "B", "E", "A", "T", "Q", "B") ）

 

# user_id,打车开始时间，打车结束时间，查询连续打车时间小于5分钟的user_id.