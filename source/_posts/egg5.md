---
title: egg 简易例子4之单元测试
date: 2020-12-07 14:35:01
categories: node后端框架
tags: 
- egg
---


```
测试文件都在test目录下，里面文件结构同app目录结构
eg: app/controller => test/app/controller
```

egg-bin: [https://www.npmjs.com/package/egg-bin](https://www.npmjs.com/package/egg-bin)

**service 测试**

```
# test/app/service/users.test.js


"use strict";
const { app, mock, assert } = require("egg-mock/bootstrap");

describe("test/app/service/users.test.js", () => {
    describe("findAll()", () => {
        // 获取所有用户信息
        it("should get exists users", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.findAll();
            assert(res);
        });
    });

    describe("findOne(id)", () => {
        // 根据存在的id查询用户信息
        it("should get exists user By id", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.findOne(1);
            assert(res);
        });
        // 用不存在的用户id查询
        it("should get null when user not exists", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.findOne(600);
            assert(!res);
        });
    });

    describe("add(params)", () => {
        // 添加一个用户信息
        it("should add users", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.add({
                name: new Date().toISOString().substr(10),
                roleId: 3
            });
            assert(res);
        });
    });

    describe("delete(id)", () => {
        // 删除一个指定id的用户信息
        it("should delete users", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.delete(46);
            assert(res);
        });
    });

    describe("update(id, params)", () => {
        // 修改一个指定id的用户信息
        it("should update users", async () => {
            const ctx = app.mockContext();
            const res = await ctx.service.users.update(45, {
                name: "鹤旋",
                password: "222222",
                status: "1"
            });
            assert(res);
        });
    });

});
```

**controller测试**

```
# test/app/controller/users.test.js


"use strict";

const { app, mock, assert } = require("egg-mock/bootstrap");

describe("test/app/controller/users.test.js", () => {
  // 查询所有用户
  it("should get /api/v1/users 200", () => {
    return app
      .httpRequest()
      .get("/api/v1/users")
      .expect("Content-Type", /json/)
      .expect(200)
      .then(res => {
        assert(res);
        assert(res.text);
        assert(JSON.parse(res.text));
      });
  });

  // 查询指定id的用户
  it("should get /api/v1/users/1 200", () => {
    return app
      .httpRequest()
      .get("/api/v1/users/1")
      .expect(200); // 期望返回 status 200
  });

  // 添加用户
  it("should post /api/v1/users 200", () => {
    return app
      .httpRequest()
      .post("/api/v1/users")
      .send({
        name: new Date().toISOString().substr(10),
        roleId: 3
      })
      .expect(200); // 期望返回 status 200
  });

  // 删除用户
  it("should del /api/v1/users/5 200", () => {
    return app
      .httpRequest()
      .del("/api/v1/users/5")
      .expect(200); // 期望返回 status 200
  });

  // 修改指定id的用户信息
  it("should update /api/v1/users 200", async () => {
    await app
      .httpRequest()
      .put("/api/v1/users/1")
      .send({
        name: "John",
        password: "222222",
        status: "1"
      })
      .expect(200); // 期望返回 status 200
  });
});
```

**extend 测试**

```
# test/app/extend/helper.test.js

"use strict";
const { app, assert } = require("egg-mock/bootstrap");

describe("test/app/extend/helper.test.js", () => {
    it("should formatTime(1579154539350) === '2020-01-16 14:02:19'", () => {
        const ctx = app.mockContext();
        assert(ctx.helper.formatTime(1579154539350) === '2020-01-16 14:02:19');
    })

    it("should success()", () => {
        const ctx = app.mockContext();
        // 该方法没有返回值，故先执行，然后查看ctx.body判断是否有效
        ctx.helper.success(ctx, null, 'xxxx');
        assert(ctx.body);
        assert(ctx.body = { code: 1, data: null, msg: 'xxxx' })
    })

})


```

## 参考

**代码测试**：

* 提供测试框架\(Mocha, Jasmine, Jest, Cucumber\)

* 提供断言\(Chai, Jasmine, Jest, Unexpected\)

* 生成，展示测试结果\(Mocha, Jasmine, Jest, Karma\)

* 快照测试\(Jest, Ava\)

* 提供仿真\(Sinon, Jasmine, enzyme, Jest, testdouble\)

* 生成测试覆盖率报告\(Istanbul, Jest, Blanket\)

* 提供类浏览器环境\(Protractor, Nightwatch, Phantom, Casper\)