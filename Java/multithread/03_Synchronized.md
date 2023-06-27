# 스레드 동기화

`synchronized` 키워드를 이용해 하나의 스레드만 접근가능하도록 동기화 블록을 설정할 수 있다.

```java
public class Synchronized {

    private static class Calculator {
        private int memory;

        public int getMemory() {
            return memory;
        }

        // 하나의 스레드만 메서드에 접근 가능
        public synchronized void setMemory(int memory) {
            this.memory = memory;
            
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {}

            System.out.println(getMemory());
        }
    }

    private static class MyThread extends Thread {

        private Calculator calculator;
        private int memory;

        public MyThread(String name) {
            setName(name);
        }

        public void setCalculator(Calculator calculator) {
            this.calculator = calculator;
        }

        public void setMemory(int memory) {
            this.memory = memory;
        }

        @Override
        public void run() {
            calculator.setMemory(this.memory);
        }
    }

    public static void main(String[] args) {

        Calculator calculator = new Calculator();

        MyThread thread1 = new MyThread("thread1");
        MyThread thread2 = new MyThread("thread2");

        thread1.setCalculator(calculator);
        thread2.setCalculator(calculator);

        thread1.setMemory(50);
        thread2.setMemory(100);

        thread1.start();
        thread2.start();

        // 100
        // 50
    }
    
}
```