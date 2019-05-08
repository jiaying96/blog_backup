

## 调用start方法方可启动线程，真正实现了多线程；而run方法只是thread的一个普通方法调用还是在当前线程里执行，并没有增加线程的数量。

## start()

用start()来启动线程，真正实现了多线程，就算出现start()方法也是**继续执行下面的代码而不需等待run()体代码执行完**,如下代码无需等待pong()方法输出B而是继续执行下面的输出A


    public class Test {
        public static void main(String args[]) {
            Thread t = new Thread() {
                public void run() {
                    pong();
                }
            };
            t.start();
            System.out.print("A");
        }
        static void pong() {
            System.out.print("B");
        }
    }

    
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109201816468.png)


通过调用Thread类的 start()方法来启动一个线程，这时此线程处于就绪（可运行）状态，并没有运行，一旦得到cpu时间片，就开始执行run()方法，这里方法 run()称为线程体，它包含了要执行的这个线程的内容，Run方法运行结束，此线程随即终止。

## run()

run()方法只是类的一个普通方法而已，如果直接调用Run方法，程序中依然只有主线程这一个线程，其程序执行路径还是只有一条，还是要顺序执行，还是要等待run方法体执行完毕后才可继续执行下面的代码，没有达到多线程的目的。


    public class Test {
    	 public static void main(String args[]) {
             Thread t = new Thread() {
                 public void run() {
                     pong();
                 }
             };
             t.run();
             System.out.print("A");
         }
      
         static void pong() {
             System.out.print("B");
         }
    
    }


结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109201902102.png)