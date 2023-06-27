# 데몬 스레드

데몬 스레드는 메인 스레드의 작업을 돕는 보조적인 역할을 수행한다.

일반적인 스레드는 메인 스레드가 종료되어도 할 일을 모두 마친 뒤 종료되지만, 데몬 스레드는 메인 스레드가 종료되면 함께 종료된다.


```java
public class Daemon {

    private static class DaemonThread extends Thread {
        @Override
        public void run() {
            while (true) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {}

                System.out.println("데몬 실행");
            }
        }
    }

    public static void main(String[] args) {
        DaemonThread thread = new DaemonThread();
        thread.setDaemon(true); // 데몬으로 설정
        thread.start();

        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {}

        System.out.println("메인 스레드 종료");
    }

    /*
    데몬 실행
    데몬 실행
    메인 스레드 종료
     */
}
```