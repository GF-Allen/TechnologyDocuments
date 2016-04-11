### Java 锁

1. synchronized 关键字
	1. 加在方法中，锁根据当前类对象，synchronized(this)

2. Lock对象锁（java.util.concurrent.locks.Lock）
	1. lock.lock()
	2. lock.unlock()