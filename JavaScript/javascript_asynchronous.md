# JavaScript Asynchronous

## 1. 비동기 처리

### 동기와 비동기
- 동기(Synchronous)
  - 모든 일을 순서대로 하나씩 처리하는 것
  - 이전 작업이 끝나야 다음 작업을 시작

- 비동기(Asynchronous)
  - 작업을 시작한 후 결과를 기다리지 않고 다음 작업을 처리하는 것(병령적 수행)
  - 시간이 필요한 작업들은 요청을 보낸 뒤 응답이 빨리 오는 작업부터 처리

- 비동기를 사용하는 이유
  - **사용자 경험**을 고려하기 때문
  - 먼저 처리되는 부분부터 보여주면서, 긍정적인 효과를 볼 수 있음
  - 예시
    ```js
    console.log("첫 번째 작업")
    setTimeout(() => console.log("시간이 오래 걸리는 두 번째 작업"), 2000)
    console.log("세 번째 작업")

    /*
    첫 번째 작업
    세 번째 작업
    시간이 오래 걸리는 두 번째 작업
    */
    ```

### JavaScript의 비동기 처리
- 자바스크립트는 single thread 언어로, 한 번에 하나의 일만 수행할 수 있다.
  - thread: 작업을 처리할 때 실제로 작업을 수행하는 주체
- 요청이 들어올 때마다 **Call Stack**에서 순차적으로 처리한다.
- **따라서 비동기 처리를 위한 환경(런타임)이 필요하다.**
  - 런타임: 특정 언어가 동작할 수 있는 환경(자바스크립트의 경우 웹 브라우저, Node.js 등이 있다.)
- 브라우저 환경에서는 **Web API**, **Task Queue**, **Event Loop**를 제공한다.
  - Web API: 브라우저에서 제공하는 런타임 환경으로, 시간이 소요되는 작업을 처리
  - Task Queue: Web API에서 비동기 처리된 CallBack 함수가 대기하는 Queue
  - Event Loop: Call Stack이 비어 있는지 지속적으로 확인 후, 비어 있다면 Task Queue에서 대기 중인 가장 오래된 작업을 Call Stack으로 push

- 브라우저 환경에서의 비동기 동작은 다음과 같다.
  1. 모든 작업이 Call Stack(LIFO)으로 들어간 후 처리된다.
  2. 비동기 함수의 경우 Web API로 보내서 처리한다.
  3. Web API에서 처리가 끝난 작업들은 Task Queue(FIFO)에 순서대로 들어간다.
  4. Event Loop가 Call Stack이 비어 있는지 확인한 뒤, Task Queue에서 가장 오래된 작업을 Call Stack으로 보낸다.
  - 예시

    <img src="https://user-images.githubusercontent.com/109272360/198319006-e573f375-b447-434b-ba98-6f5ed776036d.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319011-c4ab7038-e87f-4276-a816-cc2cdf1414e9.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319020-06f0eab9-fe31-48b0-a8db-a33dccd4a98a.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319023-269304bb-4949-46b7-95a6-8584f7f727cc.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319031-af423bac-fa42-4dce-8f35-3149b5a21a30.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319041-6868164e-f3a4-45bd-ba99-8721d6e55f07.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319047-57eaa60f-9b25-43dc-85b3-ac14675d2b57.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319054-f36630c6-1e3b-4cf4-90af-6031a43caa0c.png" width="750px">
    <img src="https://user-images.githubusercontent.com/109272360/198319060-2433ccf9-0321-4e2f-89ba-4599d1e20198.png" width="750px">

