# 자바 스레드

- 스레드 상태

  <img src="https://github.com/ysu6691/TIL/assets/109272360/62fea82a-2070-47c9-abcc-42436512f599">

  |상태|열거 상수|설명|
  |--|--|--|
  |객체 생성|NEW|스레드 객체 생성 후, 아직 `start()` 메서드가 실행되지 않은 상태|
  |실행 대기|RUNNABLE|실행을 기다리는 상태|
  |일시 정지|WAITING|다른 스레드가 통지할 때까지 기다리는 상태|
  ||TIMED_WAITING|주어진 시간 동안 기다리는 상태|
  ||BLOCKED|사용하고자 하는 객체의 lock이 풀릴 때까지 대기하는 상태|
  |종료|TERMINATED|실행을 마친 상태|

- 스레드 메서드

  |구분|메서드|설명|
  |--|--|--|
  |일시 정지로 이동|`sleep(long millis)`|주어진 시간 동안 스레드를 일시 정지 상태로 만든다.|
  ||`join()`|`threadA.join()`을 호출한 스레드는 `threadA`가 종료될 때까지 일시 정지 상태가 된다.|
  ||`wait()`|`notify()`나 `interrupt()`가 호출될 때까지 스레드를 일시 정지 상태로 만든다.|
  |일시 정지에서 벗어남|`interrupt()`|일시 정지 상태일 경우, InterruptedException을 발생시켜 RUNNABLE 또는 TERMINATED 상태로 만든다.|
  ||`notify()` `notifyAll()`|`wait()` 메서드로 인해 일시 정지 상태인 스레드를 RUNNABLE 상태로 만든다.|
  |실행 대기로 보냄|`yield()`|실행 상태에서 다른 스레드에게 실행을 양보하고 RUNNABLE 상태가 된다.|





