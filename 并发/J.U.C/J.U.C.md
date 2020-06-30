# J.U.C

## Lock

### 1. ReentrantLock

可重入锁，内部实现公平锁`FairSync`和非公平锁`NonfairSync`，默认为非公平。

```java
// 无参构造方法，内部锁默认为非公平锁实现
public ReentrantLock() {
	sync = new NonfairSync();
}
```

![可重入锁](.\img\ReentrantLock.jpg)