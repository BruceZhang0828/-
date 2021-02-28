### 05sleep()方法和wait()方法

#### sleep（）方法

Thread类中的静态方法，线程会暂时让出指定时间执行权，线程进入timed_waiting;

```java
Thread.sleep(10);
```



#### wait() 方法

Object中的方法；多线程中的锁放弃执行的权利。应为锁是Object，所以这个服务写到了Object类中。但是如果当前线程不是object的monitor（jvm锁实现的）的持有者，那么会抛出异常*IllegalMonitorStateException*  。



#### sleep（）方法和wait（）方法的对比

1. 两者主要的区别的在于: sleep()不会释放锁，而wait（）方法释放了锁。
2. sleep是Thread的方法，wait()方法是Object的。
3. 两者都是暂停线程的执行。
4. wait（）通常被用户线程之间交互/通信，sleep（）通常被用于暂停执行。
5. wait（）方法被调用后，线程不会自动苏醒，需要别的线程调用同一个而对象上的notify（）或者notifyAll（）方法。sleep（）方法执行完成后，线程会自动苏醒，或者可以使用wait（long）超时后线程会自动苏醒。