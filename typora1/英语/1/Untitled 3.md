# 用sqlite是出现Unable to prepare statement: 1警告

![img](Untitled%203.assets/original.png)

[Yang TY](https://blog.csdn.net/hnyzyty)![img](Untitled%203.assets/newCurrentTime2.png)于 2015-07-13 11:07:50 发布![img](Untitled%203.assets/articleReadEyes2.png)2436![img](Untitled%203.assets/tobarCollect2.png) 收藏

分类专栏： [SQL](https://blog.csdn.net/hnyzyty/category_5643245.html) 文章标签： [个人笔记](https://so.csdn.net/so/search/s.do?q=个人笔记&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[![img](Untitled%203.assets/resize,m_fixed,h_64,w_64.png)SQL专栏收录该内容](https://blog.csdn.net/hnyzyty/category_5643245.html)

1 篇文章0 订阅

订阅专栏

![img](Untitled%203.assets/20150713110253402.png)

今天编总成件数据库时，出现的这个问题。

问题原因——有一条查询语句写错了：

select * from subparts_detail 