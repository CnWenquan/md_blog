---
title: Android ORM面向对象数据库
url: 99.html
id: 99
categories:
  - Android课程总结
date: 2016-07-12 19:35:57
tags:
---

1、导包

2、定义一个Student类，声明数据库的字段

    ①：注解该类代表一个Model数据库表

@DatabaseTable(tableName = "tb_student")

    ②：定义字段名称与类型

  

自增：long型  

@DatabaseField(columnName = "_id" , generatedId = true)
private long _id;

int型：
@DatabaseField(columnName = "age" , dataType = DataType.INTEGER)
private int age;

String型：
@DatabaseField(columnName = "sex" , dataType = DataType.STRING)
private String sex;


③：定义构造方法：
public Student(long _id, int age, String sex, String name) {
    this.\_id = \_id;
    this.age = age;
    this.sex = sex;
    this.name = name;
}

④：定义每个的get、set

3、写一个类，继承OrmLiteSqliteOpenHelper

    ①：定义数据库名称

private static final String DB\_NAME = "db\_students.db";

    ②：定义数据库版本号

private static final int DB_VERSION = 3;

    ③：连接数据库

public MySQLiteOpenHelper(Context context) {
    super(context, DB\_NAME, null, DB\_VERSION);
}

  

    ④：单例获取数据库工具类

MySQLiteOpenHelper getInstance(Context context) {
    (== ) {
        = MySQLiteOpenHelper(context);
    }
    ;
}

    ⑤：获取表的访问控制

Dao<Student, Long> getStudentDao() SQLException {
    (== ) {
        = getDao(Student.);
    }
    ;
}

 ⑥：重写OnCreate与OnUpdate方法

4、在MainActivity中初始化数据库连接

private void initDBHelper() {
    dbHelper = MySQLiteOpenHelper.getInstance(mContext);
    try {
        studentDao = dbHelper.getDao(Student.class);
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

5、增加数据：

Student student = new Student();

student.setName(name);
student.setAge(Integer.(age));
student.setSex(sex);

{
    count = .create(student);
    (count > ) {
        .reloadListView(seleteData(), );
    } {
        Toast.(, , Toast.).show();
    }
} (SQLException e) {
    e.printStackTrace();
}

6、查：

List<Student> seleteData() {
    {
        .queryForAll();
    } (SQLException e) {
        e.printStackTrace();
    }
    ;
}