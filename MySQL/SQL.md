[TOC]

## MySql数据库操作

将SQL语句写在文档中，直接复制到命令行，可节省输入。

### 数据类型

> VARCHAR或CHAR，最多支持255个字符
> BLOB,支持大文本，即多于255个字符时
> DEC,DECIMAL表示小数， DEC(6, 2)表示最大位数为6位，小数点后面有2位
> INT, 表示整数
> DATA, 日期，格式为月日年，比如11/22/2018或者11-22-2018等
> DATATIME,TIMESTAMP,表示日期和时间，datatime通常表示未来的时间，timestamp通常表示当下时间

|       我们爱单引号      | 我们拒用单引号 |
|-------------------------|----------------|
| CHAR                    | DEC            |
| VARCHAR                 | INT            |
| DATE                    |                |
| DATATIME,TIME,TIMESTAMP |                |
| BLOB                    |                |

### 简单操作

```mysql
/*创建数据库*/
CREATE DATABASE learn_database;

/*进入learn_database数据库*/
USE learn_database;

/*创建表person， id设置为主键自增默认从1开始，name有默认值yangweijie,此时表为空表*/
CREATE TABLE person
(
id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(10) NOT NULL DEFAULT 'yangweijie'
);

/*显示数据库learn_database信息*/
SHOW learn_database;

/*显示表person的信息*/
DESC person;
DESC learn_database.person;

/*在表person中输入一行（也叫记录）*/
INSERT INTO person (id, name) VALUES (5, 'Jeyser');

/*在输入时只填入一部分列值*/
INSERT INTO person (id) VALUES (6);

/*输入省略列名，则必须填入所有的值*/
INSERT INTO person VALUES (7, 'JeyserYang');

/*查看表person中所有数据*/
SELECT * FORM person;

/*删除表*/
DROP TABLE IF EXISTS person;

/*使用Show命令查看原来使用的命令*/
SHOW CREATE TABLE person \G

```

### SELECT

```mysql

/*在表person中找到名字为yangweijie的所有列的数据*/
SELECT * FROM person WHERE name = 'yangweijie';

/*如果想要插入特殊字符，需要使用 \ 来转义*/
INSERT INTO person VALUES (8, 'yang\'s dog');

/*输入多行（记录）数据,中间以逗号分隔，最后以分号结束*/
INSERT INTO person (name) VALUES
('yang'),
('xuan'),
('ye');

/*选择特定列数据，将每一个写成一行看起来更清楚*/
SELECT id, name
FROM person
WHERE name = 'yangweijie';

/*在WHERE子句中使用 AND OR*/
SELECT id FROM person WHERE id = 5 AND name = 'yang';

/*使用小于号，大于号，等于号，不等于(<>)号等*/
SELECT * FROM person WHERE id > 6 OR name = 'Jeyser';
SELECT * FROM person WHERE id <> 6;

/*在字符串中使用比较运算符*/
/*这里需要注意，无论使用的大于等于还是大于都是一样的效果，都代表包含从 J 开始，无论使用的是小于等于还是小于，都代表不包含 Y*/
SELECT * FROM person WHERE name > 'J' AND name <= 'Y';
/*这句话与上面等效*/
SELECT * FROM person WHERE name BETWEEN 'J' AND 'Y';

/*如何选择值为NULL的数据*/
SELECT name FROM person WHERE birthday IS NULL;

/*使用LIKE通配符*/
/*%通配符代表任意数量的未知字符*/
SELECT * FROM person WHERE name LIKE '%ser';
/*_通配符代表一个未知字符*/
SELECT * FROM person WHERE name LIKE '%ser_ang';

/*选取一定范围的数据,下面两个式子等效，推荐第二种*/
SELECT * FROM person WHERE id >= 6 AND id <= 8;
SELECT * FROM person WHERE id BETWEEN 6 AND 8;

/*使用关键字IN来选取值的集合,NOT IN来排除某些信息*/
SELECT * FROM person WHERE name IN ('Jeyser', 'yangweijie');
SELECT * FROM person WHERE id NOT IN (5, 6);

/*使用NOT表示否定，不在的意思,NOT必须紧跟在where后面*/
SELECT * FROM person WHERE NOT id BETWEEN 5 AND 6;
/*选择出所有不以J开头和不以Y开头的列*/
SELECT * FROM person WHERE NOT name LIKE 'J%' AND NOT name LIKE 'y%';

```

### DELETE和UPDATE

```mysql

/*删除一行记录,DELETE后面不需要任何东西，可直接删除一行记录*/
DELETE FROM person WHERE name = 'YANG';

/*由于delete操作具有不可恢复性，所以每次删除记录时都应该使用select查看删除的数据是否无误*/
SELECT * FROM person WHERE id >= 8;
DELETE FROM person WHERE id >= 8;

/*使用update更新一列或多列数据*/
UPDATE person SET name = 'Yangweijie' WHERE name = 'yang\'s dog'; 
/*更新多列数据，使用逗号分隔，且SQL不区分大小写*/
UPDATE person SET birthday = '1998-02-02', name = 'Yangweijie' WHERE name = 'yangweijie';

/*使用数学基础运算(加减乘除)来update，也有针对字符串操作的函数，比如LOWER(),UPPER()*/
UPDATE person SET name = UPPER(name);

```

### ALTER

```mysql

/*Alter添加一列column数据，并指定位置after,before以及FIRST,LAST表示第一个和最后一个，另外还有SECOND,THIRD等可用*/
ALTER TABLE person ADD COLUMN address VARCHAR(20) AFTER name;

/*修改表名,列名rename*/
ALTER TABLE person RENAME TO human;
ALTER TABLE human RENAME COLUMN name TO human_name;

/*既要修改列名又要修改列的属性，使用change*/
/*注意：修改数据类型有可能出现数据精度丢失，比如从varchar(10)改到char(1)*/
ALTER TABLE human CHANGE COLUMN birthday birth DATE; 

/*假如只要修改数据类型，而保持列名不变modify*/
ALTER TABLE human MODIFY COLUMN birth varchar(20);

/*删除表中的某一列数据*/
ALTER TABLE human DROP COLUMN address;

/*字符串处理函数，可以搭配select，update，delete使用*/
/*选择birth列中从右向左数的两个字符*/
SELECT RIGHT(birth, 2) FROM human;
/*找到birth列中 - 第一次出现的位置，取出它前面所有的字符*/
SELECT SUBSTRING_INDEX(birth, '-', 1) FROM human;
/*选择字串，开始于4，长度为5，substr = substring*/
SELECT SUBSTRING("YANGWEIJIE", 4, 5);

SELECT REVERSE("yangweijie");
/*去掉左边的空格*/
SELECT LTRIM("  YANG ");
SELECT RTRIM(" YANG  ");
SELECT LENGTH("YANGWEIJIE");

/*现在如果我们想要将birth拆分为年月日三个列属性怎么做*/
ALTER TABLE human 
ADD COLUMN year VARCHAR(4),
ADD COLUMN month VARCHAR(3),
ADD COLUMN day VARCHAR(3);
UPDATE human 
SET year = LEFT(birth, 4), month = SUBSTRING(birth, 6, 2), day = RIGHT(birth, 2);

```

### SELECT进阶（CASE,ORDER BY,GROUP BY and so on)

```mysql

/*使用case语句更新，类似于选择语句*/
UPDATE human SET address =
CASE
    WHEN address IS NULL
        THEN 'CHINA'
    WHEN address = 'china'
        THEN 'JIAXING'
    ELSE 'SHANGHAI'
END;

/*使用order by排序单列，多列，配合select使用*/
/*升序排序规则：null， 数字， 大写字母， 小写字母，也可加上ASC，但没必要*/
SELECT id, human_name, address FROM human WHERE birth = '1998-02-02' ORDER BY address, id;

/*添加DESC反序,DESC必须放在列名的后面*/
SELECT * FROM human ORDER BY human_name DESC;

/*假设现在我们有这样一张表，里面存储多人的多天销售记录，如何找到销售总额最大的数据,使用
Group By实现分组，使用SUM来求和,AVG来求平均值，MAX，MIN求最大值和最小值，COUNT来计数指定列的行数。使用Order By来排序*/
/*此表创建，输入如下所示*/
CREATE TABLE cookie_sales
(
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    sales DEC(5, 2),
    sale_date DATE
);
INSERT INTO cookie_sales VALUES (NULL, 'Jim', 15.52, '2011-03-06');
INSERT INTO cookie_sales VALUES (NULL, 'John', 14.95, '2011-03-06');
INSERT INTO cookie_sales (name, sales, sale_date) VALUES ('Jim', 48.52, '2011-03-07');
INSERT INTO cookie_sales (name, sales, sale_date) VALUES ('John', 25.58, '2011-03-07');
SELECT name, SUM(sales), AVG(sales), MAX(sales), COUNT(sale_date)
FROM cookie_sales
GROUP BY name
ORDER BY SUM(sales) DESC;

/*使用Distinct关键字找到不重复的值,不同的值只会返回一次*/
SELECT DISTINCT sale_date
FROM cookie_sales
ORDER BY sale_date;

/*将Count和Distinct搭配使用，注意Distinct也要出现在括号里,找到所有不同数据的个数*/
SELECT COUNT(DISTINCT sale_date)
FROM cookie_sales;

/*使用LIMIT限制查询结果的数量（行数）*/
SELECT name, SUM(sales)
FROM cookie_sales
GROUP BY name
ORDER BY SUM(sales) DESC
LIMIT 2;

/*limit还可以指定参数，返回指定序号范围的结果*/
/*比如LIMIT 0,4代表显示从0开始，长度为4的记录，示例如下*/
SELECT * FROM cookie_sales LIMIT 0, 3;

```

### 多张表设计(Inner Join, Foreign key, AS, SubQuery, Not Exists)

```mysql

/*从一个已有的表中提取列成为一个新的表,有以下三种方法*/
/*第一种，先创建新表，插入元素，使用select出的列*/
CREATE TABLE name
(
    id INT AUTO_INCREMENT PRIMARY KEY,
    person_name VARCHAR(20) 
);
INSERT INTO name (person_name)
SELECT name FROM cookie_sales GROUP BY name;

/*第二种，利用select进行create table，然后使用Alter来添加主键*/
CREATE TABLE name AS
SELECT name FROM cookie_sales GROUP BY name;
ALTER TABLE name 
ADD COLUMN id INT AUTO_INCREMENT FIRST,
ADD PRIMARY KEY (id);

/*同一时间使用create，select，insert,这里的AS是alias别名的意思*/
CREATE TABLE name
(
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(15),
    bir DATE
) AS SELECT name, sale_date FROM cookie_sales GROUP BY name;

/*给表或列设置别名，用于在以后使用JOIN连接操作时不至于头晕眼花*/
SELECT name AS person_name
FROM name AS sale_name;

/*考虑我们现在有两张表，一张表是男孩表，包括id和姓名，一张表是玩具表，包含id和玩具种类
；且为了两者之间有关联，在男孩表中使用foreign key表示男孩喜欢的玩具*/
/*现在两张表的创建如下*/
CREATE TABLE toys
(
    toy_id INT AUTO_INCREMENT PRIMARY KEY,
    toy_name VARCHAR(20)
);
CREATE TABLE boys
(
    boy_id INT AUTO_INCREMENT PRIMARY KEY,
    boy_name VARCHAR(15),
    toy_id INT,
    /*使用constraint关键字指定约束的名字，由于外键约束可以有多个，如果不指定名字，将来删除就会很麻烦*/
    /*删除外键的代码如下 drop foreign key fk_toy_id_boys_id;所有的约束都可以有名字，只不过主键唯一，有和没有都一样*/
    CONSTRAINT fk_toy_id_boys_id
    FOREIGN KEY (toy_id) REFERENCES toys (toy_id)
);

INSERT INTO toys VALUES (NULL, 'hula hoop');
INSERT INTO toys VALUES (NULL, 'balsa glider');
INSERT INTO toys VALUES (NULL, 'toy soldiers');
INSERT INTO toys VALUES (NULL, 'harmonica');
INSERT INTO toys VALUES (NULL, 'baseball cards');

INSERT INTO boys VALUES (NULL, 'Davey', 3);
INSERT INTO boys VALUES (NULL, 'Bobby', 5);
INSERT INTO boys VALUES (NULL, 'Beaver', 2);
INSERT INTO boys VALUES (NULL, 'Richie', 1);

/*使用交叉连接（也叫笛卡儿积）从多张表中选择数据,cross join 也可省略不写，使用逗号分隔*/
SELECT boys.boy_name, toys.toy_name FROM boys, toys ORDER BY boys.boy_name;

/*内连接，使用inner join，ON表示条件，也可使用where,使用等于号表示相等连接*/
/*使用不等于号相当于不等连接*/
SELECT b.boy_name, t.toy_name FROM boys AS b INNER JOIN toys AS t ON b.toy_id = t.toy_id;
SELECT * FROM boys AS b INNER JOIN toys AS t ON b.toy_id = t.toy_id;
SELECT * FROM boys AS b INNER JOIN toys AS t ON b.toy_id <> t.toy_id;

/*自然连接，natural join会自动使用相同列名的内连接，与上面的相等连接一样结果*/
SELECT boys.boy_name, toys.toy_name FROM boys NATURAL JOIN toys;

/*使用子查询，将查询的结果作为条件，比如查询喜欢玩具hula hoop的男孩*/
/*第一种方法，使用连接*/
SELECT b.boy_name name FROM toys t NATURAL JOIN boys b WHERE t.toy_name = 'hula hoop';
/*第二种方法，使用子查询*/
SELECT boy_name name FROM boys WHERE toy_id = (SELECT toy_id FROM toys WHERE toy_name = 'hula hoop');

/*子查询作为select语句选取的列,下面的结果与使用自然连接的结果相同*/
SELECT boy_name boy, (SELECT toy_name toy FROM toys WHERE boys.toy_id = toys.toy_id) AS toy FROM boys;

/*定义：如果子查询可以独立运行且不会引用外层查询的任何结果，就称为非关联子查询。显然，
上上面的查询就是非关联子查询，而上面的查询正好不属于非关联子查询。
查询效率： 交叉连接最慢，关联子查询也慢，连接通常比子查询更有效率，但想要知道每种查询具体的运行时间，还是应该做测试。*/

/*一个搭配EXISTS或者NOT EXISTS的（好用）关联子查询，比如找到不在男孩表中出现的玩具*/
SELECT toy_name toy FROM toys WHERE NOT EXISTS (SELECT * FROM boys WHERE boys.toy_id = toys.toy_id);

```

### Outer Join, Self Join, Union

```mysql

/*定义：左外连接左边的叫做左表，右边的叫做右表；右外连接相反*/
/*左外连接接受左表中的所有行，并用这些行依次与右表中行进行匹配*/
/*查询每男孩拥有的玩具,这个结果与使用内连接是一样的。*/
SELECT b.boy_name boy, t.toy_name toy
FROM boys b LEFT OUTER JOIN toys t ON b.toy_id = t.toy_id;

/*外连接主要在于连接的左右表的顺序，顺序不同，结果就会不一样,当匹配右表中每一行都没有结果是，使用NULL填充结果*/
/*注意：Inner Join中ON可用Where代替，这里外连接不可以*/
SELECT b.boy_name boy, t.toy_name toy
FROM toys t LEFT OUTER JOIN boys b ON b.toy_id = t.toy_id;

/*现在又有一种情况，假设有一个记录学校的人的表，为了简单，其中只有老师和学生，表中只有
姓名这唯一的有效信息，当id=1的人后面的teacher_id为2时，代表学生1是老师2的学生；规定老师的老师是它自己。则表的设计如下*/
CREATE TABLE person
(
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(15),
    teacher_id INT NOT NULL
);

INSERT INTO person VALUES (NULL, 'yang', 3);
INSERT INTO person VALUES (NULL, 'wei', 2);
INSERT INTO person VALUES (NULL, 'jie', 3);
INSERT INTO person VALUES (NULL, 'Jeyser', 6);

/*使用自连接，找到学生yang对应的老师*/
SELECT p1.name student, p2.name teacher FROM person AS p1 INNER JOIN person p2 ON p1.teacher_id = p2.id;

/*使用Union来获取查询结果的并集，结果中没有重复值，且列的数据类型必须相同或可以相互转换
可以使用Union All来查看所有数据*/
SELECT id FROM person UNION SELECT teacher_id FROM person;
SELECT id FROM person UNION ALL SELECT teacher_id FROM person;

/*Intersect（交集）和Except（差集）,这两个查询不在Mysql中*/
SELECT id FROM person INTERSECT SELECT teacher_id FROM person;
SELECT id FROM person EXCEPT SELECT teacher_id FROM person;

```

### Constraint, View, 事务(Transaction)

```mysql

/*Check约束，用于指定某列数据的范围，避免将来输入难以理解的值*/
/*我们可以直接在创建表时添加约束，也可以使用Alter来添加约束，比如给上面的person表*/
CREATE TABLE test_check
(
    coin char(1) check (coin in ('P', 'N', 'D', 'Q'))
);
/*之后就只能插入PNDQ的数据了，但是Check约束在mysql中会被忽略*/

/*视图View被称为虚拟表，可以做许多和表一样的操作，但其有很多好处*/
/*1）视图将复杂查询简化为一个命令 2）可以隐藏信息等等*/
CREATE VIEW boy_toy AS
SELECT * FROM toys t WHERE t.toy_id < 10;

/*比如我们为上面的外连接建立视图，之后我们就可以直接使用下面的语句查询了*/
SELECT * FROM boy_toy;
/*查看视图*/
DESC boy_toy;
SHOW TABLE boy_toy;

/*利用视图也可以进行insert，update与delete，与表的操作类似，但是一般不值得这么麻烦*/
/*虽然上面的where子句要小于10，但12是可以插进入的，要想不插入12，必须使用check option*/
INSERT INTO boy_toy VALUES (12, 'yang');

/*在mysql中，带有Check Option的视图可以实现类似于表的check constraint效果*/
CREATE VIEW boy_toy AS
SELECT * FROM toys t WHERE t.toy_id < 10 WITH CHECK OPTION;

/*定义：事务（transaction）是可完成一组工作的一系列SQL语句。*/
/*在事务过程中，如果所有步骤无法不受干扰地完成，则不该完成任何单一步骤，与OS中的原子操作类似*/
/*判断一组SQL语句是否构成一个事务的四个原则：1）原子性（atomicity） 2）一致性 （consistency）
3）隔离性（isolation） 4）持久性（durability）*/

/*想要事务功能在mysql下运作，必须采用正确的存储引擎（storage engine）。存储引擎必须是BDB或InnoDB，两种支持事务的引擎*/

/*对现在的目标而言，从两种引擎中选择任何一种都可以，改变存储引擎*/
ALTER TABLE person TYPE = BDB;

/*使用start transaction开始事务，rollback处理事务过程中发生意外回滚到开始事务的地方，
commit提交事务，事务已经完成，可以提交了，同git中的commit*/
START TRANSACTION;
SELECT * FROM person;
UPDATE person SET name = 'yang' WHERE name = 'yangweijie';
SELECT * FROM person;   /*这里能够看到改变*/
ROLLBACK;   /*回滚*/
SELECT * FROM person;   /*数据又恢复了原样，因为没有commit*/

```

### Security(Add user, Grant, Role)

```mysql

/*设置根用户root的密码*/
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('3602muyue');

/*mysql中所有用户的数据也是存在表中的，具体位置在mysql.user表中*/
/*查询所有用户*/
SELECT * FROM mysql.user\G

/*添加新用户yang,此时用户没有任何权限*/
CREATE USER yang IDENTIFIED BY '3602muyue';

/*使用Grant语句授予用户权限,给用户yang在表person上有选择权限，其他权限类似*/
/*with grant option使得当我们授予yang在person上的选择权限，则这个用户也能授予其他人这个权限*/
GRANT SELECT, INSERT ON person TO yang, Jeyser WITH GRANT OPTION;

/*也能让用户只可以拥有一列的权限,grant all授予select，update，insert，delete所有权
限，database_name.*表示授予数据表中所有表的权限*/
GRANT SELECT(boy_name) ON boys TO yang;

/*撤销权限revoke,使用From而不是to，也可以只移除with grant option。*/
REVOKE SELECT(boy_name) ON boys FROM yang;
/*用户yang和jeyser还可以进行insert，只是不能把这个权限授予他人*/
REVOKE GRANT OPTION ON INSERT ON person FROM yang, Jeyser;

/*撤销权限具有传递性，即如果此时yang给用户wei授予了insert权限，而yang的insert权限被取消，则wei的insert权限也没有了*/

/*Cascade（级联）和restrict*/
/*使用cascade移除目标用户权限，则连同被授予者权限一起移除*/
REVOKE SELECT(boy_name) ON boys FROM yang CASCADE;
/*使用restrict，如果目标用户已经将权限授予他人，删除目标用户权限会报错，因为会影响到被授予者权限*/
REVOKE SELECT(boy_name) ON boys FROM yang RESTRICT;

/*想象一下，如果有多个用户拥有相同权限，为每个用户都指定一下权限将会多么麻烦*/
/*角色（role）：把特定权限汇集成组，在把权限授予一群人的方式*/
CREATE ROLE date_entry;     /*创建角色*/
GRANT SELECT, INSERT ON person TO data_entry;       /*给角色授予权限*/
GRANT data_entry TO yang;           /*使用角色*/

/*删除角色*/
DROP ROLE data_entry;

/*查看授予某人的权限,用户名大小写敏感*/
SHOW GRANTS FOR root@localhost\G

/*将创建用户和授予权限写成一个语句，RDBMS会自动检查名字是否存在，不存在则创建*/
CREATE SELECT ON person TO yang IDENTIFIED BY '3602muyue';

/*切换用户*/
/*在命令行下进入目录C:\Program Files\MySQL\MySQL Server 8.0\bin，使用mysql -uyang -p输入密码后进入用户yang下*/

```

### 其他

```mysql

/*All Any Some*/
/*规则：它们都可以和比较运算使用，大于加上All可以找到任何大于集合中最大值的值，小于加上
All可以找到任何小于集合中最小值的值。大于加上Any找出任何大于集合中最小值的值，小于正相反
一般情况下，some的用法与Any相同。*/
SELECT name, rating FROM restaurant_ratings WHERE rating < ALL
(SELECT rating FROM restaurant_rating WHERE rating > 3 AND rating < 9);

/*更多数据类型*/
/*Boolean：布尔类型，有符号整数INT(SIGNED)，整数类型：TINYINT,SMALLINT,MEDIUMINT,
BIGINT.Date与Time类型： DATE YYYY-MM-DD     DATATIME YYYY-MM-DD HH:MM:SS    
TIMESTAMP YYYYMMDDHHMMSS    TIME HH:MM:SS*/
/*日期格式化函数DATA_FORMAT*/
SELECT DATA_FORMAT(a_date, '%M %Y') FROM some_dates;

/*创建临时表*/
CREATE TEMPORARY TABLE my_temp_table
(
    some_id INT,
    some_data VARCHAR(50)
);

/*数据类型转换cast函数*/
SELECT CAST('2005-01-01' AS DATE);
SELECT CAST(2 AS DEC);

/*获取当前用户，时间，日期*/
SELECT CURRENT_USER;
SELECT CURRENT_DATE;
SELECT CURRENT_TIME;

/*使用索引加快查找速度，为我们经常使用的列添加索引*/
ALTER TALBE my_table ADD INDEX (name);

```