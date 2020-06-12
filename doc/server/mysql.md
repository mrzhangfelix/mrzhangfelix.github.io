# Mysql
## 设置时区
```mysql
SELECT TIMEDIFF(NOW(), UTC_TIMESTAMP); 
select curtime();  
show variables like "%time_zone%";

set global time_zone = '+8:00';  ##修改mysql全局时区为北京时间，即我们所在的东8区
set time_zone = '+8:00';  ##修改当前会话时区
flush privileges;  #立即生效
```

jdbc.url=jdbc:mysql://localhost:3306/demo?serverTimezone=Asia/Shanghai&characterEncoding=utf-8 
