# Spring 启动核心流程



## 启动

```java
// 声明一个基于xml配置的应用容器
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("classpath:conf/learn1/learnTest1.xml");
```

### 查看`ClassPathXmlApplicationContext`的构造函数

```java
public ClassPathXmlApplicationContext(String configLocation) throws BeansException {
    this(new String[] {configLocation}, true, null);
}

public ClassPathXmlApplicationContext(
    String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
    throws BeansException {
    // 1.设置父容器，这是一个扩展点
    super(parent);
    // 2.设置xml配置路径
    setConfigLocations(configLocations);
    if (refresh) {
        // 3.刷新容器 容器核心的流程
        refresh();
    }
}
```

#### 1.设置父容器

应用容器之间有父子关系的一个概念，这是一个spring容器的可扩展点。

#### 2.设置xml配置路径

```java
public void setConfigLocations(@Nullable String... locations) {
    if (locations != null) {
        Assert.noNullElements(locations, "Config locations must not be null");
        this.configLocations = new String[locations.length];
        for (int i = 0; i < locations.length; i++) {
            // 解析配置文件名中的通配符${}
            this.configLocations[i] = resolvePath(locations[i]).trim();
        }
    }
    else {
        this.configLocations = null;
    }
}
protected String resolvePath(String path) {
    return getEnvironment().resolveRequiredPlaceholders(path);
}

public ConfigurableEnvironment getEnvironment() {
    if (this.environment == null) {
        // 创建环境
        this.environment = createEnvironment();
    }
    return this.environment;
}

protected ConfigurableEnvironment createEnvironment() {
    // 返回默认的环境
    return new StandardEnvironment();
}
```

#### 3.刷新容器 容器核心的流程

```java
@Override
public void refresh() throws BeansException, IllegalStateException {
    synchronized (this.startupShutdownMonitor) {
        // Prepare this context for refreshing.
        //  1. 容器 刷新前准备：创建环境变量 设置应用前置监听器
        prepareRefresh();
        // Tell the subclass to refresh the internal bean factory.
        // 2. 创建 bean工厂
        ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();
        // Prepare the bean factory for use in this context.
        // 3. 对 beanFactory 进行初始化（将容器的属性放入beanFactory）
        prepareBeanFactory(beanFactory);
        try {
            // Allows post-processing of the bean factory in context subclasses.
            // 4. beanFactory的后置处理方法，默认为空方法
            postProcessBeanFactory(beanFactory);

            // Invoke factory processors registered as beans in the context.
            //5. 调用bean工厂后置处理器
            invokeBeanFactoryPostProcessors(beanFactory);

            // Register bean processors that intercept bean creation.
            // 6. 注册可用的bean后置处理器
            registerBeanPostProcessors(beanFactory);

            // Initialize message source for this context.
            // 7. 初始化消息源
            initMessageSource();

            // Initialize event multicaster for this context.
            // 8. 初始化事件广播器
            initApplicationEventMulticaster();

            // Initialize other special beans in specific context subclasses.
            // 9.初始化其他的 bean 空方法
            onRefresh();

            // Check for listener beans and register them.
            // 10. 注册监听
            registerListeners();

            // Instantiate all remaining (non-lazy-init) singletons.
            // 11.实例化单例
            finishBeanFactoryInitialization(beanFactory);

            // Last step: publish corresponding event.
            // 12. 最后的刷新收尾动作
            finishRefresh();
        }

        catch (BeansException ex) {
            if (logger.isWarnEnabled()) {
                logger.warn("Exception encountered during context initialization - " +
                            "cancelling refresh attempt: " + ex);
            }
            // Destroy already created singletons to avoid dangling resources.
            destroyBeans();

            // Reset 'active' flag.
            cancelRefresh(ex);

            // Propagate exception to caller.
            throw ex;
        }

        finally {
            // Reset common introspection caches in Spring's core, since we
            // might not ever need metadata for singleton beans anymore...
            resetCommonCaches();
        }
    }
}
```

##### 3.1 prepareRefresh() 刷新前准备

```java
protected void prepareRefresh() {
    // Switch to active.
    //设置启动时间
    this.startupDate = System.currentTimeMillis();
    // 设置 关闭标识
    this.closed.set(false);
    // 设置 激活标识
    this.active.set(true);

    if (logger.isDebugEnabled()) {
        if (logger.isTraceEnabled()) {
            logger.trace("Refreshing " + this);
        }
        else {
            logger.debug("Refreshing " + getDisplayName());
        }
    }

    // Initialize any placeholder property sources in the context environment.
    // 初始化其他的带占位符的配置文件，默认实现未空方法
    initPropertySources();

    // Validate that all properties marked as required are resolvable:
    // see ConfigurablePropertyResolver#setRequiredProperties
    // 验证所有必须的配置是否都存在
    getEnvironment().validateRequiredProperties();

    // Store pre-refresh ApplicationListeners...
    // 设置应用监听
    if (this.earlyApplicationListeners == null) {
        this.earlyApplicationListeners = new LinkedHashSet<>(this.applicationListeners);
    }
    else {
        // Reset local application listeners to pre-refresh state.
        this.applicationListeners.clear();
        this.applicationListeners.addAll(this.earlyApplicationListeners);
    }

    // Allow for the collection of early ApplicationEvents,
    // to be published once the multicaster is available...
    this.earlyApplicationEvents = new LinkedHashSet<>();
}
```

##### 3.2. obtainFreshBeanFactory() 创建bean工厂