# Redux

## 1. React에서 Redux를 사용하는 이유

Redux는 컴포넌트 간의 props로 연결되는 state나 앱 전체적으로 쓰인 state를 관리하는 **상태 관리 라이브러리**이다.

하지만 React에는 이미 상태 관리를 도와주는 **Context Hook**이 존재한다.

그렇다면 왜 Redux를 사용할까?

- Context Hook의 잠재적인 단점
  - 애플리케이션의 규모에 따라 상태 관리가 복잡해질 수 있다.
    ```js
    return (
      <AuthContextProvider>
        <ThemeContextProvider>
          <UIInteractionContextProvider>
            ...
          </UIInteractionContextProvider>
        </ThemeContextProvider>
      </AuthContextProvider>
    )
    ```
  - 데이터의 변경이 자주 일어나는 경우 성능이 다소 떨어질 수 있다.

## 2. Redux 작동 방식