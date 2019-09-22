
## 1.基础查询

语法：select 查询列表 from 表名;  
类似于：System.out.println(打印东西);

特点：    
1、查询列表可以是：表中的字段、常量值、表达式、函数  
2、查询的结果是一个虚拟的表格   
  

#### 1.查询表中的单个字段

	SELECT last_name FROM employees;

#### 2.查询表中的多个字段
	SELECT last_name,salary,email FROM employees;

#### 3.查询表中的所有字段

##### 方式一：
```
SELECT 
    `employee_id`,
    `first_name`,
    `last_name`,
    `phone_number`,
    `last_name`,
    `job_id`,
    `phone_number`,
    `job_id`,
    `salary`,
    `commission_pct`,
    `manager_id`,
    `department_id`,
    `hiredate` 
FROM
    employees ;  
```    
##### 方式二：  
	SELECT * FROM employees;
 
#### 4.查询常量值
	 SELECT 100;
	 SELECT 'john';
 
#### 5.查询表达式
 	SELECT 100%98;
 
#### 6.查询函数
 	SELECT VERSION();
 
#### 7.起别名
 
 ①便于理解  
 ②如果要查询的字段有重名的情况，使用别名可以区分开来    

 ##### 方式一：使用as
	SELECT 100%98 AS 结果;
	SELECT last_name AS 姓,first_name AS 名 FROM employees;

##### 方式二：使用空格
	SELECT last_name 姓,first_name 名 FROM employees;

	案例：查询salary，显示结果为 out put  
	SELECT salary AS "out put" FROM employees;


#### 8.去重
	案例：查询员工表中涉及到的所有的部门编号  
	SELECT DISTINCT department_id FROM employees;


#### 9.+号的作用

**java中的 + 号：   
①运算符，两个操作数都为数值型  
②连接符，只要有一个操作数为字符串**

**mysql中的 + 号：仅仅只有一个功能：运算符**  
```
select 100+90; 两个操作数都为数值型，则做加法运算    
select '123'+90;只要其中一方为字符型，试图将字符型数值转换成数值型  
			如果转换成功，则继续做加法运算  
select 'john'+90;	如果转换失败，则将字符型数值转换成0  

select null+10; 只要其中一方为null，则结果肯定为null  
```  
  
```
案例：查询员工名和姓连接成一个字段，并显示为 姓名
SELECT CONCAT('a','b','c') AS 结果;

SELECT 
	CONCAT(last_name," ",first_name) AS 姓名
FROM
	employees;
```
---
## 条件查询

语法：

	select 
		查询列表
	from
		表名
	where
		筛选条件;

分类：

	一、按条件表达式筛选
	简单条件运算符：> < = != <> >= <=
	
	二、按逻辑表达式筛选
	逻辑运算符：
	作用：用于连接条件表达式
		&& || !
		and or not
		
	&&和and：两个条件都为true，结果为true，反之为false
	||或or： 只要有一个条件为true，结果为true，反之为false
	!或not： 如果连接的条件本身为false，结果为true，反之为false
	
	三、模糊查询
		like
		between and
		in
		is null
	

### 一、按条件表达式筛选

	案例1：查询工资>12000的员工信息

	SELECT 
		*
	FROM
		employees
	WHERE
		salary>12000;


	案例2：查询部门编号不等于90号的员工名和部门编号
	SELECT 
		last_name,
		department_id
	FROM
		employees
	WHERE
		department_id<>90;


### 二、按逻辑表达式筛选

	案例1：查询工资z在10000到20000之间的员工名、工资以及奖金
	SELECT
		last_name,
		salary,
		commission_pct
	FROM
		employees
	WHERE
		salary>=10000 AND salary<=20000;
	案例2：查询部门编号不是在90到110之间，或者工资高于15000的员工信息
	SELECT
		*
	FROM
		employees
	WHERE
		 NOT(department_id>=90 AND department_id<=110) OR salary>15000;

### 三、模糊查询 

+ like
+ between and
+ in
+ is null|is not null

#### 1.like

特点：一般和通配符搭配使用  
通配符：% 任意多个字符,包含0个字符  
       	_ 任意单个字符

	案例1：查询员工名中包含字符a的员工信息

	select 
		*
	from
		employees
	where
		last_name like '%a%';#abc

	案例2：查询员工名中第三个字符为e，第五个字符为a的员工名和工资
	select
		last_name,
		salary
	FROM
		employees
	WHERE
		last_name LIKE '__n_l%';



	案例3：查询员工名中第二个字符为_的员工名

	SELECT
		last_name
	FROM
		employees
	WHERE
		last_name LIKE '_$_%' ESCAPE '$';
	
#### 2.between and
特点：  
①使用between and 可以提高语句的简洁度  
②包含临界值  
③两个临界值不要调换顺序  

案例1：查询员工编号在100到120之间的员工信息

	SELECT
		*
	FROM
		employees
	WHERE
		employee_id >= 120 AND employee_id<=100;
	#-----------------------------------------------
	SELECT
		*
	FROM
		employees
	WHERE
		employee_id BETWEEN 120 AND 100;（临界值不能调换）

#### 3.in

含义：判断某字段的值是否属于in列表中的某一项   
特点：   
	①使用in提高语句简洁度  
	②in列表的值类型必须一致或兼容  
	③in列表中不支持通配符  

	案例：查询员工的工种编号是 IT_PROG、AD_VP、AD_PRES中的一个员工名和工种编号

	SELECT
		last_name,
		job_id
	FROM
		employees
	WHERE
		job_id = 'IT_PROT' OR job_id = 'AD_VP' OR JOB_ID ='AD_PRES';
	#------------------
	SELECT
		last_name,
		job_id
	FROM
		employees
	WHERE
		job_id IN( 'IT_PROT' ,'AD_VP','AD_PRES');


#### 4、is null

=或<>不能用于判断null值
is null或is not null 可以判断null值

	案例1：查询没有奖金的员工名和奖金率
	SELECT
		last_name,
		commission_pct
	FROM
		employees
	WHERE
		commission_pct IS NULL;


	案例2：查询有奖金的员工名和奖金率
	SELECT
		last_name,
		commission_pct
	FROM
		employees
	WHERE
		commission_pct IS NOT NULL;

	#----------以下为×
	SELECT
		last_name,
		commission_pct
	FROM
		employees

	WHERE 
		salary IS 12000;
	
	
安全等于  <=>

	案例1：查询没有奖金的员工名和奖金率
	SELECT
		last_name,
		commission_pct
	FROM
		employees
	WHERE
		commission_pct <=>NULL;


	案例2：查询工资为12000的员工信息
	SELECT
		last_name,
		salary
	FROM
		employees

	WHERE 
		salary <=> 12000;
	

#### is null pk <=>

IS NULL:仅仅可以判断NULL值，可读性较高，建议使用
<=>    :既可以判断NULL值，又可以判断普通的数值，可读性较低



























 
 
 


