---
title: Day11
url: 72.html
id: 72
categories:
  - Android课程总结
date: 2016-05-27 21:29:52
tags:
---

Android11 总结
------------

**摘要**：Android中数据持久化常用的路径、SQL相关知识、SQLiteDatabase的使用、SQLiteOpenHelper

**Android中数据持久化常用路径：**

A、内部存储容器：data/data/包名

    1、内部缓存目录：data/data/包名/cache  

    2、内部文件目录：data/data/包名/files  

    3、默认数据库：data/data/包名/databases  

    4、sharedPreferences目录：data/data/包名/shared_prefs  

B、外部存储容器(SD卡)

    1、外部私有目录：storage/sdcard/Android/data  

        ①：外部私有缓存目录：storage/sdcard/Android/data/包名/cache  

        ②：外部私有文件目录：storage/  

    2、外部公有目录：  

        storage/sdcard/内置9个公有目录  

    3、SD卡中自定义目录  

**SQL相关知识**

1、create database XXX:创建数据库

2、create table table_name(指定主键,XXX,XXX)（SQLite可以不定义数据类型）

3、insert into table_name(X,    X,    X) values(X,    X,    X)插入一条数据

4、delete table_name where(?)    删除数据

5、update table_name set XXX = XXX where ?     修改数据

6、select XXX, from table_name where ?    group by ? Having ? Order by ? limit ?    :查询数据

右边是图片

绝对没有问题的，不信你也可以试试看

  
  

**SQLiteDatabase:(读取本地数据库文件)**

1、初始化数据库对象：  

String dbPath = SDCardHelper.getSDCardPrivateFilesDir(mContext, Environment.DIRECTORY_DOCUMENTS) + File.separator + "android_manual.db";

dbConn = SQLiteDatabase.openDatabase(dbPath, null, SQLiteDatabase.OPEN_READWRITE);

        2、增加数据：  

String sql = "insert into android_basic (id , title , content , importance , showFlag , version) " +

 "values(? , ? ,?  , ? , ? , ? )";

dbConn.execSQL(sql, new Object\[\]{maxid + 1, title, content, 1, 1, 1});

        3、删除：

dbConn.execSQL("delete from android_basic where id=?", new Object\[\]{id});

  

        4、修改：  

dbConn.execSQL("update android_basic set title=? , content=? where id=?",

 new Object\[\]{editText\_dialog\_title.getText() + "",

editText\_dialog\_content.getText() + "", id});

        5、查询：

dbConn.rawQuery("select max(id) from android_basic order by id desc", null);

  

  

**SQLiteOpenHelper:(可以创建数据库和更新数据库)**

1、getReadableDatabase():当存储满了，变为只读数据库

      getWriteableDatabase():当存储满了，再往里面存储，报错  

2、封装一个SQLiteOpenHelper工具类

1.      继承SQLiteOpenHelper父类，实现构造方法，连接数据库，创建数据库对象
    

public MySQLiteOpenHelper(Context context) {
        super(context, DB\_NAME, null, DB\_VERSION);
        dbConn = getReadableDatabase();
    }

    2、重写onCreate方法，在方法中创建数据表：  

@Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE IF NOT EXISTS tb_words
        (_id INTEGER PRIMARY KEY AUTOINCREMENT,word,detail)");
    }

    3、升级版本：  

@Override

 public  void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {

 if (newVersion > oldVersion) {

            db.execSQL("DROP TABLE IF EXISTS tb_words");

            onCreate(db);

        }

    }

      

    4、实现数据的删、改、增  

public  boolean execData(String sql, Object\[\] bindArgs) {

 try {

 if (bindArgs == null) {

                dbConn.execSQL(sql);

} else {

                dbConn.execSQL(sql, bindArgs);

            }

 return  true;

} catch (Exception e) {

Log.i(TAG , "-->>" \+ e.toString());

 return  false;

        }

    }

  

5、执行查询返回Cursor

public Cursor selectCursor(String sql, String\[\] selectionArgs) {

 return  dbConn.rawQuery(sql , selectionArgs);

    }

  

6、    如果sql语句是select count(*) from table_name ---->>返回count

public  int selectCount(String sql, String\[\] selectionArgs) {

        Cursor cursor = dbConn.rawQuery(sql, selectionArgs);

        cursor.moveToFirst();

 int count = cursor.getInt(0);

 if (cursor != null) {

            cursor.close();

        }

 return count;

    }

  

7、如果sql语句是select id from table_name --->>返回总数

public  int selectCount2(String sql, String\[\] selectionArgs) {

        Cursor cursor = dbConn.rawQuery(sql, selectionArgs);

 int count = cursor.getCount();

 if (cursor != null) {

            cursor.close();

        }

 return count;

    }

  

  

8、查询数据库的表：（返回LIst集合）

  

public List<Map<String , String>> selectList(String sql, String\[\] selectionArgs) {

        Cursor cursor = dbConn.rawQuery(sql, selectionArgs);

 return cursorToList(cursor);

    }

  

  

9、查询数据，并且确信只返回一条数据（返回Map）

public  Map<String , String> selectMap(String sql, String\[\] selectionArgs) {

Map<String ,String > map = new HashMap<>();

        Cursor cursor = dbConn.rawQuery(sql, selectionArgs);

        cursor.moveToFirst();

 for (int i = 0; i < cursor.getColumnCount(); i++) {

            map.put(cursor.getColumnName(i) , cursor.getString(i));

        }

 if (cursor!=null) cursor.close();

 return map;

    }

  

10、将Cursor转换成List集合：

public List<Map<String , String>> cursorToList(Cursor cursor) {

List<Map<String , String>> list  = new ArrayList<>();

 while (cursor.moveToNext()) {

Map<String , String> map = new HashMap<>();

 for (int i = 0; i < cursor.getColumnCount(); i++) {

                map.put(cursor.getColumnName(i)  , cursor.getString(i));

            }

            list.add(map);

        }

 if (cursor!= null)  cursor.close();

 return list;

    }