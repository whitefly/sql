[老师学生成绩经典练习题](http://blog.csdn.net/qaz13177_58_/article/details/5575711/)
表结构：

```
course    cno 课程编号，cname 课程名称 tno 老师id号
score     sno学生id        cno 课程id        degree 成绩
student  sno学生id    sname 学生名字   ssex 学生性别   sbirthday 学生生日 class所属班级id
teacher  tno 老师id   tname 老师姓名 tsex 老师性别  tbirthday 老师生日   prof老师职位 depart所属院系
```


##23、查询“张旭“教师任课的学生成绩
```sql
select * from SCORE where cno in(
(select A.cno from COURSE A join TEACHER B on A.TNO=B.TNO 
 WHERE B.TNAME='张旭')
```
##24、查询选修某课程的同学人数多于5人的教师姓名。
```sql
select tname from TEACHER where tno in(
select A.tno from COURSE A join SCORE B on A.cno=B.cno 
group by A.tno having count(*)>5)
```
或者同时3表相连，不过这种方式我不太习惯。（还是偏向于一次只连2个表,然后在第三表中查询）。不过这种方式会比上面的快一点~why？
```sql
select t.tname,count(*) from TEACHER T join (COURSE C,SCORE S)on (C.cno=S.cno and C.TNO=T.tno)
group by t.tname having count(*)>5
```

##25、查询95033班和95031班全体学生的记录。
```sql
select * from STUDENT where class in ('95033','95031') order by class
```

##26、查询存在有85分以上成绩的课程Cno.
```sql
select cno from SCORE group by cno having max(degree)>85
```

##27、查询出“计算机系“教师所教课程的成绩表。
```sql
select A.* from SCORE A join (TEACHER B,COURSE C) on （A.cno=C.CNO and C.tno=b.tno）
where b.depart='计算机系'
```
##28、查询“计算机系”中，与“电子工程系“已有职称不同的职称的教师的Tname和Prof
```sql
select tname,PROF from TEACHER where DEPART='计算机系'
and PROF not in 
(select PROF from TEACHER where DEPART='电子工程系')
```

##29、查询选修编号为“3-105“课程且成绩至少高于一个选修编号为“3-245”的同学的Cno、Sno和Degree,并按Degree从高到低次序排序。
```sql
SELECT * FROM SCORE WHERE cno='3-105' and DEGREE>ANY(SELECT DEGREE FROM SCORE WHERE CNO='3-245') 
ORDER BY DEGREE DESC
```

其实我的第一反应是建立第一个自连接， 左边大于min（右边） 但是min（）为聚焦函数，自连接中无法使用。于是强行group by
于是强行group。出现了下面的sql。使用grouby的坏处就是不能随意显示select属性了
```sql
select s1.SNO,s1.DEGREE  from SCORE s1 join SCORE s2 on s1.CNO='3-105'and s2.cno='3-245'  --返回了笛卡尔积
group by s1.SNO,s1.DEGREE having s1.DEGREE>min(s2.DEGREE)   --强行groupby
ORDER BY DEGREE DESC
```

##30、查询选修编号为“3-105”且成绩高于所有选修编号为“3-245”课程的同学的Cno、Sno和Degree.
```sql
SELECT * FROM SCORE WHERE cno='3-105' and DEGREE>all(SELECT DEGREE FROM SCORE WHERE CNO='3-245')
```
##31、查询所有教师和同学的name、sex和birthday.
```sql
select tname as name,tsex as sex ,tbirthday as birthday from TEACHER
union
select sname as name,ssex as sex,sbirthday as birthday from STUDENT
```
union可以联合不同表中查询的字段.
前提：查询的字段的属性类型相同，名字可以相同也可以不同，但字段的前后顺序必须相同。不然报错

##32、查询所有“女”教师和“女”同学的name、sex和birthday.
```sql
select tname as name,tsex as sex ,tbirthday as birthday from TEACHER  where tsex='女'
union
select sname as name,ssex as sex,sbirthday as birthday from STUDENT where ssex='女'
```
union只是联合结果，子查询后where语句和和平时没区别，字段名不能使用别名

##33、查询成绩比该课程平均成绩低的同学的成绩表。
```sql
select s1.cno from SCORE s1 join SCORE s2 on s1.CNO =s2.cno
group by s1.cNO , s1.DEGREE having s1.DEGREE<avg(s2.DEGREE)
```
碰到这种属性相关联的，第一反应就是自连接。 
但是为了使用聚焦函数，只能使用group，但group后字段提取受到限制。还有另外一种实现类似自连接的效果
使用2个select where做连接，在显示子段中使用聚焦函数
```sql
select s1.* from SCORE s1 where s1.DEGREE <
(select avg(s2.degree) from SCORE s2 where s1.CNO and s2.cno)
```
总结：2种使用自连接的方式，
- join中on连接方式
- 子select中where连接方式

##34、查询所有任课教师的Tname和Depart.有些老师木有任课
想法：刚开始认为老师数量，course中老师数量和score中老师数据就递减的。后来发现
course中老师数量和score中老师数量是一样的
```sql
select tname ,depart from TEACHER where tno in(
select c.tno from SCORE s  join COURSE c on s.cno=c.cno)
```
所以在course与teacher表做等价连接时，就相当于取了交集。得到了最少数量的老师。不用管score表
```sql
select t.TNAME,t.DEPART from TEACHER t inner join COURSE c 
on t.tno =c.tno

-- 2表中区交集也可以使用in
SELECT TNAME,DEPART FROM TEACHER WHERE TNO IN (SELECT TNO FROM COURSE);
-- 能使用in就能使用exists
select tname,depart from teacher a where exists
(select * from course b where a.tno=b.tno);
```sql

##35  查询所有未讲课的教师的Tname和Depart. 
```sql
select tname,depart from TEACHER where  not exists 
(select * from COURSE where tno=TEACHER.tno)
```

##36、查询至少有2名男生的班号。
```sql
select class from STUDENT where SSEX='男'
group by CLASS having  count(*)>1
```

##37、查询Student表中不姓“王”的同学记录。
```sql
select * from STUDENT where sname not like '王%'
```

##38、查询Student表中每个学生的姓名和年龄。
涉及到日期函数使用 now()和year() 
```sql
select sname,year(now())-year(SBIRTHDAY) as age from STUDENT
```

##39、查询Student表中最大和最小的Sbirthday日期值。
思想：有关系的聚焦筛选 要不使用自连接+group+having 或者 子select+子select中的聚焦函数
```sql
select sname,sbirthday from student where sbirthday =(select min(SBIRTHDAY) 
from student)
union
select sname,sbirthday from student where sbirthday =(select max(SBIRTHDAY) from 
student);
```

##40、以班号和年龄从大到小的顺序查询Student表中的全部记录。

思路：班号按大到小，出生日期按小到大
```sql
select * from STUDENT 
order by class desc,SBIRTHDAY asc
```
可以按构造的新属性进行排序
```sql
select sname,2017-year(sbirthday) as age,class  from STUDENT
order by class desc,age desc 

-- 也可以放后面
select sname,class,sbirthday  from STUDENT
order by class desc,2017-year(sbirthday)  desc 
```

##41、查询“男”教师及其所上的课程。
```sql
select t.tname,c.cname from TEACHER t join course c on  t.tno=c.tno
where t.tsex='男'
```
如果二表相连的属性名相同，则可以使用using代替on 二者等价
```sql
select t.tname,c.cname from TEACHER t  join course c using(tno)
where t.tsex='男'
```

##42、查询最高分(不分课程)同学的Sno、Cno和Degree列。
```sql
select * from SCORE where DEGREE=
(select max(DEGREE) from SCORE )
```
拓展 查询最高分(分课程)同学的姓名、Cno和Degree
```sql
select s1.DEGREE,s1.CNO,s.SNAME from SCORE s1 /*把id换成名字*/  join STUDENT s using(sno) 
where s1.DEGREE = 
(select max(DEGREE) from SCORE s2 where s1.CNO=s2.CNO)
```

##43、查询和“李军”同性别的所有同学的Sname.
```sql
select s1.* from STUDENT s1 join STUDENT s2 
on (s2.SNAME='李军' and s1.SSEX=s2.SSEX)

-- 或者更直观的写法
SELECT SNAME,sno FROM STUDENT A WHERE SSEX=
(SELECT SSEX FROM STUDENT B WHERE B.SNAME='李军');
```
on 和 where的区别 on比where先执行，
在内联结的情况下，on和where的效果相同，
left和right联结是，on和where结果不同

##44、查询和“李军”同性别并同班的同学Sname
```sql
select s1.* from STUDENT s1 join STUDENT s2 
on (s2.SNAME='李军' and s1.SSEX=s2.SSEX and s1.CLASS=s2.CLASS)
```

##45、查询所有选修“计算机导论”课程的“男”同学的成绩表
```sql
select s1.* from SCORE s1 join (STUDENT s2,COURSE c) 
using (sno,cno)
where c.CNAME='计算机导论' and s2.ssex='男'
```
