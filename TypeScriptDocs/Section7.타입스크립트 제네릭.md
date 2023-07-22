### 타입스크립트의 제네릭

#### 제네릭 함수

모든 타입에 사용이 가능한 범용 함수

> 타입변수 : <T>

```typescript
  function func<T>(value: T): T {
    return value;
  };

  // 인수를 통한 타입 추론
  let num = func(10);
  let str = func('hello');
  let bool = func(true);

  // 명시적으로 타입 정의
  let arr = func<[number, number, number]>([1, 2, 3] );
```

#### 타입변수의 응용

1. 함수를 호출할 때, 인수로 전달받는 타입이 다른 경우

```typescript
  function swap<T>(a: T, b: T) {
    return [b, a];
  };

  const [a, b] = swap(1, 2);

  // -> 타입 변수를 여러개 선언하여 문제 해결 가능
  function swap<T, U>(a: T, b: U) {
    return [b, a];
  };

  const [a, b] = swap('1', 2);
```
2. 변수를 배열타입으로 정의하는 경우

```typescript
  // 배열에 포함된 인자들의 타입별로 타입을 추론가능하게 하는 방법
  function returnFirstValue<T>(data: [T, ...unknown[]]) {
    return data[0];
  };

  let resultNum = returnFirstValue([0, 1, 2]);
  let resultStr = returnFirstValue([0, '1', '2']);
```

3. 타입변수를 제한

```typescript
  // 타입변수를 확장하여 무조건 프로퍼티를 가지게하여 제한해 값허용을 할 수 있게 하는 방법
  function getLength<T extends { length: number }>(data: T) {
    return data.length;
  }

  let result1 = getLength([1, 2, 3]);
  let result2 = getLength('12345');
  let result3 = getLength({ length : 10 });
  // length 프로퍼티가 없는 값들은 사용에 제한됨.
  let result4 = getLength(1);
```