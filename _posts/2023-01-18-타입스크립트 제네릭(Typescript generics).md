---
title: '[typescript] 제네릭(Generics)'
categories:
    - typescript
tag:
    - typescript
    - generic
---


## 타입스크립트 제네릭(Generics)의 사전적 정의

제네릭은 재사용성이 높은 컴포넌트를 만들 때 자주 활용되는 특징이다. 특히, 한 가지 타입보다 여러 가지 타입에서 동작하는 컴포넌트를 만들 때 유용하다. **<span style="color: blue">제네릭이란 타입을 마치 함수의 파라미터처럼 사용하는 것</span>**을 의미한다.  

```typescript
function getText<T>(text: T): T {
    return text;
}
```
위 함수는 제네릭 기본 문법이 적용된 함수이다. 함수를 호출할 때 아래와 같이 함수 안에서 사용할 타입을 파라미터처럼 넘겨줄 수 있다.

```typescript
getText<string>("hello world");
getText<number>(20230118);
getText<boolean>(true);
```
위의 코드 중 `getText<string>("hello world");` 가 어떻게 동작하는지 알아보자.

```typescript
function getText<string>(text: string): string {
    return text;
}
```
위 함수에서 제네릭 타입이 `string` 이 되는 이유는 `getText()` 함수를 호출할 때 제네릭 값 (함수 안에서 사용할 타입)으로 `string` 을 넘겼기 때문이다. 그리고 함수의 인자로 `"hello world"` 를 넘기게 되면 위의 함수는 입력 값의 타입이 `string` 이면서 반환 값 타입 또한 `string` 이어야 하는 함수가 된다. 