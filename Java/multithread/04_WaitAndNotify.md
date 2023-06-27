# wait()과 notify()

- `wait()`: 대기 큐에 진입한다.

- `notify()`: 대기 큐에 있는 스레드를 깨운다.

- `notifyAll()`: 대기 큐에 있는 모든 스레드를 깨운다.

```java
public class WaitAndNotify {

    private static class WorkObject {

        public synchronized void myMethod(int step) {

            Thread thread = Thread.currentThread();
            System.out.println(thread.getName() + ": " + step +"번째 작업중");
    
            // 대기 큐의 스레드 깨우기
            notify();
    
            try {
                // 대기 큐 진입
                wait();
            } catch (InterruptedException e) {}
        }
    }


    private static class MyThread extends Thread {

        private WorkObject workObject;

        public MyThread(WorkObject workObject, String name) {
            setName(name);
            this.workObject = workObject;
        }

        @Override
        public void run() {
            for (int i=1; i<=10; i++) {
                workObject.myMethod(i);
            }
        }
    }

    public static void main(String[] args) {
        WorkObject workObject = new WorkObject();

        MyThread threadA = new MyThread(workObject, "threadA");
        MyThread threadB = new MyThread(workObject, "threadB");

        threadA.start();
        threadB.start();
    }

    /*
     *  threadA: 1번째 작업중
        threadB: 1번째 작업중
        threadA: 2번째 작업중
        threadB: 2번째 작업중
        threadA: 3번째 작업중
        threadB: 3번째 작업중
        ...
        threadA: 9번째 작업중
        threadB: 9번째 작업중
        threadA: 10번째 작업중
        threadB: 10번째 작업중
     */

}
```