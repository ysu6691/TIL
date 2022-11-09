# Vue Lifecycle

## 1. Lifecycle Hooks
- 각 vue 인스턴스는 생성부터 소멸까지 미리 사전에 정의된 몇 단계의 과정을 거치게 된다.
- 각 단계가 트리거가 되어 특정 로직을 수행할 수 있다.

### beforeCreate
- 가장 먼저 실행된다.
- 인스턴스가 DOM에 추가되기 전으로, this로 인스턴스 접근이 불가능하다.
- data, event, methods 등에도 접근할 수 없다.

### created
- 템플릿 및 DOM이 마운팅 혹은 렌더링 되기 전에 실행된다.(DOM에 접근 불가)
- 부모 > 자식 순으로 created 훅이 실행된다.
- data, event, methods 등에 접근할 수 있다.
- 따라서 data 세팅 및 이벤트 리스너 선언에 적합하다.

### beforeMount
- 템플릿 및 render 함수가 컴파일 된 후, 초기 렌더링이 일어나기 직전에 실행된다.

### mounted
- 컴포넌트, 템플릿, DOM에 접근할 수 있고, DOM을 수정할 수 있다.
- 자식 > 부모 순으로 mounted 훅이 실행된다.
- 하지만 자식 컴포넌트가 서버에서 비동기로 데이터를 받아오는 경우처럼, 부모의 mounted 훅이 모든 자식 컴포넌트가 마운트된 상태를 보장하지는 않는다.

### beforeUpdate
- data가 변경됨으로써 DOM에 그 변화를 적용시키기 위해 업데이트 사이클이 시작될 때 실행된다.
- 컴포넌트가 렌더링되기 전에 반응형 데이터의 신규 상태를 가져올 수 있다.

### updated
- DOM이 변경된 이후에 실행된다.
- 변경된 data가 DOM에도 적용된 상태로, 변경된 값들에 DOM을 이용해 접근할 수 있다.
- 다만 여기서 data를 변경할 경우 무한 루프를 일으킬 수 있으므로, 데이터를 직접 변경할 수 없다.

### beforeDestroy
- 인스턴스가 해체되기 직전에 실행된다.
- 인스턴스는 아직 완전하게 작동하기 때문에 모든 속성에 접근 가능하다.

### destroyed
- 인스턴스가 해체되고 난 직후에 실행된다.
- 인스턴스 속성에 접근할 수 없다.
- 인스턴스 해체를 원격 서버에 알려야 할 경우 사용할 수 있다.

## 2. Lifecycle Hooks 사용해보기

```vue
<template>
  <div>
    value : {{ value }}
    <button @click="changeValue">change value</button><br>
  </div>
</template>

<script>
export default {
  name:'ChildComponent',
  data() {
    return {
      value: 0,
    }
  },
  methods: {
    changeValue() {
      this.value = this.value + 1
    }
  },
  beforeCreate() {
    console.log('beforeCreate')
  },
  created() {
    console.log('created')
  },
  beforeMount() {
    console.log('beforeMount')
  },
  mounted() {
    console.log('mounted')
  },
  beforeUpdate() {
    console.log('beforeUpdate')
  },
  updated() {
    console.log('updated')
  },
  beforeDestroy() {
    console.log('beforeDestroy')
  },
  destroyed() {
    console.log('destroyed')
  }
}
</script>

<style>

</style>
```