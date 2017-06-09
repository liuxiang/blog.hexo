
title: MyThread.java 线程，锁，同步
date: 2017-2-16 00:00:00
tags: [线程 ]


---
- G:\liuxiang_code_git\myPro\thread\src\MyThread.java
```
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;


/**
 * Created by Administrator on 2017/2/16 0016.
 */
public class MyThread {
    /* 一.synchronized 同步方法 */
    private static M_sync m_sync = new M_sync();


    /* 二.synchronized_Block 同步方法 */
    private static Object sycnBlock_obj = new Object();


    /* 三.Lock 同步方法 */
    private static Lock lock = new ReentrantLock();
    private static ReentrantReadWriteLock rwlock = new ReentrantReadWriteLock();


    /* 四.公平锁 */
    private static Lock lock_Fair = new ReentrantLock(true);// 公平锁
    private static Lock lock_NonFair = new ReentrantLock(false);// 非公平锁(默认)


    /* 五.Volatile 理解 */
    public static volatile int count = 0;


    public static void main(String[] args) throws InterruptedException {
        threadStart("1");
        threadStart("2");


        /* 测试是非公平锁 */
        for (int i = 2; i < 10; i++) {
            threadStart(i + "");
        }


        /* 配合get_tryLock_Interrup */
//        Thread.sleep(1000);
//        thread2.interrupt();// 在一定业务时间未响应(示例1s)，将线程断开
    }


    public static Thread threadStart(String threadName) {
        Thread thread = new Thread(threadName) {
            @Override
            public void run() {
                /* 一.synchronized 同步方法 */
//                new M_sync().get();// 不能同步:synchronized方法不在一个实例中
//                m_sync.get();// 同步


                /* 二.synchronized_Block 同步方法 */
//                new M_syncBlock().get(new Object());// 不能同步:同步索对象不同(局部变量)
//                new M_syncBlock().get(sycnBlock_obj);// 同步:成员变量


                /* 三.Lock 同步方法 `Java 并发开发：Lock 框架详解 - 推酷` http://www.tuicool.com/articles/FB7fyiy */
//                new M_lock().get(new ReentrantLock());// 不能同步:同步索对象不同(局部变量)
//                new M_lock().get(lock);// 同步:成员变量
//                new M_lock().get_tryLock(lock);// 检查是否获得锁
//                new M_lock().get_tryLock_time(lock);// 一定时间内检查是否获得锁
//                new M_lock().get_lockInterrup(lock);// 主动释放索.需要配合 `Thread.sleep(1000);` `thread2.interrupt();`
//                new M_lock().get_ReadWriteLock(rwlock);// 可同时进行读操作


                /* 四.公平锁 synchronized和Lock都是非公平锁,但Lock可以设置为公平锁 */
//                new M_lock().get_lock_fair(lock_Fair);
//                new M_lock().get_lock_fair(lock_NonFair);// 非公平锁,没能感觉出来？
//                new M_lock().get_lock_fair(lock);// 非公平锁(默认),没能感觉出来？
//                new M_syncBlock().get_NonFair(sycnBlock_obj);// 非公平锁,感觉明显


                /* 五.Volatile 理解 */
//                new M_Volatile().get_NonSycn();// 反例,非同步。结果: 99967 (丢失了33.出错率 3.3/10000 万分之3)
//                new M_Volatile().get_Sycn(sycnBlock_obj);// 借助`synchronized (sycnBlock_obj)`同步 结果:10*10000=100000
//                new M_Volatile().get_doubleCheck(sycnBlock_obj);
                MyThread myThread = new M_Volatile().get_doubleCheck_NonVolatile(sycnBlock_obj);
                int count = myThread.count;// 处理器会进行指令重排序优化,有可能已经赋值了地址但未初始化实例。见:http://blog.csdn.net/glory1234work2115/article/details/50814419
            }
        };


        thread.start();
        return thread;
    }


    public static void handle() {
        handle(100);
    }


    public static void handle(int time) {
        long start = System.currentTimeMillis();
        System.out.println("线程" + Thread.currentThread().getName() + "开始读操作>>>>>>>>>>>>>>>");
        System.out.println("线程" + Thread.currentThread().getName() + "开始进行操作...");
        while (System.currentTimeMillis() - start <= time) {
        }
        System.out.println("线程" + Thread.currentThread().getName() + "开始读完毕<<<<<<<<<<<<<<<");
    }
}


class M_sync {
    public synchronized void get() {
        MyThread.handle();// 锁间处理
    }
}


class M_syncBlock {
    public void get(Object sycnBlock_obj) {
        synchronized (sycnBlock_obj) {
            MyThread.handle();// 锁间处理
        }
    }


    public void get_NonFair(Object sycnBlock_obj) {
        System.out.println("线程" + Thread.currentThread().getName() + "请求锁");
        synchronized (sycnBlock_obj) {
            MyThread.handle();// 锁间处理
        }
    }
}


class M_lock {
    public void get(Lock lock) {
        lock.lock();
        try {
            MyThread.handle();// 锁间处理
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }


    public void get_tryLock(Lock lock) {
        if (lock.tryLock()) {
            try {
                MyThread.handle();// 锁间处理
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        } else {
            System.out.println("线程" + Thread.currentThread().getName() + "获得锁失败 ××××");
        }
    }


    public void get_tryLock_time(Lock lock) {
        try {
            // 5s内如检查获得锁,会提前执行
            if (lock.tryLock(5, TimeUnit.SECONDS)) {
                try {
                    MyThread.handle();// 锁间处理
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            } else {
                System.out.println("线程" + Thread.currentThread().getName() + "获得锁失败 ××××");
            }


        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }


    public void get_lockInterrup(Lock lock) {


        try {
            lock.lockInterruptibly();// 可被`thread2.interrupt()`断开
            // lock.lock();// 不可被断开


            try {
                MyThread.handle(2000);// 锁间处理(需要处理2s时间)
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        } catch (Exception e) {
            System.out.println("线程 " + Thread.currentThread().getName() + "被中断 ×××");
        }
    }


    public void get_ReadWriteLock(ReentrantReadWriteLock rwlock) {
        rwlock.readLock().lock(); // 在外面获取锁
        try {
            MyThread.handle();// 锁间处理
        } finally {
            rwlock.readLock().unlock();
        }


        /**
         * 线程A和线程B在同时进行读操作，这样就大大提升了读操作的效率。
         * 不过要注意的是，如果有一个线程已经占用了读锁，则此时其他线程如果要申请写锁，则申请写锁的线程会一直等待释放读锁。
         * 如果有一个线程已经占用了写锁，则此时其他线程如果申请写锁或者读锁，则申请的线程也会一直等待释放写锁。
         */
    }


    public void get_lock_fair(Lock lock) {
        System.out.println("线程" + Thread.currentThread().getName() + "请求锁");
        lock.lock();
        try {
            MyThread.handle();// 锁间处理
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}


class M_Volatile {


    public static volatile MyThread myThread = null;// 加volatile
    public static MyThread myThread_NonVolatile = null;// 没加volatile(存在指令重排风险)
    public static volatile ScheduledFuture<?> logFuture = null;// ?不方便实例化


    public void get_NonSycn() {
        for (int k = 0; k < 10000; k++) {
            MyThread.count++;
            System.out.println(MyThread.count);
        }
    }


    public void get_Sycn(Object sycnBlock_obj) {
        synchronized (sycnBlock_obj) {
            for (int k = 0; k < 10000; k++) {
                MyThread.count++;
                System.out.println(MyThread.count);
            }
        }
    }


    public MyThread get_doubleCheck(Object sycnBlock_obj) {
        if (myThread == null) { // 校验:已知实例化情况,减少`synchronized`同步消耗
            synchronized (sycnBlock_obj) {
                if (myThread == null) {// 同步校验, 未 实例化再实例化
                    long start = System.currentTimeMillis();
                    while (System.currentTimeMillis() - start <= 1) {
                    }
                    myThread = new MyThread();
                    System.out.println("实例成功");
                } else {
                    System.out.println("已经实例");
                }
            }
        }
        return myThread;
    }


    public MyThread get_doubleCheck_NonVolatile(Object sycnBlock_obj) {
        if (myThread_NonVolatile == null) { // 校验:已知实例化情况,减少`synchronized`同步消耗
            synchronized (sycnBlock_obj) {
                if (myThread_NonVolatile == null) {// 同步校验,未实例化才需要实例化
                    long start = System.currentTimeMillis();
                    while (System.currentTimeMillis() - start <= 1) {
                    }
                    myThread_NonVolatile = new MyThread();
                    System.out.println("实例成功");
                } else {
                    System.out.println("已经实例");
                }
            }
        }
        return myThread_NonVolatile;
    }


}


/**
 * Java 并发开发：Lock 框架详解 - 推酷 http://www.tuicool.com/articles/FB7fyiy
 */
```