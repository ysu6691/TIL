# 스레드 이름

```java
public class Name {

    public static void main(String[] args) {
        Thread mainThread = Thread.currentThread();
        System.out.println(mainThread.getName() + " 실행"); // main 실행

        Thread thread1 = new Thread() {
            @Override
            public void run() {
                System.out.println(getName() + " 실행"); // Thread-0 실행
            }
        };
        thread1.start();

        Thread thread2 = new Thread() {
            @Override
            public void run() {
                System.out.println(getName() + " 실행"); // MyThread 실행
            }
        };
        thread2.setName("MyThread");
        thread2.start();
    }

}
```