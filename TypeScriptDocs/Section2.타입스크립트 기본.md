### TypeScript 기본

#### 기본타입

  각각의 타입들은 서로 부모와 자식 관계를 이루게 되며 계층을 형성

1. 타입스크립트가 자체적으로 제공하는 타입 : null, undefined, number, string, object, Array...

2. 자바스크립트에서는 제공하지 않았던 타입 : unknown, any, void, never...

#### 원시타입 (Primitive Type)

  하나의 값만 저장하는 타입

> number, string, boolean, null, undefined

  타입이 다른 값을 저장할 때 tsconfig.json에 strictNullChecks 옵션을 추가

#### 리터럴 타입

  값 자체를 타입으로 정의하는 타입

```typescript
let numA: 10 = 10; //true
let numB: 10 = 11; //false
```

#### 배열 타입

> 배열의 타입을 정의하기 위해서는 배열의 요소의 타입을 먼저 선언

```typescript
let numArr: number[] = [1, 2, 3];
let strArr: string[] = ['hello', 'world'];

//제네릭 문법
let boolArr: Array<boolean> = [true, false];
```

> 배열에 들어가는 요소들의 타입이 다양할 경우

```typescript
let multiArr: (number | string)[] = [1, 'hello'];
```

> 다차원 배열의 타입을 정의하는 방법

```typescript
let doubleArr: number[][] = [
  [1, 2, 3],
  [4, 5],
];
```

#### 튜플 타입

  길이와 타입이 고정된 배열

  자바스크립트에서는 제공하지 않고 타입스크립트에서만 제공

```typescript
let tup1: [number, number] = [1, 2];
let tup2: [number, string, boolean] = [1, "2", false];

//길이가 다르고 타입이 같은 배열 
tup1 = [1, 2, 3]; //false

//길이는 같지만 타입이 다른 배열
tup1 = ['1', '2'];  //false

//길이가 같지만 배열의 타입 순서가 다른 배열
tup2 = ['2', 1, true];  //false
```

  튜플은 자바스크립트 코드로 컴파일 되었을 때는 배열로 변환되서 push(), pop() 메서드 사용이 가능

    -> push(), pop() 메서드를 사용할 때는 배열의 길이 신경을 쓰지 않기 때문에 주의를 요함

  튜플을 사용하기 좋은 경우

  - 인덱스의 위치에 따라 넣어야하는 값이 정해져있을 경우

  - 인덱스의 순서를 지키는게 중요한 경우

```typescript
const users: [string, number][] = [
  ['노민호', 1],
  ['김아무개', 2],
  ['이아무개', 3],
  ['박아무개', 4],
]
```

#### 객체 타입

> object

  객체타입을 object로 정의할 때 문제점

    타입스크립트의 object라는 타입은 해당 변수가 객체라는 정보 외에 아무런 정보를 얻을 수 없음.

```typescript
let user: object = {
  id : 1,
  name : '노민호',
};

user.id; //false
```

> 리터럴 객체

  object로 정의할 때 문제점 해결 방법

  -> 구조적 타입 시스템

```typescript
let user: {
  id: number;
  name: string;
} = {
  id: 1,
  name: '노민호',
};

user.id;  //true

선택적(옵셔널) 프로퍼티 예시

let userC: {
  id?: number;  //선택적 프로퍼티
  name: string;
};

userC = {
  name: '민호'
};

읽기전용 프로퍼티

let config: {
  readonly apiKey: string;
} = {
 apiKey : 'Key' 
};

config.apiKey = '123';  //false
```

#### 타입 별칭 & 인덱스 시그니처

1. 타입 별칭

```typescript
type User = {
  id: number;
  name: string;
  nickname: string;
  birth: string;
  bio: string;
  location: string;
};

let user: User = {
  id: 1,
  name: '노민호',
  nickname: '미노',
  birth: '1996.11.27',
  bio: 'hi',
  location: '서울 마포구 대흥동'
};
```

2. 인덱스 시그니처

  Key와 Value의 규칙을 기준으로 객체의 타입을 정의

  해당 규칙을 위반하지만 않으면 모든 객체를 허용하기 때문에 프로퍼티가 없으면 허용

```typescript
type CoutryCodes = {
  [key: string]: string;
};

let coutryCodes1: CoutryCodes = {
  Korea: 'ko',
  UnitedState: 'us',
  Japan: 'jp'
};

let coutryCodes2: CoutryCodes = {};
```

#### Enum 타입

  여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입

  자바스크립트에서는 지원하지 않고 타입스크립트에서만 지원

```typescript
//숫자형 enum
enum Role {
  ADMIN = 0,
  USER = 1,
  GUEST = 2,
}

//문자형 enum
enum Language {
  korea = 'ko',
  engilsh = 'en',
}

const user1 = {
  name: '미노',
  role: Role.ADMIN, // 0 : 관리자
  language: Language.korea,
};

const user2 = {
  name: '아영',
  role: Role.USER, // 1 : 일반유저
  language: Language.korea,
};

const user3 = {
  name: '민아',
  role: Role.GUEST, // 2 : 게스트
  language: Language.engilsh,
};

console.log(user1, user2, user3);
```

#### Any 타입

  특정 변수의 타입을 범용적으로 사용되어 확실히 정할 수 없거나 모를 경우

```typescript
let anyVar = 10;

//anyVar 변수는 number로 타입이 추론되기 때문에 타입 오류 발생
anyVar = 'hello';  //false

let anyVar:any = 10;

let num: number = 10;

anyVar = 'hello';  //true
anyVar = true;  //true
anyVar = {};  //true

num = anyVar;  //true
```

#### Unknown 타입

  any와 같이 모든 타입의 값을 저장할 수 있지만,

  any타입과는 다르게 반대로 타입이 다른 변수에 할당은 해줄 수 없고

  연산 및 메서드를 사용할 수 없으며

  typeof 연산자를 사용하여 unknown 타입의 확실히 밝혀주었을 때만 사용이 가능함

    -> 이러한 과정을 '타입 정제' 또는 '타입 좁히기'라고 함.

```typescript
let unknownVar: unknown;

unknownVar = '';  //true
unknownVar = 1; //true
unknownVar = () => {};  //true

num = unknownVar; //false

unknownVar.toUpperCase();  //false

if(typeof unknownVar === 'number') {
  num = unknownVar; //true
}
```

#### void 타입

  아무것도 없음을 의미하는 타입

  즉, 리턴 값이 아예 없는 경우에 사용하는 타입

```typescript
//undefined로 반환해야함.
function func3(): undefined {
  console.log('hi');
  return undefined;
};
//null로 반환해야함.
function func4(): null {
  console.log('hi');
  return null;
};
//리턴값이 아예 존재하지 않음.
function func2(): void {
  console.log('hi');
};
```
    strictNullChecks 옵션을 false로 설정하면 undefined나 null값을 담을 수 있음.

#### never 타입

  존재하지 않는 즉, 리턴값을 내기가 불가능한 타입

```typescript
//무한루프로 인해 리턴값을 반환 할 수 없을 경우
function func5(): never {
  while(true){ }
};

//실행되었을 경우 프로그램이 바로 종료되는 경우
function func6(): never {
  throw new Error();
};
```

    strictNullChecks 옵션을 false로 설정해도 undefined나 null값 모두 담을 수 없음.

 