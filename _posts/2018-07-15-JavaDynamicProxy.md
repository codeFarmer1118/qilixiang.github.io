---
layout:     post
title:      动态代理
subtitle:   DynamicProxy
date:       2018-07-15
author:     Static
header-img: img/bg/black.jpg
catalog: true
tags:
    - Java
---

### 一. 代理模式

**组成：**

- 抽象角色：通过接口或抽象类声明真实角色实现的业务方法。

- 代理角色：实现抽象角色，是真实角色的代理，通过真实角色的业务逻辑方法来- 实现抽象方法，并可以附加自己的操作。

- 真实角色：实现抽象角色，定义真实角色所要实现的业务逻辑，供代理角色调用。

### 二. Java 实现动态代理

#### 1. Java原生的动态代理：java.lang.reflect.Proxy

#### 2. Guava简单封装了Java原生的代理：com.google.common.reflect.Reflection

#### 3. Cglib通过修改字节码的方式实现动态代理

> 三种方式代码如下：

```java
import com.google.common.reflect.Reflection;
import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;

import java.lang.annotation.*;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * Created by wangzhx on 2018/7/15.
 */
public interface DynamicProxy {

    /**
     * 通过注解设置车的速度
     */
    @Target(ElementType.METHOD)
    @Retention(RetentionPolicy.RUNTIME)
    @Inherited
    @interface Speed {
        double value() default 212.1;
    }

    /**
     * 接口
     */
    interface Car {
        String run(String carMessage);
    }

    /**
     * 实际类
     */
    class Lamborghini implements Car {

        @Speed(333.33)
        @Override
        public String run(String carMessage) {
            return carMessage;
        }
    }

    /**
     * 代理类
     */
    class LamborghiniProxy {
        private Car car = new Lamborghini();
        private static final String RUNMETHOD = "run";

        /**
         * Java原生写法
         * @param clazz
         * @return
         */
        Car getProxy(Class clazz) {
            return (Car) Proxy.newProxyInstance(Car.class.getClassLoader(), Lamborghini.class.getInterfaces(),
                    /**
                     * proxy 要代理的对象
                     * method 代理对象的方法
                     * args 代理对象的方法中的入参
                     */
                    (proxy, method, args) -> {

                        //代理的method 不会将方法上的注解代理过来
                        Method runMethod = clazz.getMethod(RUNMETHOD, String.class);
                        Speed speed = runMethod.getDeclaredAnnotation(Speed.class);
                        if (speed != null) {
                            if (args.length == 1 && args[0] instanceof String) {
                                args[0] += String.valueOf(speed.value());
                            }
                        }
                        return method.invoke(car, args);

                    });
        }

        /**
         * Guava封装了Java原生的代理
         * @param clazz 接口
         * @return
         */
        Car getProxyWhithGoogle(Class<Car> clazz) {
            return Reflection.newProxy(clazz, (proxy, method, args) -> {
                Method runMethod = clazz.getDeclaredMethod(RUNMETHOD, String.class);
                Speed speed = runMethod.getDeclaredAnnotation(Speed.class);
                if (speed != null) {
                    if (args.length == 1 && args[0] instanceof String) {
                        args[0] += String.valueOf(speed.value());
                    }
                }

                return method.invoke(car, args);
            });
        }

        /**
         * Cglib通过修改字节码的方式实现代理，比Java原生的更强大
         * @return
         */
        Car getProxxyWithCglib() {
            return (Car) Enhancer.create(Lamborghini.class,
                    (MethodInterceptor) (proxy, method, args, methodProxy) -> {
                        Speed speed = method.getDeclaredAnnotation(Speed.class);
                        if (speed != null) {
                            if (args.length == 1 && args[0] instanceof String) {
                                args[0] += String.valueOf(speed.value());
                                return methodProxy.invokeSuper(proxy, args);
                            }
                        }
                        return method.invoke(car, args);
                    }
            );
        }
    }
}

```

> 现在很多的框架都用到了动态代理，比如Spring AOP的实现，底层就是判断若是接口则用Java动态代理，若是类则用Cglib动态代理