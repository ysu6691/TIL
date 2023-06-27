# 스레드풀

병렬 작업 처리가 많아지면 스레드 개수가 증가하며 CPU가 바빠지고 메모리 사용량이 증가한다.

병렬 작업 증가로 인한 스레드의 폭증을 막으려면 스레드풀을 사용하는 것이 좋다.

|메서드명|초기 스레드 수|최소 유지하는 스레드 수|최대 가능한 스레드 수|
|--|--|--|--|
|`newCachedThreadPool()`|0|0|Integer.MAX_VALUE|
|`newFixedThreadPool(int nThreads)`|0|생성된 수|nThreads|

또는 `ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue)` 인스턴스를 생성해 사용할 수도 있다.

스레드풀을 종료하는 메서드는 다음과 같다.

|리턴 타입|메서드명|설명|
|--|--|--|
|void|`shutdown()`|현재 작업 뿐만 아니라 작업 큐에 대기하고 있는 모든 작업을 처리한 뒤 스레드풀을 종료시킨다.|
|List<Runnable>|`shutdownNow()`|현재 작업 처리 중인 스레드를 interrupt해서 작업을 중지시키고 스레드풀을 종료시킨다. 작업 큐 내 미처리된 작업(Runnable)을 반환한다.|


```java
import java.util.concurrent.*;

public class ThreadPool {

    public static void main(String[] args) {
        // 작업 개수가 많아지면 새 스레드를 생성시켜 작업을 처리
        // 60초 동안 스레드가 아무 작업을 하지 않으면 풀에서 제거
        ExecutorService executorService1 = Executors.newCachedThreadPool();

        // 최대 5개까지 스레드를 생성시켜 작업을 처리
        ExecutorService executorService2 = Executors.newFixedThreadPool(5);
        
        ExecutorService executorService3 = new ThreadPoolExecutor(
                3,                                 // 초기 스레드 수
                100,                               // 최대 스레드 수
                120L,                              // 놀고 있는 시간
                TimeUnit.SECONDS,                  // 놀고 있는 시간 단위
                new SynchronousQueue<Runnable>()   // 작업 큐
        );

        // 메인 스레드가 종료되더라도 스레드풀 작업은 계속 진행
        executorService1.shutdown(); // 현재 처리 중인 작업 및 작업 큐에 대기하고 있는 모든 작업을 처리한 뒤에 스레드풀 종료
        executorService2.shutdownNow(); // 현재 작업 처리 중인 스레드를 interrupt해서 작업을 중지시키고 스레드풀 종료
    }

}
```