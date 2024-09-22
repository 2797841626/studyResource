`@Insert(value = "INSERT INTO user(username,password,phone) " + "values (#{username},#{password},#{phoneNumber})")`
上语句中 user()中的变量名需要与数据库中的名字一样
values()中的变量名需要与实体类中的一样