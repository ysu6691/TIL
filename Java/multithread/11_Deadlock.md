# 데드락

데드락은 다음과 같은 상황에서 발생할 수 있다.

```java
public class Deadlock {

    public static void main(String[] args) {
        // 뮤텍스 락
        Object lock1 = new Object();
        Object lock2 = new Object();

        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("[t1] get lock1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {}
                synchronized (lock2) {
                    System.out.println("[t1] get lock2");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("[t2] get lock2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {}
                synchronized (lock1) {
                    System.out.println("[t2] get lock1");
                }
            }
        });

        t1.start();
        t2.start();

        // [t1] get lock1
        // [t2] get lock2 (데드락 발생)
    }

}
```

해결 방법 1: 순환 대기의 해소

  ```java
  public class Deadlock {
      public static void main(String[] args) {
        
          Object lock1 = new Object();
          Object lock2 = new Object();

          Thread t1 = new Thread(() -> {
              synchronized (lock1) {
                  System.out.println("[t1] get lock1");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {}
                  synchronized (lock2) {
                      System.out.println("[t1] get lock2");
                  }
              }
          });

          // lock1과 lock2의 순서를 바꿔 순환 대기를 풀어준다.
          Thread t2 = new Thread(() -> {
              synchronized (lock1) {
                  System.out.println("[t2] get lock1");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {}
                  synchronized (lock2) {
                      System.out.println("[t2] get lock2");
                  }
              }
          });

          t1.start();
          t2.start();

          // [t1] get lock1
          // [t1] get lock2
          // [t2] get lock1
          // [t2] get lock2
      }
  }
  ```

해결 방법2: 점유 대기의 해소

  ```java
  public class Deadlock {
      public static void main(String[] args) {

          Object lock1 = new Object();
          Object lock2 = new Object();

          Thread t1 = new Thread(() -> {
              synchronized (lock1) {
                  System.out.println("[t1] get lock1");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {}
                  synchronized (lock2) {
                      System.out.println("[t1] get lock2");
                  }
              }
          });

          // lock1의 점유를 lock2 밖으로 꺼낸다.
          Thread t2 = new Thread(() -> {
              synchronized (lock2) {
                  System.out.println("[t2] get lock2");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {}
              }

              synchronized (lock1) {
                  System.out.println("[t2] get lock1");
              }
          });

          t1.start();
          t2.start();

          // [t1] get lock1
          // [t2] get lock2
          // [t1] get lock2
          // [t2] get lock1
      }
  }
  ```