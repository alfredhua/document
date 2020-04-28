CyclicBarrier可以理解为循环栅栏。栅栏就是一种障碍物，比如，通常在私人宅邸的周围就可以围上一圈栅栏，阻止闲杂人等入内。这里当然就是用来阻止线程继续执行，要求线程在栅栏处等待。前面Cyclic意为循环，也就是说这个计数器可以反复使用。比如，假设我们将计数器设置为10，那么凑齐第一批10个线程后，计数器就会归零，然后接着凑齐下一批10个线程，这就是循环栅栏内在的含义。

```java

import java.util.concurrent.CyclicBarrier;

public class CycliBarrierDemo extends Thread{
    @Override
    public void run() {
        System.out.println("开始进行数据分析");
    }
    //循环屏障
    //可以使得一组线程达到一个同步点之前阻塞.
    public static void main(String[] args) {
        CyclicBarrier cyclicBarrier=new CyclicBarrier
                (3,new CycliBarrierDemo());
        new Thread(new DataImportThread(cyclicBarrier,"file1")).start();
        new Thread(new DataImportThread(cyclicBarrier,"file2")).start();
        new Thread(new DataImportThread(cyclicBarrier,"file3")).start();
    }
}


import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class DataImportThread extends Thread{

    private CyclicBarrier cyclicBarrier;

    private String path;

    public DataImportThread(CyclicBarrier cyclicBarrier, String path) {
        this.cyclicBarrier = cyclicBarrier;
        this.path = path;
    }

    @Override
    public void run() {
        System.out.println("开始导入："+path+" 数据");
        //TODO
        try {
            cyclicBarrier.await(); //阻塞 condition.await()
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}

```
![image](http://java-run-blog.oss-cn-zhangjiakou.aliyuncs.com/ff7d89d3824944a4ac95f6a5a4e6585a.png
)