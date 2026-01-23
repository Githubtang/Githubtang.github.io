---
title: "Spring"
description: "修改 hosts 修复"
date: 2026-01-23
draft: false
tags: ["Bean", "Aop", "Ioc","事务"]
---

## Bean 的生命周期

1. **实例化**
    - 通过反射去推断构造函数进行实例化
    - 实例工厂、静态工厂
2. **依赖注入**
    - 解析自动装配(byname bytype constractor none @Autowired)
3. **初始化**
    - 调用很多**Aware**回调方法 
    
    (主要有BeanNameAware BeanFactoryAware ApplicationContextAware)
    
    - 调用BeanPostProcessor.postProcessBeforeInitialization 前置处理器
    - 调用生命周期回调初始化方法
    - 调用BeanPostProcessor.postProcessAfterInitialization,如果bean实现aop则会在这里创建动态代理
4. **销毁**
    - 在spring容器关闭的时候进行调用
    - 调用生命周期回调销毁方法


