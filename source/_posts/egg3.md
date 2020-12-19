---
title: egg 简易例子2,添加mysql支持
date: 2020-12-07 14:07:37
categories: node后端框架
tags: 
- egg
---

## egg添加mysql数据库支持

1、安装对应插件egg-mysql
```sh
npm i --save egg-mysql
```

2、开启插件

```sh
 cd config
 touch plugin.js
```

> config/plugin.js

```js
exports.mysql = {
  enable: true,
  package: 'egg-mysql',
};
```

3、开发环境，数据源配置

```sh
 cd config
 touch config.default.js
```

> config.default.js

```js
module.exports = appInfo => {
    const config = (exports = {});

    // use for cookie sign key, should change to your own and keep security
    config.keys = appInfo.name + "_1545363397110_3339";

    // add your config here
    config.middleware = [];

    config.security = {
        csrf: {
            enable: false
        }
    };

    config.mysql = {
        // 单数据库信息配置
        client: {
            // host
            host: "localhost",
            // 端口号
            port: "3306",
            // 用户名
            user: "root",
            // 密码
            password: "usbw",
            // 数据库名
            database: "egg"
        },
        // 是否加载到 app 上，默认开启
        app: true,
        // 是否加载到 agent 上，默认关闭
        agent: false
    };

    return {
        ...config
    };
};
```

4、在本地新建mysql数据库egg,在数据库中新增users表，如下相关sql：

```sql
 -- 如果存在users表则删除
        drop table if exists users;
        -- 创建users表
        create table users(
            userId int unsigned auto_increment, -- 用户编号,主键，并且自增
            name varchar(25) unique not null, -- 用户姓名(保证学生名字不可以重复)
            password varchar(25) not null default 123456, -- 用户密码
            createDate timestamp not null default current_timestamp, -- 用户创建时间
            status char(1) not null default 1,  -- 用户状态,1正常，0异常，默认为正常状态
            roleId int unsigned not null default 3, -- 用户角色类型，1：超级管理员，2：VIP用户，3：普通用户，默认为普通用户
            primary key (userId)
        );
      -- 给users表添加外键roleId
        -- alter table 需加外键的表 add constraint 外键名(不能重复) foreign key(需加外键表的字段名) referencnes 关联表名(关联字段名);
        alter table users add constraint fk_users_roles foreign key (roleId) references roles (roleId);
      -- 插入测试数据到users表
        insert into users(name,roleId) values ('张三',3);
        insert into users(name,roleId) values ('李四',3);
        insert into users(name,roleId) values ('王五',3);
      -- 如果存在roles表则删除
      drop table if exists roles;
      -- 创建roles表
      create table roles(
         roleId int unsigned auto_increment, -- 用户角色ID,主键，并且自增
         roleName varchar(10) not null, -- 用户角色名称
         roleType char(1) unique not null, -- 用户角色类型
           primary key (`roleId`)
      );

      -- 插入测试数据到roles表
      insert into roles(roleName,roleType) values ('超级管理员',0);
      insert into roles(roleName,roleType) values ('VIP用户',1);
      insert into roles(roleName,roleType) values ('普通用户',2);
```

















