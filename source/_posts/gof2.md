---
title: 设计模式2-基本原则
date: 2020-08-25 15:18:42
categories:
- 设计模式
tags:
- Javascript
- Java
---

## 设计原则

原则名称  | 描述
-----    | ----
开闭原则（Open Closed Principle，OCP）  | 当应用的需求改变时，在不修改软件实体的源代码或者二进制代码的前提下，可以扩展模块的功能，使其满足新的需求。
里氏替换原则（Liskov Substitution Principle，LSP） | 主要阐述了有关继承的一些原则，也就是什么时候应该使用继承，什么时候不应该使用继承，以及其中蕴含的原理。里氏替换原是继承复用的基础，它反映了基类与子类之间的关系，是对开闭原则的补充，是对实现抽象化的具体步骤的规范。
依赖倒置原则（Dependence Inversion Principle，DIP）| 高层模块不应该依赖低层模块，两者都应该依赖其抽象；抽象不应该依赖细节，细节应该依赖抽象。其核心思想是：要面向接口编程，不要面向实现编程。实现开闭原则的重要途径之一，它降低了客户与实现模块之间的耦合。
单一职责原则（Single Responsibility Principle，SRP）又称单一功能原则 | 一个类应该有且仅有一个引起它变化的原因，否则类应该被拆分。
接口隔离原则（Interface Segregation Principle，ISP）| 要为各个类建立它们需要的专用接口，而不要试图去建立一个很庞大的接口供所有依赖它的类去调用。
迪米特法则（Law of Demeter，LoD）又叫作最少知识原则（Least Knowledge Principle，LKP) | 如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。其目的是降低类之间的耦合度，提高模块的相对独立性。
合成复用原则（Composite Reuse Principle，CRP）又叫组合/聚合复用原则（Composition/Aggregate Reuse Principle，CARP）| 在软件复用时，要尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现。如果要使用继承关系，则必须严格遵循里氏替换原则。合成复用原则同里氏替换原则相辅相成的，两者都是开闭原则的具体实现规范。





