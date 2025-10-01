# Monitoring Garbage Collector (가비지 콜렉터 모니터링)

GC(가비지콜렉터)가 해당 데이터를 수집했는지 모니터링 할 수 있는 방법(패턴)

메모리를 크게 잡아먹는 객체나 데이터를 관리할 때, 이를 수행하여 메모리 처리 여부를 모니터링 합니다.

이를 위해 FinalizationRegistry(ES2022) [(링크)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/FinalizationRegistry) 기능을 사용합니다.

``` tsx
const TOKEN = 'Fid31vcIDNdi32';

// 레지스트리 생성
const registry = new FinalizationRegistry((heldValue) => {
  console.log(`${heldValue}가 GC되었습니다.`);
});

// 객체 등록
const obj = { data: 'Large Object' };
registry.register(obj, 'obj의 설명', TOKEN); // obj가 GC되면 콜백 실행

// 등록 취소
registry.unregister(TOKEN); // 등록 시 사용한 토큰으로 취소
```

## 작동이 안되는 경우

GC는 비결정적이며, 메모리 부족 상황이 발생할 때에만 실행(트리거)됩니다.

즉, 일반적으로 메모리가 부족한 상황이 아니기 때문에 참조를 끊는다고 즉시 발동하지 않음.

때문에 모니터링을 위해서 일부러 메모리가 부족할만큼 특정 상황을 구현해두고 테스트해야 GC가 바로 수행됩니다.

## 이걸 언제 써야 좋을까요?

- 클라이언트 내에서 복잡한 연산(수학적인 또는 그래픽 작업 등)을 다룰 때
- 용량이 큰 정적 파일(이미지, 영상, 파일)을 참조하는 변수를 다룰 때

``` jsx
// 만약 이 큰 변수가 전역화 되어있거나
// 만약 이 큰 변수가 클로져 상에서 계속 존재해있다면?
const file = ''; // 큰 정적 파일
```
