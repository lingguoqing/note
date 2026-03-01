- statis 区分实例方法



# synchronized

- **实现原理**依赖于JVM的**Monitor**（监视器锁）和**对象头**（Object Header）

### 修饰方法

- ACC SYNCHRONIZED 

### 修饰代码块

- 会在代码块前后加上Monitorenter（加锁） 和Monitorexit（解锁）

