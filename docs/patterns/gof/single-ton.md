# Single Ton (싱글톤)

특정 클래스의 인스턴스 또는 함수의 객체를 오직 하나만 생성하여 전역적으로 접근하도록 만드는 패턴

## 원칙

1. 인스턴스 생성의 통제 (오직 1개의 인스턴스만 존재해야함)
2. 전역 접근 허용 (어디서든 동일한 인스턴스에 접근 가능해야함)
3. 생성 제한 (new 연산자나 외부에서 임의로 생성하는 것을 차단해야함)

## 사용

- 로깅 및 DB연결
- 특정 기능의 init이 포함된 전역적 사용이 필요한 경우

## 예제

상황에 따라 다양한 싱글톤 패턴이 존재

### ES6 클래스 기반한 패턴

new 연산자로 생성을 방지하고 한번 생성한 instance로 어디든 접근해 사용할 수 있음

```jsx title="SingletonClass.js"
let instance = null;

export class Singleton {
  private value;

  constructor() {
    if (instance) {
      throw new Error('싱글톤은 new로 생성할 수 없습니다.');
    }

    instance = this;
    this.value = Math.random();
  }

  static getInstance() {
    if (!instance) {
      instance = new StrictSingleton();
    }

    return instance;
  }

  printValue() {
    console.log(this.value);
  }
}
```

```jsx title="index.js"
import { Singleton } from './SingletonClass';

const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();

console.log(singleton1 === singleton2); // true
singleton1.printValue(); // 0.98765421
singleton2.printValue(); // 0.98765421 (같은 인스턴스로 생성되었으므로 동일)
```

### 클로저(Closure)를 이용한 IIFE를 이용한 패턴

싱글톤 클래스는 서브클래스 확장이 어려우므로 IIFE로 간단하게 만드는 것도 가능

```jsx title="SingletonWithIife.js"
// Singleton 참조가 유지되는 한 instance가 클로저에 의해 메모리에 계속 할당
export const Singleton = (function() {
  let instance;
  
  function createInstance() {
    const randomNum = Math.random();

    const obj = {
      getRandom: function() {
        return randomNum;
      },
    };

    return obj;
  }

  return {
    getInstance: function() {
      if (!instance) {
        instance = createInstance();
      }

      return instance;
    }
  }
})();
```

```jsx title="index.js"
import { Singleton } from './SingletonWithIife';

const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();

console.log(singleton1 === singleton2); // true
console.log(instance1.getRandom()) // 0.98765421
console.log(instance2.getRandom()) // 0.98765421
```