表结构：
course    cno 课程编号，cname 课程名称 tno 老师id号
score     sno学生id        cno 课程id        degree 成绩
student  sno学生id    sname 学生名字   ssex 学生性别   sbirthday 学生生日 class所属班级id
teacher  tno 老师id   tname 老师姓名 tsex 老师性别  tbirthday 老师生日   prof老师职位 depart所属院系



23、查询“张旭“教师任课的学生成绩
```sql
select * from SCORE where cno in(
(select A.cno from COURSE A join TEACHER B on A.TNO=B.TNO 
 WHERE B.TNAME='张旭')
```
---

26、查询存在有85分以上成绩的课程Cno.
