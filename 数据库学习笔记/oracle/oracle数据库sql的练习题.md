### oracle数据库sql的练习题

```sql
-- 1.查询20号部门的所有员工信息。
select *
from EMP
where DEPTNO = 20;

-- 2.查询所有工种为CLERK的员工的工号、员工名和部门名。
select e.EMPNO, e.ENAME, d.DNAME
from emp e
         join DEPT d on e.DEPTNO = d.DEPTNO
where JOB = 'CLERK';

-- 3.查询奖金（COMM）高于工资（SAL）的员工信息。
select *
from EMP
where COMM > SAL;

-- 4.查询奖金高于工资的20%的员工信息。
select *
from EMP
where COMM > SAL * (0.2);

-- 5.查询10号部门中工种为MANAGER和20号部门中工种为CLERK的员工的信息。
select *
from EMP
where (JOB = 'MANAGER' and DEPTNO = 10)
   OR (JOB = 'CLERK' and DEPTNO = 20);
-- 6.查询所有工种不是MANAGER和CLERK，且工资大于或等于2000的员工的详细信息。

select *
from EMP
where JOB != 'MANAGER'
  and JOB != 'CLERK'
  and SAL >= 2000;

-- 7.查询有奖金的员工的不同工种。
select distinct (job)
from EMP
where COMM is not null
  and COMM > 0;

-- 8.查询所有员工工资和奖金的和。
select EMPNO, ENAME, (SAL + nvl(COMM, 0)) SALCOMM
from EMP;

-- 9.查询没有奖金或奖金低于100的员工信息。
select *
from EMP
where COMM is null
   or COMM < 100;

-- 10.查询各月倒数第2天入职的员工信息。
select *
from EMP
where HIREDATE in (select (last_day(HIREDATE) - 1) from EMP);

-- 11.查询员工工龄大于或等于10年的员工信息。
select *
from EMP
where (sysdate - HIREDATE) / 365 > 10;

-- 12.查询所有员工信息,其中员工姓名首字母以大写开头
select EMPNO,
       initcap(ENAME) ENAME,
       JOB,
       MGR,
       HIREDATE,
       SAL,
       COMM,
       DEPTNO
from EMP;

-- 13.查询员工名正好为6个字符的员工的信息
select *
from EMP
where length(ENAME) = 6;

-- 14.查询员工名字中不包含字母“S”员工
select *
from EMP
where instr(ENAME, 'S') = 0;

-- 15.查询员工姓名的第2个字母为“M”的员工信息。
select *
from EMP
where instr(ENAME, 'M') = 2;

-- 16.查询所有员工姓名的前3个字符
select substr(ENAME, 0, 3)
from EMP;

-- 17.查询所有员工的姓名，如果包含字母“s”，则用“S”替换
select TRANSLATE(ENAME, 's', 'S')
from EMP;

-- 18.查询员工的姓名和入职日期，并按入职日期从先到后进行排列。
select ENAME, HIREDATE
from EMP
order by HIREDATE;

-- 19.显示所有的姓名、工种、工资和奖金，按工种降序排列，若工种相同则按工资升序排列。
select ENAME, JOB, SAL, COMM
from EMP
order by job desc, SAL asc;

-- 20.显示所有员工的姓名、入职的年份和月份，若入职日期所在的月份排序，若月份相同则按入职的年份排序。
select ENAME,
       extract(year from HIREDATE)  year,
       extract(month from HIREDATE) month
from EMP
order by month, year;

-- 21.查询在2月份入职的所有员工信息。
select *
from EMP
where extract(month from HIREDATE) = 2;

select *
from EMP
where to_char(HIREDATE, 'MM') = 2;

-- 22.查询所有员工入职以来的工作期限，用“**年**月**日”的形式表示
select floor((sysdate - HIREDATE) / 365) || '年'
           || floor(mod((sysdate - HIREDATE), 365) / 30) || '月'
           || ceil(mod(mod(sysdate - HIREDATE, 365), 30)) || '日'
from EMP;

-- 23.查询至少有一个员工的部门信息
select *
from DEPT
where DEPTNO in (select distinct (EMP.DEPTNO) from EMP where EMP.MGR is not null);

-- 24.查询工资比SMITH员工工资高的所有员工信息。
select *
from EMP
where SAL > (select SAL from EMP where ENAME = 'SMITH');

-- 25.查询所有员工的姓名及其直接上级的姓名
select e.ENAME, e2.ENAME
from EMP e
         join emp e2 on e.DEPTNO = e2.DEPTNO
where e2.JOB = 'MANAGER';

-- 不知道表中还有一个字段mgr
select e1.ENAME, e2.ENAME
from emp e1
         join emp e2 on
    e1.mgr = e2.EMPNO;

-- 26.查询入职日期早于其直接上级领导的所有员工信息
select e1.ENAME
from EMP e1
         join emp e2 on
    e1.mgr = e2.EMPNO and e1.HIREDATE < e2.HIREDATE;

select ENAME
from emp
where empno in (select staempno
                from (select empno staempno, hiredate stahiredate, mgr from emp) t
                         join emp on t.mgr = emp.empno and stahiredate < hiredate);

-- 27.查询所有部门及其员工信息，包括那些没有员工的部门。
select *
from DEPT
         left join EMP E on DEPT.DEPTNO = E.DEPTNO
order by DEPT.DEPTNO;

-- 28.查询所有员工及其部门信息，包括那些还不属于任何部门的员工
select *
from DEPT
         right join EMP E on DEPT.DEPTNO = E.DEPTNO
order by DEPT.DEPTNO;

-- 29.查询所有工种为CLERK的员工的姓名及其部门名称
select e.ENAME, d.DNAME
from EMP e
         join DEPT d on e.JOB like 'CLERK' and e.DEPTNO = d.DEPTNO;

-- 30.查询最低工资大于2500的各种工作。
select *
from (select min(sal) min_sal, job
      from EMP
      group by JOB)
where min_sal > 2500;

-- 31.查询最低工资低于2000的部门及其员工信息。
select t.DEPTNO, d.DNAME, e.ENAME
from (select min(sal) min_sal, DEPTNO
      from EMP
      group by DEPTNO) t
         join dept d on t.DEPTNO = d.DEPTNO
         join emp e on e.DEPTNO = t.DEPTNO
where t.min_sal < 2000
order by t.DEPTNO;


select *
from emp
where deptno in (select deptno from (select min(sal) min_sal, deptno from emp group by deptno) where min_sal < '2000');

-- 32.查询在SALES部门工作的员工的姓名信息
select E.ENAME, E.DEPTNO
from EMP E
         join DEPT D
              on E.DEPTNO = D.DEPTNO and D.DNAME = 'SALES';

select ename
from emp
where deptno = (select deptno from dept where dname like 'SALES');

-- 33.查询工资高于公司平均工资的所有员工信息
select *
from emp
where SAL > (select avg(SAL)
             from EMP);

-- 34.查询与SMITH员工从事相同工作的所有员工信息
select *
from EMP
where JOB = (select JOB from EMP where ENAME like 'SMITH')
  and ENAME not like 'SMITH';

-- 35.列出工资等于30号部门中某个员工工资的所有员工的姓名和工资。
select ENAME, EMPNO
from EMP
where SAL in (select SAL from EMP where DEPTNO = 30)
  and EMPNO != '30';

select ename, sal
from emp
where sal = any (select sal from emp where deptno = 30);

-- 36.查询工资高于30号部门中工作的所有员工的工资的员工姓名和工资。
select ename, sal
from emp
where sal > all (select sal from emp where deptno = 30);

-- 37.查询每个部门中的员工数量、平均工资和平均工作年限

select d.DNAME, t.员工数量, t.平均工资, t.平均工作年限
from DEPT d
         join (select DEPTNO,
                      count(1)                        员工数量,
                      avg(SAL)                        平均工资,
                      avg((SYSDATE - HIREDATE) / 365) 平均工作年限
               from EMP
               group by DEPTNO) t on d.DEPTNO = t.DEPTNO;

select dname, count, avg_sal, avg_date
from dept
         join (select count(*) count, avg(sal) avg_sal, avg((sysdate - hiredate) / 365) avg_date, deptno
               from emp
               group by deptno) t on dept.deptno = t.deptno;

-- 38.查询从事同一种工作但不属于同一部门的员工信息。
select distinct e1.EMPNO, e1.ENAME, e1.DEPTNO
from EMP e1
         join emp e2 on e1.JOB like e2.JOB and e1.DEPTNO <> e2.DEPTNO;

select distinct t1.empno, t1.ename, t1.deptno
from emp t1
         join emp t2 on t1.job like t2.job and t1.deptno <> t2.deptno;

-- 39.查询各个部门的详细信息以及部门人数、部门平均工资。
select d.*, t.员工数量, t.平均工资
from DEPT d
         join (select DEPTNO,
                      count(1) 员工数量,
                      avg(SAL) 平均工资
               from EMP
               group by DEPTNO) t on t.DEPTNO = d.DEPTNO;

-- 40.查询各种工作的最低工资
select job, min(SAL)
from emp
group by JOB;

-- 41.查询各个部门中的不同工种的最高工资
select DEPTNO, JOB, max(sal)
from EMP
group by DEPTNO, JOb
order by DEPTNO, JOB;
-- 42.查询10号部门员工以及领导的信息。
select *
from EMP
where EMPNO in (select MGR from emp where DEPTNO = 10)
   or DEPTNO = 10;

--43.查询各个部门的人数及平均工资。
select count(1), avg(SAL)
from EMP
group by DEPTNO;

-- 44.查询工资为某个部门平均工资的员工信息。
select *
from EMP
where SAL = any (select avg(SAL)
                 from EMP
                 group by DEPTNO);

-- 45.查询工资高于本部门平均工资的员工的信息。
select e.*
from EMP e
         join (select avg(SAL) a_sal, DEPTNO
               from EMP
               group by DEPTNO) t on e.DEPTNO = t.DEPTNO and e.SAL > t.a_sal;

-- 46.查询工资高于本部门平均工资的员工的信息及其部门的平均工资。
select e.*, t.a_sal
from EMP e
         join (select avg(SAL) a_sal, DEPTNO
               from EMP
               group by DEPTNO) t on e.DEPTNO = t.DEPTNO and e.SAL > t.a_sal;

-- 47.查询工资高于20号部门某个员工工资的员工的信息。
select *
from EMP
where SAL > any (select SAL from EMP where DEPTNO = 20);

-- 48.统计各个工种的人数与平均工资。
select JOB, count(1), avg(SAL)
from EMP
group by JOB
order by JOB;

-- 49.统计每个部门中各个工种的人数与平均工资。
select DEPTNO, JOB, count(1), avg(SAL)
from EMP
group by DEPTNO, JOB
order by DEPTNO, JOB;

-- 50.查询工资、奖金与10 号部门某个员工工资、奖金都相同的员工的信息。
select *
from EMP e
         join (select SAL, COMM from EMP where DEPTNO = 10) e1
              on e.sal = e1.SAL
                  and nvl(e.COMM, 0) = nvl(e1.COMM, 0) and e.DEPTNO != 10;

-- 51.查询部门人数大于5的部门的员工的信息。
select *
from EMP
where DEPTNO in (select DEPTNO
                 from (
                          select DEPTNO, count(1) cou
                          from EMP
                          group by DEPTNO) t
                 where t.cou > 5);

-- 52.查询所有员工工资都大于1000的部门的信息
select *
from DEPT
where DEPTNO in (select distinct t.DEPTNO
                 from (select min(SAL) a_sal, DEPTNO
                       from EMP
                       group by DEPTNO) t
                 where t.a_sal > 1000);

-- 53.查询所有员工工资都大于1000的部门的信息及其员工信息。
select d.*, e.*
from DEPT d
         join emp e on d.DEPTNO = e.DEPTNO
    and d.DEPTNO in (select distinct t.DEPTNO
                     from (select min(SAL) a_sal, DEPTNO
                           from EMP
                           group by DEPTNO) t
                     where t.a_sal > 1000);

-- 54.查询所有员工工资都在900~3000之间的部门的信息
select *
from DEPT d
where d.DEPTNO in (select distinct DEPTNO
                   from emp
                   where DEPTNO not in (select distinct DEPTNO
                                        from EMP
                                        where sal not between 900 and 3000));

select *
from dept
where deptno in (select distinct deptno
                 from emp
                 where deptno not in (select distinct deptno from emp where sal not between 900 and 3000));


-- 55.查询所有工资都在900~3000之间的员工所在部门的员工信息。
select *
from EMP
where DEPTNO in (select distinct deptno
                 from emp
                 where deptno not in (select distinct deptno from emp where sal not between 900 and 3000));

-- 56.查询每个员工的领导所在部门的信息
-- 每个员工的领导
-- 每个领导的单位
-- 关联每个单位表信息
select d.*
from DEPT d
         join (select distinct DEPTNO
               from emp
               where EMPNO in (select distinct MGR
                               from EMP)) t on d.DEPTNO = t.DEPTNO;

select e1.EMPNO, e1.ENAME, e1.MGR mno, e2.ENAME, e2.DEPTNO
from emp e1
         join emp e2 on e1.MGR = e2.EMPNO;

select *
from (select e1.EMPNO, e1.ENAME, e1.MGR mno, e2.ENAME mname, e2.DEPTNO
      from emp e1
               join emp e2 on e1.MGR = e2.EMPNO) t
         join DEPT d on t.DEPTNO = d.DEPTNO;


select *
from (select e1.empno, e1.ename, e1.mgr mno, e2.ename mname, e2.deptno
      from emp e1
               join emp e2 on e1.mgr = e2.empno) t
         join dept on t.deptno = dept.deptno;

-- 57.查询人数最多的部门信息
select count(1) cou, DEPTNO
from EMP
group by DEPTNO;

select max(cou)
from (select count(1) cou, DEPTNO
      from EMP
      group by DEPTNO);

select t.cou, d.*
from (select count(1) cou, DEPTNO
      from EMP
      group by DEPTNO) t
         join DEPT d on t.DEPTNO = d.DEPTNO
    and t.cou in (select max(cou)
                  from (select count(1) cou, DEPTNO
                        from EMP
                        group by DEPTNO));


select *
from dept
where deptno in (select deptno
                 from (select count(*) count, deptno from emp group by deptno)
                 where count in (select max(count) from (select count(*) count, deptno from emp group by deptno)));

-- 58.查询30号部门中工资排序前3名的员工信息。
select *
from EMP
where DEPTNO = 30
order by SAL desc;

select t.*
from (select *
      from EMP
      where DEPTNO = 30
      order by SAL desc) t
where ROWNUM < 4;


select *
from emp
where empno in (select empno from (select empno, sal from emp where deptno = 30 order by sal desc) where rownum < 4);

-- 59.查询所有员工中工资排在5~10名之间的员工信息
select *
from EMP
order by SAL desc;

select *, ROWNUM r
from (select *
      from EMP
      order by SAL desc);

select *
from emp
where empno in (select empno
                from (select empno, rownum num from (select empno, sal from emp order by sal desc))
                where num between 5 and 10);

-- 取出num
select EMPNO, ROWNUM num
from (select EMPNO, SAL
      from EMP
      order by SAL desc);


select EMPNO
from (select EMPNO, ROWNUM num
      from (select EMPNO, SAL
            from EMP
            order by SAL desc)) t
where t.num between 6 and 10;
-- 最终答案
select *
from EMP
where EMPNO in (select EMPNO
                from (select EMPNO, ROWNUM num
                      from (select EMPNO, SAL
                            from EMP
                            order by SAL desc)) t
                where t.num between 6 and 10)
order by SAL desc;

--  59.查询所有员工中工资排在5~10名之间的员工信息
-- 使用差集计算
select *
from (select empno, sal from emp order by sal desc)
where rownum <= 10
minus
select *
from (select empno, sal from emp order by sal desc)
where rownum <= 5;

-- 60.向emp表中插入一条记录，员工号为1357，员工名字为oracle，工资为2050元，部门号为20，入职日期为2002年5月10日。
insert into EMP(empno, ename, sal, deptno, HIREDATE)
VALUES (1357, 'oracle', 2050, 20, to_date('2002-05-10', 'yyyy-MM-DD'));
select *
from EMP
where EMPNO = 1357;

-- 61. 向emp表中插入一条记录，员工名字为FAN，员工号为8000，其他信息与SMITH员工的信息相同。
insert into EMP
select 8000,
       'FAN',
       JOB,
       MGR,
       HIREDATE,
       SAL,
       COMM,
       DEPTNO
from emp
where ename like 'SMITH';

select *
from EMP;

-- 62. 将各部门员工的工资修改为该员工所在部门平均工资加1000。
update EMP e
set SAL = (select avg(sal) from EMP e1 where e.DEPTNO = e1.DEPTNO) + 1000;

--
update emp e
set SAL = 1000 + (select a_sal
                  from (select avg(sal) a_sal, e1.DEPTNO from EMP e1 group by e1.DEPTNO) t
                  where t.DEPTNO = e.DEPTNO);

-- 恢复数据库
-- 1.查询数据库当前时间
select to_char(sysdate, 'yyyy-mm-dd hh24:mi:ss')
from DUAL;
--
select *
from EMP as of timestamp to_timestamp('2021-01-20 10:08:45', 'yyyy-mm-dd hh24:mi:ss');
--
flashback table EMP to timestamp to_timestamp('2021-01-20 10:08:44', 'yyyy-mm-dd hh24:mi:ss');
-- 出现[72000][8189] ORA-08189: 因为未启用行移动功能, 不能闪回表
-- 执行功能开启
alter table EMP
    enable row movement;

select *
from EMP;

-- 练习题第二篇
-- 1.查询82年员工
select *
from EMP
where to_char(HIREDATE, 'yy') like '82';

select *
from EMP
where to_char(HIREDATE, 'yyyy') like '1982';

-- 2.查询32年工龄的人员
select *
from EMP w
where round((sysdate - HIREDATE) / 356) = 42;
-- 3.显示员工雇佣期6个月后下一个星期一的日期
select next_day(add_months(HIREDATE, 6), 2)
from EMP;
-- 4.找没有上级的员工，把mgr的字段信息输出为 "boss"
select empno,
       ename,
       job,
       decode(MGR, null, 'boss'),
       hiredate,
       sal,
       comm,
       deptno
from EMP
where MGR is null;

-- 5.为所有人长工资，标准是：10部门长10%；20部门长15%；30部门长20%其他部门长18%
select decode(e.deptno, 10, e.sal * 1.1, 20, e.sal * 1.15, e.sal * 1.18) 涨工资,
       e.deptno,
       e.sal
from emp e;

-- 练习题
-- 1.求部门中薪水最高的人
select *
from EMP
where SAL in (select max(SAL)
              from EMP
              group by DEPTNO);

select max(SAL)
from EMP
group by DEPTNO;

-- 2.求部门平均薪水的等级
select t.DEPTNO, s.GRADE
from (select avg(SAL) a_sal, DEPTNO
      from EMP
      group by DEPTNO) t
         join SALGRADE s on t.a_sal between s.LOSAL and s.HISAL;

-- 3.求部门平均的薪水等级
-- 先关联薪水等级
select e.*, s.GRADE
from emp e
         join SALGRADE s on e.SAL between s.LOSAL and s.HISAL;
-- 求个部门的薪水等级的平均数
select DEPTNO, avg(t.GRADE)
from (select e.*, s.GRADE
      from emp e
               join SALGRADE s on e.SAL between s.LOSAL and s.HISAL) t
group by t.DEPTNO;

-- 4. 雇员中有哪些人是经理人
select *
from EMP
where EMPNO in (select distinct MGR from EMP);

select distinct e2.ENAME
from EMP e1
         join EMP e2 on e1.MGR = e2.EMPNO;

-- 5. 不准用组函数，求薪水的最高值
select SAL
from (select SAL
      from EMP
      order by SAL desc)
where ROWNUM < 2;

select distinct sal max_sal
from emp
where sal not in
      (select e1.sal e1_sal
       from emp e1
                join emp e2 on e1.sal < e2.sal);


-- 6. 求平均薪水最高的部门的部门编号
-- 求部门的平均薪水
select avg(SAL) a_sal, DEPTNO
from EMP
group by DEPTNO;

-- 求部门平均薪水最高 
select max(a_sal)
from (select avg(SAL) a_sal, DEPTNO
      from EMP
      group by DEPTNO);

-- 求最高薪水的部门编号
select t.DEPTNO
from (select avg(SAL) a_sal, DEPTNO
      from EMP
      group by DEPTNO) t
where t.a_sal = (select max(a_sal)
                 from (select avg(SAL) a_sal, DEPTNO
                       from EMP
                       group by DEPTNO));

-- 7.求平均薪水最高的部门的部门名称
select DNAME
from DEPT
where DEPTNO = (select t.DEPTNO
                from (select avg(SAL) a_sal, DEPTNO
                      from EMP
                      group by DEPTNO) t
                where t.a_sal = (select max(a_sal)
                                 from (select avg(SAL) a_sal, DEPTNO
                                       from EMP
                                       group by DEPTNO)));

-- 8. 求平均薪水的等级最低的部门的部门名称
select t.a_sal, s.GRADE, DEPTNO
from (select avg(SAL) a_sal, DEPTNO from EMP group by DEPTNO) t
         join SALGRADE s on t.a_sal between s.LOSAL and s.HISAL;

-- 求最小等级
select min(GRADE)
from (select t.a_sal, s.GRADE, DEPTNO
      from (select avg(SAL) a_sal, DEPTNO from EMP group by DEPTNO) t
               join SALGRADE s on t.a_sal between s.LOSAL and s.HISAL);

-- 求最小等级

select t.DEPTNO, d.DNAME
from (select t.a_sal, s.GRADE, DEPTNO
      from (select avg(SAL) a_sal, DEPTNO from EMP group by DEPTNO) t
               join SALGRADE s on t.a_sal between s.LOSAL and s.HISAL) t
         join DEPT d on t.DEPTNO = d.DEPTNO
where t.GRADE in (select min(GRADE)
                  from (select t.a_sal, s.GRADE, DEPTNO
                        from (select avg(SAL) a_sal, DEPTNO from EMP group by DEPTNO) t
                                 join SALGRADE s on t.a_sal between s.LOSAL and s.HISAL));

-- 9.求部门经理人中平均薪水最低的部门名称
select min() over ()
from EMP
where JOB = 'MANAGER';

select dname
from (select deptno, avg(sal) avg_sal from emp where empno in (select mgr from emp) group by deptno) t
         join dept on t.deptno = dept.deptno
where avg_sal = (select min(avg_sal)
                 from (select avg(sal) avg_sal
                       from emp
                       where empno in
                             (select mgr from emp)
                       group by deptno) t);


select avg(sal) a_sal
from (select *
      from EMP
      where EMPNO in (
          select mgr
          from EMP)) t
group by t.DEPTNO;

select min(a_sal)
from (select avg(sal) a_sal
      from (select *
            from EMP
            where EMPNO in (
                select mgr
                from EMP)) t
      group by t.DEPTNO);

select t.*, d.DNAME
from (
         select avg(sal) a_sal, DEPTNO
         from (select *
               from EMP
               where EMPNO in (
                   select mgr
                   from EMP)) t
         group by t.DEPTNO) t
         join dept d on d.DEPTNO = t.DEPTNO
where t.a_sal = (select min(a_sal)
                 from (select avg(sal) a_sal
                       from (select *
                             from EMP
                             where EMPNO in (
                                 select mgr
                                 from EMP)) t
                       group by t.DEPTNO));

-- 10. 求比普通员工的最高薪水还要高的经理人名称(not in)
select max(t.sal) m_sal, DEPTNO
from (
         select *
         from EMP
         where EMPNO not in (select distinct mgr
                             from EMP
                             where mgr is not null)) t
group by t.DEPTNO;

select d.DNAME, d.DEPTNO, t.m_sal, t2.SAL
from DEPT d
         join (select max(t.sal) m_sal, DEPTNO
               from (
                        select *
                        from EMP
                        where EMPNO not in (select distinct mgr
                                            from EMP
                                            where mgr is not null)) t
               group by t.DEPTNO) t on d.DEPTNO = t.DEPTNO
         join (select * from emp where EMPNO in (select distinct MGR from EMP)) t2
              on t2.DEPTNO = t.DEPTNO and t.m_sal < t2.SAL;
-- 这里是直接比较普通员工比经理最高的的工资
select ename
from emp
where empno in (select mgr from emp)
  and sal > (select max(sal)
             from (select e2.sal
                   from emp e1
                            right join emp e2 on e1.mgr = e2.empno
                   where e1.ename is null) t);


select ename
from emp
where empno in (select mgr from emp)
  and sal >
      (select max(sal) from emp where empno not in (select distinct mgr from emp where mgr is not null));


-- 11.求薪水最高的前5名雇员
select *
from (select *
      from EMP
      order by SAL desc) where ROWNUM <=5;

-- 12.求薪水最高的第6到第10名雇(!important)

select t.*,ROWNUM r
from (select *
      from EMP
      order by SAL desc) t;

select *
from (select t.*, ROWNUM r
      from (select *
            from EMP
            order by SAL desc) t) t where t.r >5 and t.r <11;

-- 13.求最后入职的5名员工
select *
from EMP order by HIREDATE desc ;

select *
from (select *
from EMP order by HIREDATE desc) t  where ROWNUM <=5;


select ename, to_char(hiredate, 'YYYY"年"MM"月"DD"日"') hiredate
from (select t.*, rownum r from (select * from emp order by hiredate desc) t)
where r <= 5;
```

