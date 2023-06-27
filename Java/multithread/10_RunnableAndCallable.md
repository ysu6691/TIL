# Runnable과 Callable

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class RunnableAndCallable {

    public static void main(String[] args) {
        
        // Runnable은 리턴값이 없음
        ExecutorService executorService1 = Executors.newFixedThreadPool(5);
        for (int i=0; i<10; i++) {
            executorService1.execute(new Runnable() {
                @Override
                public void run() {
                    Thread thread = Thread.currentThread();
                    System.out.println(thread.getName());
                }
            });
        }
        /*
        pool-1-thread-1
        pool-1-thread-1
        pool-1-thread-2
        pool-1-thread-2
        pool-1-thread-2
        pool-1-thread-2
        pool-1-thread-4
        pool-1-thread-1
        pool-1-thread-3
        pool-1-thread-5
         */

        try {
            Thread.sleep(3000);
            System.out.println("=====");
        } catch (InterruptedException e) {}

        // Callable은 Future를 리턴
        ExecutorService executorService2 = Executors.newFixedThreadPool(5);
        for (int i=0; i<10; i++) {
            Future<String> future = executorService2.submit(new Callable<String>() {
                @Override
                public String call() throws Exception {
                    Thread thread = Thread.currentThread();
                    return thread.getName();
                }
            });
            try {
                String result = future.get();
                System.out.println(result);
            } catch (Exception e) {}
        }
        /*
        pool-2-thread-1
        pool-2-thread-2
        pool-2-thread-3
        pool-2-thread-4
        pool-2-thread-5
        pool-2-thread-1
        pool-2-thread-2
        pool-2-thread-3
        pool-2-thread-4
        pool-2-thread-5
         */
    }

}
```