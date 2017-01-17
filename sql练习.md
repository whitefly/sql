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
select A.* from SCORE A join (TEACHER B,COURSE C) on A.cno=C.CNO and C.tno=b.tno
where b.depart='计算机系'
```

