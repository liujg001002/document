一个表最多16个索引,最大索引长度256字节，mysql8.0本地测试（varchar(191)），
索引一般不明显影响插入性能（大量小数据例外），因为建立索引的时间开销是O(1)或者O(logN)

###1. 函数（function）
> 函数只会返回一个值(储存过程返回结果集)

* 用法：
> *出现1418错误时  
*set global log_bin_trust_function_creators = 1;  
*start slave;
*DELIMITER // 为转译
DELIMITER //--创建函数  
CREATE FUNCTION select_user(val_1 int,val_2 int)  
RETURNS BIGINT(20)  
BEGIN  
	RETURN 10;  	
END;  
//  
DELIMITER ;    
//-----------------------  
DELIMITER //--创建函数  
CREATE FUNCTION select_user(val_1 int,val_2 int)  
RETURNS BIGINT(20)  
BEGIN  
	DECLARE count_num BIGINT;  
	select count(1) into count_num from scm_user where approve = val_1 and user_type = val_2;  
	RETURN count_num;  
END;  
//  
DELIMITER ;  
select  select_user(1,1);#执行方法   
drop function select_user;#删除函数

###2. 储存过程（PROCEDURE）
>DELIMITER // --创建过程  
 CREATE PROCEDURE select_user_p(in val_1 int,in val_2 int,out val varchar(20))  
 BEGIN  
 	DECLARE sum_num int;  
 	set sum_num = val_1 + val_2;  
 	if sum_num < 100 THEN  
 		set val = 'unpass';  
 	ELSEIF sum_num = 100 then   
 		set val = 'pass';  
 	ELSE  
 		set val = 'good';  
 	end if;  
 	select val;  
 END;  
 //  
 DELIMITER ;  
 set @name = null;#设置输出变量  
 CALL select_user_p(100,0,@name);#执行过程  
 select @name from dual #查询变量值  
 drop PROCEDURE select_user_p;#删除过程  