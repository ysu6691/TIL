# Interrupt

```java
public class Interrupt {

    private static class ExceptionThread extends Thread {
        @Override
        public void run() {
            try {
                System.out.println("실행 중");
                Thread.sleep(1000); // 일시 정지 상태가 됐을 때 InterruptedException 예외가 발생한다.
            } catch (InterruptedException e) {
                System.out.println("Interrupt 발생");
            }
            System.out.println("실행 종료");
        }
    }

    private static class PrintThread extends Thread {
        @Override
        public void run() {
            while (true) {
                System.out.println("실행 중");
                // 실행 중일 때에도 interrupt를 감지할 수 있다.
                if (Thread.interrupted()) {
                    System.out.println("Interrupt 발생");
                    break;
                }
            }
            System.out.println("실행 종료");
        }
    }

    public static void main(String[] args) {
        Thread thread1 = new ExceptionThread();
        thread1.start();
        thread1.interrupt();
        /*
        실행 중
        Interrupt 발생
        실행 종료
         */

        Thread thread2 = new PrintThread();
        thread2.start();
        thread2.interrupt();
        /*
        실행 중
        Interrupt 발생
        실행 종료
         */
    }

}
```