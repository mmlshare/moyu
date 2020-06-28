# J.U.C

## Lock

### 1. ReentrantLock

可重入锁，内部实现分公平锁和非公平锁，默认为非公平。

```java
public ReentrantLock() {
	sync = new NonfairSync();
}
```





