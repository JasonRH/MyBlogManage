---
title: SQLite表升级和导入
categories: 移动端
tags:
  - Android
abbrlink: 56528
date: 2021-04-15 14:31:00
---

##### 数据库升级增加表和删除表都不涉及数据迁移，但是修改表涉及到对原有数据进行迁移。

```
public class DBservice extends SQLiteOpenHelper {
        private String CREATE_BOOK = "create table book(bookId integer
        primarykey,
        bookName text);";
        private String CREATE_TEMP_BOOK = "alter table book rename to _temp_book";
        private String INSERT_DATA = "insert into book select *,'' from _temp_book";
        private String DROP_BOOK = "drop table _temp_book";

        public DBservice(Context context, String name, CursorFactory factory, int
                version) {
            super(context, name, factory, version);
        }

        @Override
        public void onCreate(SQLiteDatabase db) {
            db.execSQL(CREATE_BOOK);
        }

        @Override
        public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
            switch (newVersion) {
                case 2:
                    db.beginTransaction();
                    db.execSQL(CREATE_TEMP_BOOK);
                    db.execSQL(CREATE_BOOK);
                    db.execSQL(INSERT_DATA);
                    db.execSQL(DROP_BOOK);
                    db.setTransactionSuccessful();
                    db.endTransaction();
                    break;
            }
        }
```

##### 打开外部数据库
```
public class DatabaseManager {
    private DaoMaster mDaoMaster = null;
    private DaoSession otherDaoSession = null;

    private DatabaseManager() {
    }

    private static final class Holder {
        private static final DatabaseManager INSTANCE = new DatabaseManager();
    }

    public static DatabaseManager getInstance() {
        return Holder.INSTANCE;
    }

    //获取外部其它数据库DaoSession
    DaoSession getOtherDaoSession(String dbName) {
        //if (otherDaoSession == null) {
            otherDaoSession = getDaoMaster(dbName).newSession();
        //}
        return otherDaoSession;
    }

    private DaoMaster getDaoMaster(String dbName) {
        if (dbName == null || TextUtils.isEmpty(dbName)) {
            return null;
        }
        //if (mDaoMaster == null) {
            //mDaoMaster = new DaoMaster(new DaoMaster.DevOpenHelper(context, dbName).getWritableDb());
            mDaoMaster = new DaoMaster(SQLiteDatabase.openDatabase(dbName, null, SQLiteDatabase.OPEN_READWRITE));
        //}
        return mDaoMaster;
    }

}
```