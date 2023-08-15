# SQL from Leecode
176.第二高-->limit 1,1;
177.第n高：ifnull（，null）；
178.分数排名：DENSE_RANK() OVER 分区 AS 名称；
180.连续数字：自连接
181.组内查重：group by+count（）；
184.查询各组最大值：外连接，in，group by；
185.额外命名表的时候千万不要用相同字母的不同大小写，因为sql对大小写不敏感！！！
186.最大三个：超过它的不超过三个
196.删除：如用表别名，delete后要加别名。
``` SQL
delete p1 from Person p1,person p2 where P1.email=P2.email and p1.id>p2.id;
```
更优：
```SQL
delete from Person where Id not in (select Id from (select min(Id) as Id 
         from Person group by Emal) p);
```
262.函数-->保留小数：ROUND（expr，n）；判断赋值：IF（expr，，）；
511.group by的优先级在 order by之前，不能利用分组之后的结果排序（可用having）；
601.# 临时表；连续三个满足条件；方法一：自连接的排列组合；方法二：辅助列
```SQL
WITH diff_group AS (
    SELECT id
            ,visit_date
            ,people
            ,id - row_number() OVER(ORDER BY id) AS diff
    FROM Stadium
    WHERE people >= 100
)
SELECT id
        ,visit_date
        ,people 
FROM diff_group
WHERE diff IN (SELECT diff FROM diff_group  GROUP BY diff HAVING count(id) >= 3)
```


# SQL from NIU
关于执行顺序问题：
![turn] [turn.png]

22.每个学校答过题的用户平均答题数目：避免出现只有一张表中有的数据-->inner join；一个用户可能会答好多题目，计算平均统计数目-->distinct去重；
25.满足条件1或条件2 不去重-->union all
26.判断
```SQL
SELECT  IF(age>=25,'25岁及以上','25岁以下') AS age_cut, 
COUNT(device_id) AS number FROM user_profile GROUP BY age_cut;
```
27.多条件判断
```SQL
select device_id,gender,
case
    when age<20 then '20岁以下'
    when age<25 then '20-24岁'
    when age>=25 then '25岁及以上'
    else '其他'
end age_cut
from user_profile;
```
28.次日留存率：利用subdate后拨一天
```SQL
SELECT COUNT(b.device_id)/ COUNT(a.device_id)
FROM(
    SELECT DISTINCT device_id,date
    FROM question_practice_detail ) as a
LEFT JOIN (
    SELECT DISTINCT device_id,DATE_SUB(date,interval 1 day) as date
    FROM question_practice_detail ) as b
ON a.date=b.date AND a.device_id=b.device_id
```

30.文本处理函数：
>1、LOCATE(substr , str )：返回子串 substr 在字符串 str 中第一次出现的位置，如果字符substr在字符串str中不存在，则返回0；
>2、POSITION(substr  IN str )：返回子串 substr 在字符串 str 中第一次出现的位置，如果字符substr在字符串str中不存在，与LOCATE函数作用相同；
>3、LEFT(str, length)：从左边开始截取str，length是截取的长度；
>4、RIGHT(str, length)：从右边开始截取str，length是截取的长度；
>5、SUBSTRING_INDEX(str  ,substr  ,n)：返回字符substr在str中第n次出现位置之前的字符串;
>6、SUBSTRING(str  ,n ,m)：返回字符串str从第n个字符截取到第m个字符；
>7、REPLACE(str, n, m)：将字符串str中的n字符替换成m字符；
>8、LENGTH(str)：计算字符串str的长度。

32.count（）需要和group by一起使用
33.局域最小：
```SQL
SELECT
    device_id,
    university,
    gpa
FROM user_profile u
WHERE gpa = 
    (SELECT MIN(gpa)
     FROM user_profile
     WHERE university = u.university)
ORDER BY university
```
34.
```SQL
SELECT
    u.device_id,
    u.university,
    COUNT(q.question_id) AS question_cnt,
    SUM(if(result = 'right' ,1,0)) AS right_question_cnt
FROM user_profile u
LEFT JOIN question_practice_detail q
USING(device_id)
WHERE university = '复旦大学' AND (MONTH(date) = 8 or MONTH(date) IS NULL)
GROUP BY device_id;
```
***如果没有group by，聚合函数只返回一个值
6.数字替代列名排序

7.更新问题：insert into name_ values()空值不空，用null代替
replace into name_ values()不管插入值有没有都可以
完播率：注意时长大于1的情况
删除：
 
添加：
 
索引：
 
累计涨粉：（sum...over...）
```SQL
select t2.author author, date_format(t1.start_time,'%Y-%m') month,
round(avg(case when t1.if_follow = 1 then 1
         when t1.if_follow = 2 then -1
         else 0 end),3) fans_growth_rate,
sum(sum(case when t1.if_follow = 1 then 1
         when t1.if_follow = 2 then -1
         else 0 end) ) 
over (partition by t2.author order by date_format(t1.start_time,'%Y-%m')) fans_total
from tb_user_video_log t1 
inner join tb_video_info t2 using(video_id)
where year(t1.start_time)=2021 and year(t1.end_time)=2021
group by t2.author,month
order by t2.author,fans_total;
```

累计n日播放量：
```SQL
select *
from
(select tag, dt,
sum(add_likes)over(partition by tag order by dt rows 6 preceding) sum_like_cnt_7d,
max(add_retweets)over(partition by tag order by dt rows 6 preceding) max_retweet_cnt_7d
from (select tag,
     date_format(start_time, '%Y-%m-%d') dt,
     sum(if_like) add_likes,
     sum(if_retweet) add_retweets
     from tb_user_video_log t1
     join tb_video_info t2
     on t1.video_id=t2.video_id
     group by tag, dt
     ) a
) b
where dt between '2021-10-01' and '2021-10-03'
order by tag desc, dt
```

难题：先求一级指标，再求二级指标：例如
```SQL
select tem.video_id,
round((100*tem.play_rate+5*tem.like_cnt+3*tem.comment_cnt+2*tem.retweet)/(tem.new+1),0) as hot_index
from ( select t1.video_id ,
avg( if(timestampdiff(second,t1.start_time,t1.end_time)>=t2.duration, 1,0) ) as play_rate,
sum(t1.if_like) as like_cnt, 
count(t1.comment_id) as comment_cnt,
sum(t1.if_retweet) as retweet,
datediff( (select max(end_time) from tb_user_video_log) ,max(end_time)) as new 
from tb_user_video_log t1 inner join tb_video_info t2 using(video_id) 
where datediff( (select max(end_time) from tb_user_video_log),t2.release_time)<=29
group by video_id) as tem
order by hot_index desc
limit 3;
```

**注意avg在要求id不重复的时候不能用，得手写
***双指标同时去重
with rollup
order by的执行顺序会在union后还在体现，所以不如在union后排序
新用户次日留存率：初次活跃并全部登录天数，查找是否可以容错一天（这个也是连接条件之一）
```sql
select 
    a.dt,
    ifnull(round(count(distinct b.uid)/count(a.uid),2),0)uv_left_rate
from
    (SELECT uid,date(min(in_time)) dt
    FROM tb_user_log
    GROUP BY uid) a 
left join
    (SELECT uid,DATE(in_time) dt
    FROM tb_user_log
    union
    SELECT uid,DATE(out_time) dt
    FROM tb_user_log) b # 跨天情况消除
ON a.uid = b.uid
and b.dt = DATE_ADD(a.dt,INTERVAL 1 DAY)
WHERE a.dt like "2021-11%"
GROUP BY a.dt
ORDER BY a.dt;
```
**四表查询
单日去重**
```SQL
select  distinct answer_date,author_id from answer_tb order by author_id,answer_date

```

+ 连续签到
```sql
# 第一步：先过滤掉不签到的，重复的
# 第二步： ranK()over(partition by uid order by dt) --按照签名排序 
# 第三步：date_sub(dt,interval ranK()over(partition by uid order by dt) 
#          按照签到日期减去签到排名的差值 排序，（如果是连续签到，则得到的日期相同）-- 即得到连续签到的天数，rank_day
# 第四步：rank()over(partition by uid,rank_day order by dt)，按照连续签的天数到排序，在mod与7，即可以判断连续签到的天数是否可以达到额外加金币的条件

select
             
    uid,
    date_format(dt,'%Y%m') month,
    sum(coin) coin
from (
    select
        uid,
        dt,
        case when mod(rank()over(partition by uid,rank_day order by dt), 7)=0 then 7
             when mod(rank()over(partition by uid,rank_day order by dt), 7)=3 then 3
             else 1
             end  coin 
             --   按照rank_day排序的排名，与7求mod，
    from(
        select
            uid,
            dt,
             date_sub(dt,interval ranK()over(partition by uid order by dt) day ) rank_day--  签到日期减去签到排名，（如果是连续签到，则得到的日期相同），
        from (
            select
                distinct uid,  -- 去重 过滤掉重复签到的情况
                date(in_time) dt-- 登入日期 
            from tb_user_log
            where artical_id=0 and sign_in=1
                and date(in_time)  between '2021-07-07' and '2021-10-31'
            ) t1  
          ) t2
     ) t3
group by uid,month
order by  month,uid
```
+ 复购率问题：求有第二次重复的
```SQL
select a.product_id product_id, round(count(distinct a.uid_) / count(distinct a.uid),3) repurchase_rate
from 
    (select  c.product_id product_id, a.uid uid, 
             if(count(*)over(partition by c.product_id,a.uid) >=2, a.uid, null) uid_
    from tb_order_overall a  join  tb_order_detail b join tb_product_info c
    on a.order_id = b.order_id and b.product_id = c.product_id
    where c.tag = '零食' 
    and timestampdiff(day,date(a.event_time),(select date(max(event_time)) from tb_order_overall)) 
    between 0 and 89) a
group by a.product_id
order by repurchase_rate desc, product_id
limit 3;
```
+ 新客名单
```SQL
select

    round(avg(price_1), 1) as avg_amount,

    round(avg(price_0-price_1), 1) as avg_cost

from (

select uid,

    sum(price*cnt) as price_0,

    max(total_amount) as price_1

from (

select uid, a.order_id, event_time,

    a.product_id, price, cnt, total_amount,

    dense_rank() over(partition by uid order by event_time) as rnk

from tb_order_detail a left join tb_order_overall b

on a.order_id = b.order_id

where status = 1

    ) t1

where rnk = 1

and date_format(event_time, '%Y-%m') = '2021-10'

group by uid

        ) t2
```
+ 动销率
```SQL
select
  dt1,
  round(
    count(
      distinct if(timestampdiff(day, dt, dt1) between 0 and 6,
        tb1.product_id, null)
    ) / count(
      distinct if(dt1 >= date(release_time), tb3.product_id, null)
    ),3
  ) sale_rate,
  1 - round(
    count(
      distinct if(
        timestampdiff(day, dt, dt1) between 0 and 6,
        tb1.product_id,
        null
      )
    ) / count(
      distinct if(dt1 >= date(release_time), tb3.product_id, null)
    ),3
  ) unsale_rate
from
  (
    select
      date(event_time) dt1
    from
      tb_order_overall
    having
      dt1 between '2021-10-01' and '2021-10-03'
  ) tb2,
  (
    select

      b.product_id,

      date(event_time) dt

    from

      tb_order_overall a

      left join tb_order_detail b on a.order_id = b.order_id

      left join tb_product_info c on b.product_id = c.product_id

    where

      shop_id = 901

  ) tb1

  left join tb_product_info tb3 on tb1.product_id = tb3.product_id

where

  shop_id = 901

group by

  dt1
```
+ 连续两天登录
```SQL {.line-numbers}
select user_id,cnt as days_count
from
(select user_id,days_group,count(*)as cnt
from 
(
select *,date_sub(sales_date,interval rk DAY)as days_group 
from 
(select *,
        ROW_NUMBER()over(partition by user_id order by sales_date)as rk
from 
(select 
         distinct sales_date,user_id  from sales_tb
    )a
  )b
)c
group by user_id,days_group
)d
where cnt>=2
order by user_id
```
+ 最大同时在线人数
```SQL
select course_id,course_name,user_id,in_datetime as dt,1 as flag
from attend_tb a join course_tb b using(course_id)
union
select course_id,course_name,user_id,out_datetime as dt,-1 as flag 
from attend_tb a
join course_tb b using(course_id)

```