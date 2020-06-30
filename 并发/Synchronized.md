# Synchronized

## 1. 有什么用？

synchronized能保证某一代码块在同一时刻只能有一个线程进入临界区。

java中每一个对象都可以作为锁，这是synchronized实现同步的基础。

1. 普通同步方法：锁是当前实例对象（`this`）。
2. 静态同步方法：锁是当前类的`class`对象。
3. 同步代码块：锁是括号里的对象。

```java
public class SyncTest {

    public synchronized void synchronizedTest1(){
        System.out.println("普通方法 synchronized，锁的是当前对象 this");
    }

    public static synchronized void  synchronizedTest2(){
        System.out.println("静态方法 synchronized，锁的是当前实例的class");
    }

    public void  synchronizedTest3(){
        synchronized (this){
            System.out.println("同步代码块 synchronized，锁的是括号中的对象");
        }
    }
}
```

## 2. synchronized的底层原理

1. 同步代码块

   通过monitorenter指令和monitorexit指令实现，在同步代码块开始位置插入monitorenter指令，在同步代码块结束位置插入monitorexit指令。`JVM`保证monitorenter和monitorexit是成对出现的，任意一组指令都有一个monitor和它关联，当前线程执行到monitorenter指令时，首先会尝试获取monitor对象的锁。

2. 同步方法

   synchronized方法时在Class文件的方法表中将该方法的accessflag字段中的synchronized标志置为1。

## 3.synchronized优化


1. 自旋锁

   线程的阻塞和唤醒需要频繁地在用户态和核心态之间切换，这很影响机器的性能，在很多场景，线程对锁只持有很短的时间，在这个场景下频繁地切换状态对cpu资源造成浪费。自旋就是给予一定地尝试次数或重试时间，当达到这个上限再将线程挂起。

2. 适应性自旋锁

   当某个线程获取到锁后，将提高它下次自旋的上限次数，相反降低下次自旋的上限次数。

3. 锁消除

   JVM通过逃逸分析，检测到某块代码不存在临界区竞争，会对同步锁清除。

   ```java
   public void clearLock(){
           // vector操作方法都加上了同步锁，由于list只存在局部代码块中，所以不存在临界区竞争，JVM对其进行锁消除
           Vector<Integer> list = new Vector<>();
           for(int i=0;i<10;i++){
               list.add(i);
           }
       }
   ```

4. 锁粗化

   ```java
    public void lockPlus(){
       	//锁粗化是将多个连续的加锁，解锁扩展为范围更大的锁
           List<Integer> list = new ArrayList<>();
           for(int i=0;i<10;i++){
               synchronized (this){
                   list.add(i);
               }
           }
       }
   ```

   

5. 偏向锁

6. 轻量锁