# Priority 설정

```java
public class Priority {
    
    private static class WorkObject {

        public synchronized void myMethod(int step) {

            Thread thread = Thread.currentThread();
            System.out.println(thread.getName() + ": " + step +"번째 작업중");
    
            notify();
    
            try {
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
        MyThread threadC = new MyThread(workObject, "threadC");

        threadA.setPriority(Thread.MAX_PRIORITY);
        threadB.setPriority(5);
        threadC.setPriority(Thread.MIN_PRIORITY);

        threadA.start();
        threadB.start();
        threadC.start();
    }

    /*
     *  threadA: 1번째 작업중
        threadC: 1번째 작업중
        threadB: 1번째 작업중
        threadC: 2번째 작업중
        threadA: 2번째 작업중
        threadB: 2번째 작업중
        threadA: 3번째 작업중
        threadB: 3번째 작업중
        threadC: 3번째 작업중
        threadB: 4번째 작업중
        threadA: 4번째 작업중
        threadB: 5번째 작업중
        threadA: 5번째 작업중
        threadC: 4번째 작업중
        threadA: 6번째 작업중
        threadB: 6번째 작업중
        threadA: 7번째 작업중
        threadB: 7번째 작업중
        threadC: 5번째 작업중
        threadB: 8번째 작업중
        threadA: 8번째 작업중
        threadB: 9번째 작업중
        threadA: 9번째 작업중
        threadC: 6번째 작업중
        threadA: 10번째 작업중
        threadB: 10번째 작업중
        threadC: 7번째 작업중 <- 우선순위가 낮아 가장 늦게 작업을 끝냄
        
        threadC를 notify할 스레드가 없어 무한 wait한다.
     */

}
```