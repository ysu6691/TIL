# 스레드 join

```java
public class Join {

    private static class SumThread extends Thread {
        private long sum;

        public long getSum() {
            return sum;
        }

        @Override
        public void run() {
            for (int i=1; i<=100; i++) {
                sum += i;
            }
        }
    }

    public static void main(String[] main) {
        SumThread sumThread = new SumThread();

        sumThread.start();

        try {
            // sumThread가 종료될 때까지 일시 정지
            sumThread.join();
        } catch (InterruptedException e) {}

        System.out.println(sumThread.getSum()); // 5050
    }

}
```