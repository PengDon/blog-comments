---
title: nextjs+typescript 
date: 2019-11-14 20:11:27
categories: 
- React-SSR
tags: 
- nextjs
---

### 基础

#### 1. 先创建一个最基础的Nextjs项目
```bash
    mkdir next-ts
    cd next-ts
    npm init -y
    npm install --save react react-dom next
    mkdir pages
```
#### 2. 添加Typescipt和@types相关依赖
```bash
    npm install --save-dev typescript @types/react @types/node
```
#### 3. 修改next-ts目录下package.json文件中scripts属性的内容
```bash
    "scripts": {
        "dev": "next",
        "build": "next build",
        "start": "next start"
    }
```
#### 4. 在pages目录下创建index.tsx文件，文件内容如下：
```bash
   const Home = () => <h1>Nextjs typescript!</h1>;
   export default Home;
```
#### 5. 启动dev server查看效果
```bash
    npm run dev
```

