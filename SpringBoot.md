SpringBoot

```java
@Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        for (ApplicationListener<?> listener : this.application.getListeners()) {
            if (listener instanceof ApplicationContextAware) {
                //实现了 ApplicationContextAware 就能获得上下文的原因
                ((ApplicationContextAware) listener).setApplicationContext(context);
            }
            context.addApplicationListener(listener);
        }
        this.initialMulticaster.multicastEvent(
                new ApplicationPreparedEvent(this.application, this.args, context));
    }
```

##### ApplicationContextInitializer

ApplicationContextInitializer接口的作用就是在spring prepareContext的时候做一些初始化工作

##### SpringApplicationRunListener

主要就是在springboot 启动初始化的过程中可以通过SpringApplicationRunListener接口回调来让用户在启动的各个流程中可以加入自己的逻辑

##### ApplicationListener 

能够用来监听事件，典型的观察者模式