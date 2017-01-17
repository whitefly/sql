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
SELECT * FROM SCORE WHERE cno='3-105' and DEGREE>ANY(SELECT DEGREE FROM SCORE WHERE CNO='3-245') ORDER BY DEGREE DESC
```
其实我的第一反应是建立第一个自连接， 左边大于min（右边） 但是min（）为聚焦函数，自连接中无法使用。于是强行group by
