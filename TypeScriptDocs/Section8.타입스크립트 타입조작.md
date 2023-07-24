### Typescript Type Manipulation

기본 타입이나 타입 별칭 또는 인터페이스로 만든 원래 존재하던 타입들을 상황에 따라 유동적으로 다른 타입으로 변환

- 제네릭
- 인덱스드 엑세스 타입
- keyof 연산자
- Mapped 타입
- 템플릿 리터럴 타입
- 조건부 타입

#### 인덱스드 엑세스 타입

객체, 배열, 튜플 타입에서 특정 프로퍼티 혹은 요소의 타입을 추출하는 타입

1. 객체타입에서 사용

```typescript
  interface Post {
    title: string;
    content: string;
    author: {
      id: number;
      name: string;
      age: number;
    };
  }[];

  function printAuthorInfo(author: Post['author']) {
    console.log(`${author.id} - ${author.name}`);
  };

  const post: Post = {
    title: '게시물 제목',
    content: '게시물 본문',
    author: {
      id: 1,
      name: '민호',
      age: 28,
    },
  };
```

2. 배열타입에서 사용

```typescript
  type PostList = {
    title: string;
    content: string;
    author: {
      id: number;
      name: string;
      age: number;
    };
  }[];

  function printAuthorInfo(author: PostList[number]['author']) {
    console.log(`${author.id} - ${author.name}`);
  };

  const post: PostList[number] = {
    title: '게시물 제목',
    content: '게시물 본문',
    author: {
      id: 1,
      name: '민호',
      age: 28,
    },
  };
```

3. 튜플타입에서 사용

```typescript
  type Tup = [number, string, boolean];

  type Tup0 = Tup[0];
  type Tup1 = Tup[1];
  type Tup2 = Tup[2];
```

#### keyof 연산자

특정 객체 타입으로부터 프로퍼티 키들을 모두 스트링 리터럴 유니온 타입으로 추출하는 연산자

```typescript
  interface Person {
    name: string;
    age: number;
    isOld: boolean;
  };

  type PersonKey = keyof Person;

  function getPropertyKey(person: Person, key: PersonKey) {
    return person[key];
  };

  const personKey: Person = {
    name: '',
    age: 0,
    isOld: false,
  };
```

#### Mapped 타입

기존의 객체 타입으로부터 새로운 객체 타입을 만드는 타입

```typescript
  interface User {
    id: number;
    name: string;
    age: number;
  };

  type PartialUser = {
    [key in keyof User]?: User[key];
  }

  type ReadonlyUser = {
    readonly [key in keyof User]: User[key];
  }

  function fetchUser(): ReadonlyUser {
    // ... 기능
    return {
      id: 1,
      name: 'minho',
      age: 27,
    };
  };

  const user = fetchUser();
  // user.age = 12;
```

#### 템플릿 리터럴 타입

스트링 리터럴 타입을 기반으로 정해진 패턴의 문자열만 포함하는 타입

```typescript
  type Company = 'NAVER' | 'KAKAO' | 'LINE' | 'COUPANG' | 'BAEMIN';

  type Employee = 'developer' | 'marketer' | 'desiner';

  type CompanyEmployee = `${Company} - ${Employee}`;

  const companyEmployee: CompanyEmployee = '';
```