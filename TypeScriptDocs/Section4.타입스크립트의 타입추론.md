#### 타입 추론

1. 가장 대표적인 타입 추론이 가능한 상황

-> 일반적인 변수를 선언하는 상황

```typescript
  let num = 10; //number 타입으로 추론
  let str = 'hi'  //string 타입으로 추론
  let arr = [1, 'hi', true];  //(number | string | boolean) 타입으로 추론
  function func() { //func(): number 타입으로 추론
    return 1;
  };
```

2. 예외적 타입 추론 상황

-> 값을 초기화하지 않는 변수를 선언하는 상황

```typescript
  let num; //암묵적인 any 타입으로 추론

  num = 10; //number 타입으로 타입 추론
  d.toFixed();  //true

  num = '10'; //string 타입으로 타입 추론
  num.toUpperCase();  //true
  num.toFixed();  //false
```

-> const로 변수를 선언하는 상황

```typescript
  const num = 10; //num:10 으로 number 리터럴 타입으로 타입 추론
  const str = 'hi'; //str:'hi'로 string 리터럴 타입으로 타입 추론
```

#### 타입 단언

변수 값의 타입을 직접 단언(가정)

1. 타입 단언의 규칙
  
  > 값 as 단언 : A as B
    
    1. A가 B의 슈퍼타입이거나
    2. A가 B의 서브타입이어야 함.

```typescript
  let num1 = 10 as never;
  let num2 = 10 as unknown;

  //false example
  let num3 = 10 as string;

  type Person = {
    name: string;
    age: number;
  };

  let person1 = {} as Person;

  let person2 = {
    name: '',
    age: 0,
    country: '',
  } as Person;
```
2. const 단언

객체 타입의 프로퍼티를 readonly로 변경할 때 사용

```typescript
  let person: Person = {
    name: '',
    age: 0,
  } as const;
```

3. Non Null 단언

어떤 값의 변수가 null이거나 undefined가 아님을 타입스크립트에게 알려주는 단언

```typescript
  type Post = {
    title: string;
    author?: string;
  };

  let post: Post = {
    title: '게시글',
  };

  //false example
  //post.author의 값은 undefined로 down cast라 false
  const len: number = post.author?.length;

  //true example
  //post.author의 값이 있을거라 단언하여 true
  const len: number = post.author!.length;
```

#### 타입 좁히기

조건문 등을 이용해 넓은 타입에서 좁은 타입으로 타입을 상황에 따라 좁히는 방법

- 타입 가드 (Type Guard)

    유니온 타입으로 표현된 타입을 조건문과 함께 검사하는 코드

```typescript
  type Person = {
    name: string;
    age: number;
  };

  function func(value: number | string | Date | null | Person) {
    if (typeof value === 'number') {
      console.log(value.toFixed());
    } 
    else if (typeof value === 'string') {
      console.log(value.toUpperCase());
    }
    else if (value instanceof Date) {
      console.log(value.getTime());
    }
    else if (value && "age" in value) {
      console.log(`${value.name}은 ${value.age}살 입니다.`);
    }
  };
```

#### 서로소 유니온 타입(=태그드 유니온 타입)

교집합(intersection)이 없는 타입들로만 만든 합집합(union)타입

1. 프로퍼티를 리터럴 타입으로 하여 서로소 객체로 분리

```typescript
  type Admin = {
    tag : 'ADMIN';
    name: string;
    kickCount: number;
  };

  type Member = {
    tag : 'MEMBER';
    name: string;
    point: number;
  };

  type Guest = {
    tag : 'GUEST';
    name: string;
    visitCount: number;
  };

  type User = Admin | Member | Guest;

  /**
   * Admin -> {name}님 현재까지 {kickCount}명 강퇴
    * Member -> {name}님 현재까지 {point} 적립
    * Guest -> {name}님 현재까지 {visitCount}번 방문
    */

  //if문 example
  function ifLogin(user: User) {
    if (user.tag === 'ADMIN') {
      console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴`);
    }
    else if (user.tag === 'MEMBER') {
      console.log(`${user.name}님 현재까지 ${user.point} 적립`);
    }
    else {
      console.log(`${user.name}님 현재까지 ${user.visitCount}번 방문`);
    }
  };

  //case문 example
  function caseLogin(user: User) {
    switch(user.tag){
      case 'ADMIN' : {
        console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴`);
        break;
      }
      case 'MEMBER' : {
        console.log(`${user.name}님 현재까지 ${user.point} 적립`);
        break;
      }
      case 'GUEST' : {
        console.log(`${user.name}님 현재까지 ${user.visitCount}번 방문`);
        break;
      }
    }
  };
```

2. 비동기 작업의 결과를 처리하는 객체

```typescript
  type LoadingTask = {
    state: 'LOADING';
  };
  
  type FailedTask = {
    state: 'FAILED';
    error: {
      message: string;
    };
  };
  
  type SuccessTask = {
    state: 'SUCCESS';
    response: {
      data: string;
    };
  };

  type AsyncTask = LoadingTask | FailedTask | SuccessTask;

  /**
   * LOADING -> 로딩 중
   * FAILED -> 에러 : {message}
   * SUCCESS -> 성공 : {data}
   */
  function processResult(task: AsyncTask) {
    switch(task.state) {
      case 'LOADING' : {
        console.log('로딩 중');
        break;
      }
      case 'FAILED' : {
        console.log(`에러 : ${task.error.message}`);
        break;
      }
      case 'SUCCESS' : {
        console.log(`성공 : ${task.response.data}`);
        break;
      }
    }
  };
```