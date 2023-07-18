## TypeScript의 이해

### TypeScript를 이해한다는 것은?

타입스크립트의 구체적인 원리와 동작 방식을 살펴보는 것

1. 어떤 기준으로 타입을 정의하는지
2. 어떤 기준으로 타입간의 관계를 정의하는지
3. 어떤 기준으로 타입의 오류를 검사하는지


#### 타입은 집합

타입스크립가 말하는 타입이란?

- 값들을 포함하고 있는 집합
- 타입들끼리 부모와 자식 관계를 성립

#### 타입 계층도 - 기본 타입간의 호환성

특정 타입을 다른 타입으로 취급해도 호환이 되는가

1. Unknown 타입

모든 타입들의 슈퍼타입(=전체집합)

```typescript
////true example : up cast
  let a: unknown = 1;
  let b: unknown = 'hello';
  let c: unknown = true;
  let d: unknown = undefined;
  let e: unknown = null;

  //false example : down cast
  let unknownVar: unknown;

  let num: number = ununknownVar;
  let str: string = ununknownVar;
  let bool: boolean = ununknownVar;
```

2. Never 타입

모든 타입들의 서브타입(=공집합)

```typescript
  //true example : up cast
  function neverFunc(): never {
    while(true) {};
  };

  let num: number = neverFunc();
  let str: string = neverFunc();
  let bool: boolean = neverFunc();

  //false example : down cast
  let never1: never = 10;
  let never2: never = 'hello';
  let never3: never = true;
```

3. Void 타입

undefinde 타입의 슈퍼타입
```typescript
  //undefinde 타입의 up cast
  function voidFunc(): void {
    console.log('hi');
    return undefined;
  }

  let voidVar: void = undefined;
```
4. Any 타입

Unknown 서브타입이지만,
치트키 타입으로 타입 계층도를 무시

- 모든 타입의 슈퍼타입이 되고,
- Never 타입을 제외한 모든 타입의 서브타입이 됨.

    타입 계층도를 무시하는 치트키 타입이기때문에 웬만하면 사용하지 않는 것을 권유

```typescript
  let unknownVar: unknown;
  let anyVar: any;
  let undefinedVar: undefined;

  //any타입은 down cast와 up cast가 가능
  anyVar = unknownVar;
  undefinedVar = anyVar;
```

#### 타입 계층도 - 객체 타입간의 호환성

특정 객체 타입을 다른 객체 타입으로 취급해도 호환이 되는가

```typescript
type Book = {
  name: string;
  price: number;
};

type ProgramingBook = {
  name: string;
  price: number;
  skill: string;
};

let book: Book;

let programingBook: ProgramingBook = {
  name: '',
  price : 0,
  skill: '',
};

book = programingBook;

//false example - down cast
programingBook = book;
```

초과 프로퍼티 검사

>객체 리터럴을 사용할 때, 실행되는 검사

```typescript
type Book = {
  name: string;
  price: number;
};

type ProgramingBook = {
  name: string;
  price: number;
  skill: string;
};

let programingBook: ProgramingBook;

let skiilBook: Book = {
  name: '',
  price : 0,
  skill: '',  //프로퍼티 갯수를 초과하여 false
};

//초과 프로퍼티 검사를 해도 허용되는 경우
let stroyBook: Book = programingBook;
```