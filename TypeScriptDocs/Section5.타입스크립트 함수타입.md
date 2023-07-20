### Typescript의 함수타입

1. 함수를 설명하는 좋은 방법

  - 어떤 매개변수를 받고, 어떤 결과값을 반환하는지
  - 어떤 [타입의] 매개변수를 받고, 어떤 [타입의] 결과값을 반환하는지

```typescript
  function func(a: number, b:number) {
    return a + b;
  };
```

2. 화살표 함수의 타입을 정의하는 방법

```typescript
  const add = (a:number, b:number) => a + b;
```

3. 함수의 매개변수
  
    - 선택적 매개변수는 필수 매개변수 뒤에 위치해야함.

```typescript
function introduce(name = '민호', weight:number, tall?: number) {
  console.log(`name : ${name}`);
  console.log(`weight : ${weight}`);
  if (typeof tall === 'number'){
    console.log(`tall : ${tall + 10}`);
  }
};

function getSum(...rest: number[]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));

  return sum;
};
```

#### 함수타입 표현식과 호출 시그니처

1. 함수타입 표현식

타입 별칭을 이용하여 함수의 타입을 정의

```typescript
type Operation = (a: number, b: number) => number;

const add: (a: number, b: number) => number = (a, b) => a + b;
const sub: Operation = (a, b) => a - b;
const multiply: Operation = (a, b) => a * b;
const divide: Operation = (a, b) => a / b;
```

2. 호출 시그니처 (=콜 시그니처)

함수의 타입을 별도로 정의하는 문법

```typescript
  type Operation = {
    (a: number, b:number): number;
  };

  const add: Operation = (a, b) => a + b;
  const sub: Operation = (a, b) => a - b;
  const multiply: Operation = (a, b) => a * b;
  const divide: Operation = (a, b) => a / b;
```

#### 함수 타입의 호환성

특정 함수 타입을 다른 함수 타입으로 취급해도 호환되는가?

- 반환값의 타입이 호환되는지
- 매개변수의 타입이 호환되는지

1. 반환값의 타입이 호환되는지

```typescript
  type A = () => number;  //반환값 : number
  type B = () => 10;  //반환값 : number 리터럴

  let a: A = () => 10;
  let b: B = () => 10;

  a = b;
  //false example : down cast
  b = a;
```

2. 매개변수의 타입이 호환되는지

  2-1. 매개변수의 개수가 같을 때

  매개변수의 타입을 기준으로 호환성 판단 시 up cast는 호환되지 않음

```typescript
  type X = (value: number) => void;
  type Y = (value: 10) => void;

  let x: X = (value) => {};
  let y: Y = (value) => {};

  //false example : up cast
  x = y;
  y = x;

  // 매개변수가 객체타입을 사용하는 예시
  type Animal = {
    name: string;
  };

  type Dog = {
    name: string;
    color: string;
  };

  let animalFunc = (animal: Animal) => {
    console.log(animal.name);
  };

  let dogFunc = (dog: Dog) => {
    console.log(dog.name);
    console.log(dog.color);
  };

  // false example 서브타입이 슈퍼타입 객체로 up cast 현상
  animalFunc = dogFunc;
```

  2-2. 매개변수의 개수가 다를 때

  할당하고자 하는 함수 타입의 매개변수 갯수가 적을 때에만 할당됨.

  ```typescript
  type Func1 = (a: number, b: number) => void;
  type Func2 = (a: number) => void;

  let func1: Func1 = (a, b) => {};
  let func2: Func2 = (a) => {};

  func1 = func2;
  //false example
  func2 = func1;
  ```

  #### 함수 오버로딩

  함수를 매개변수의 개수나 타입에 따라 여러가지로 정의하는 방법

  -> 자바스크립트에서는 지원하지 않고 타입스크립트에서만 지원

  > 오버로드 시그니처 : 함수의 구현부 없이 선언식만 써놓은 문법
  > 함수의 구현부(=구현 시그니처)

```typescript
  //오버로드 시그니처
  function func(a: number) : void;
  function func(a: number, b: number, c: number) : void;

  //구현 시그니처
  function func(a: number, b?: number, c?: number) {
    if(typeof b === 'number' && typeof c === 'number'){
      console.log(a + b + c);
    } 
    else {
      console.log(a * 20);
    }
  };

  func(1);
  func(1, 2, 3);
```

#### 사용자 정의 타입가드(Custom Type Guard)

함수를 타입가드로 만드는 문법

> 예시
```typescript
  type Dog = {
    name: string;
    isBark: boolean;
  };

  type Cat = {
    name: string;
    isScratch: boolean;
  };

  type Animal = Dog | Cat;

  function isDog(animal: Animal): animal is Dog {
    return (animal as Dog).isBark !== undefined;
  };

  function warning(animal: Animal) {
      if(isDog(animal)){
        animal;
      }
      else {
        animal;
      }
  };
```