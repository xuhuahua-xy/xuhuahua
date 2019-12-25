git1

git配置用户名和邮箱

git config --global user.name
git config --global user.email
查看git 配置
git config --list
删除用户信息

git config --global --unset user.name 



git log 查看最近到最远的提交日志，
git log --pretty=oneline //只显示commit 编号和备注
版本回退

git reset --hard HEAD^ // 回滚到上一次版本
git reset --hard HEAD^^ //回滚到上上一次版本	
git reset --hard commitid //找到某次提交的ID 就回滚到那一个版本


git diff HEAD 文件 //查看工作区和版本库里这个文件的区别

撤销和修改

没有添加到暂存区可以直接 git checkout 文件名 可以撤销修改

如果添加到暂存区 的话 先撤销暂存区的修改再再撤销修改

撤销暂存区的修改 git reset HEAD 文件名

如果手动误删 ，同样使用git reset HEAD  文件名 来撤销删除


从仓库中删除文件

git rm 文件名

然后将删除操作提交到仓库

git commit



# oracle 

## 1.增删改表空间和用户

### 	创建表空间

​		create tablespace 表空间名 
		datafile'位置'
		size 50m

### 	修改表空间	

​		alter tablespace 表空间名 add '位置'

### 	删除表空间

​		drop tablespace 表空间名字 including contents and datafiles

### 	创建用户

​		create user 用户名
		identified by 123456
		default tablespace '默认表空间'
		在创建用户之后必须赋予权限才能操作数据库
		//赋予权限  connenct 连接权限 resource 操作数据权限 dba 管理员权限
		grant connenct ,resource to user
		//撤销权限
		revoke connenct resource from user
	

## 2.库表和表约束的增删改

	修改库表
		//添加列
		alter table 表名 add (多个条件)/条件
		//修改列
		alter table 表名 modify（）
		//删除列
		alter table 表明 drop column 列明
	添加约束
		alter table 表名 add 约束名 comstraint 约束名 约束说明
		例：
		添加主键约束
			ALTER TABLE dept ADD COMSTRAINT teacher_PK PRIMARY KEY (deptid)
		
		添加唯一约束
			ALTER TABLE teacher ADD CONSTRAINT teacher_name_UQ UNIQUE(tname)
		
		添加检查约束
			ALTER TABLE teacher ADD CONSTRAINT gender_CK CHECK(gender = '男' OR gender='女'
			
		添加外键约束
			ALTER TABLE student ADD CONSTRAINT grade_id_FK FORFIGN KEY(grade_id) REFERENCES grade(grade_id);
	删除约束
			alter table 表明 drop constraint 约束名
			例
			ALTER TABLE teacher DROP CONSTRAINT SYS_C0011075;

## 3.表数据的增删改

	添加数据
		insert into 表明 values(按照表对应的列进行排序)
		
	修改数据
		update table set **** where ****
	删除数据
		delete from  表明 where *****
## 4.表的查询

​		

	分组
		聚集函数 max() ,min() avg(), count()
		group by 按照列的值进行分组
		having 筛选分组后的数据
	排序
		order by 按照某一列的规制进行排序 ，子句位于查询的最后 asc desc
	表连接
		内连接
		外连接
		全连接
		自然连接
	函数
		
	子查询
		例
		 --3. 查询SCOTT部门员工中工资最低的员工
		 --第一步：找到SCOTT部门的部门编号（发现部门编号是20）
		 
		  SELECT deptno FROM scott.emp WHERE ename='SCOTT'
		  
		 --第二步：找到SCOTT部门的最低的工资,（发现最低工资是800）
		 
		 SELECT MIN(sal) FROM scott.emp WHERE deptno=20
		 
		 --第三步：找到最低工资（800）和deptno为20（即scott部门）的员工编号
		 
		 SELECT * FROM scott.emp WHERE sal=800 AND deptno=20
		 
		 --最后sql为：
		 
		  SELECT * FROM scott.emp
		  WHERE sal=( SELECT MIN(sal) FROM scott.emp 
		  WHERE deptno=(SELECT deptno FROM scott.emp 
		  WHERE ename='SCOTT')) 
		  AND
		  deptno=( SELECT deptno FROM scott.emp WHERE ename='SCOTT')
	
	分页
		三层嵌套
			select * 
			from 
			(select e.* ,rownum r from (select * from emp )e where rownum<page*pageSize)
			where r>(page-1)*pageSize+1
		二层嵌套
			select * 
			from
			(select e*，rownum r from emp where rownum<page*pageSize)
			where r>(page-1)*page*pageSize+1
	
	索引

你好