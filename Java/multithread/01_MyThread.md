# 스레드의 생성과 실행

```java
public class MyThread {

    private static class Task implements Runnable {
        @Override
        public void run() {
            // 스레드가 실행하는 코드
        }
    }
    
    private static class WorkerThread extends Thread {
        @Override
        public void run() {
            // 스레드가 실행하는 코드
        }
    }

    public static void main(String[] args) {

        // 메인 스레드가 실행하는 코드

        // Runnable: 스레드가 작업을 실행할 때 사용하는 인터페이스
        Runnable task = new Task();

        // 스레드 생성 방법1
        Thread thread1 = new Thread(task);
        
        // 스레드 생성 방법2
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                // 스레드가 실행하는 코드
            }
        });

        // 스레드 생성 방법3 (람다 사용)
        Thread thread3 = new Thread(() -> {
            // 스레드가 실행하는 코드
        });

        // 스레드 생성 방법4
        Thread thread4 = new WorkerThread();

        // 스레드 생성 방법5
        Thread thread5 = new Thread() {
            @Override
            public void run() {
                // 스레드가 실행하는 코드
            }
        };

        thread1.start();
        thread2.start();
        thread3.start();
        thread4.start();
        thread5.start();
    }

}
```