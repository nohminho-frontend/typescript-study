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

### 제네릭 메서드 타입 정의

1. map 메서드

기존 라이브러리에 정의된 map 함수

```typescript
  //lib.es5.d.ts
  map<U>(callbackfn: (value: T, index: number, array: T[]) => U, thisArg?: any): U[];
```

사용자가 정의하는 map 함수

```typescript
  const arr = [1, 2, 3];
  const newArr = arr.map((it) => it * 2);

  function map<T, U>(arr: T[], callback: (item: T) => U) {
    let result = [];

    for(let i = 0; i < arr.length; i++) {
      result.push(callback(arr[i]));
    }

    return result;
  }

  map(arr, (it) => it * 2);
  map(['hi', 'hello'], (it) => parseInt(it));
```

2. forEach 메서드

기존 라이브러리에 정의된 forEach 함수

```typescript
  //lib.es5.d.ts
  forEach(callbackfn: (value: T, index: number, array: T[]) => void, thisArg?: any): void;
```

사용자가 정의하는 forEach 함수

```typescript
  const arr = [1, 2, 3];
  const newArr = arr.map((it) => it * 2);

  function forEach<T>(arr: T[], callback: (item: T) => void) {
    for(let i = 0; i < arr.length; i++) {
      callback(arr[i]);
    }
  };

  forEach(arr, (it) =>{
    console.log(it.toFixed());
  });
```

#### 제네릭 인터페이스

제네릭 인터페이스를 타입으로 정의할 떄, 타입변수에 할당하는 타입을 꼭 정의해줘야 함.

```typescript
  interface KeyPair<K, V>{
    key: K;
    value: V;
  };

  let keyPair: KeyPair<string, number> = {
    key: 'key',
    value: 0,
  };
```

1. 제네릭 타입 별칭 사용 예시

```typescript
  type Map<T> {
    [key: string]: T;
  };

  let stringMap: Map<string> = {
    key: '',
  };
```

2. 제네릭 인덱스 시그니처 사용 예시

```typescript
  interface Map<T> {
    [key: string]: T;
  };

  let numberMap: Map<number> = {
    key1: 1,
    key2: 2,
  };

  let stringMap: Map<string> = {
    key1: '1',
    key2: '2',
  };
```

#### 제네릭 클래스

제네릭 클래스는 클래스를 사용 시에 변수타입을 정의해주지 않아도 됨.

```typescript
  class List<T> {
    constructor(private list: T[]) {}

    push(data: T) {
      this.list.push(data);
    }
    pop() {
      return this.list.pop();
    }
    print() {
      console.log(this.list);
    }
  };

  const numberList = new List([1, 2, 3]);

  numberList.pop();
  numberList.push(4);
  numberList.print();
```

#### 프로미스 객체

프로미스는 resolve나 reject를 호출해서 전달하는 비동기 작업의 결과값의 
타입을 자동으로 추론하는 기능이 없기때문에 unknown타입을 가짐.

```typescript
  //false example
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(20);
    }, 3000);
  });

  promise.then((response) => { //false
    console.log(response * 10);
  });

  //true example
  const promise = new Promise<number>((resolve, reject) => {
    setTimeout(() => {
      resolve(20);
      reject('~~때문에 실패함');
    }, 3000);
  });

  promise.then((response) => {
    console.log(response * 10);
  })

  promise.catch((err) => {
    if(typeof err === 'string') {
      console.log(err);
    }
  });
```

프로미스를 반환하는 함수의 타입을 정의할 때

```typescript
  interface Post {
    id: number;
    title: string;
    content: string;
  };

  function fetchPost(): Promise<Post> {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({
          id: 1,
          title: 'hello',
          content: 'come on',
        });
      }, 3000);
    });
  };

  const postRequest = fetchPost();

  postRequest.then((post) => {
    post.id;
  });
```