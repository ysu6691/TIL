# Django CORS

## 1. CORS

### SOP(Same-Origin Policy)
- 동일 출처 정책
- 한 origin으로부터 로드된 document 또는 script가 다른 origin의 리소스와 상호작용 할 수 있는 방법을 제한하는 보안 메커니즘
```js
// 현재 document의 origin을 확인할 수 있다.
docoment.location.origin
```

### Cross-Origin Resource Sharing
- 