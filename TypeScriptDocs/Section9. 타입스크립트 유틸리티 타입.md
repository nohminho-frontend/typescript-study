### 유틸리티 타입

제네릭, 맵드, 조건부 타입 등의 타입 조작 기능을 이용해 실무에서 사용되는 타입을 미리 만들어 놓는 것

- 맵드 타입 기반의 유틸리티 타입
- 조건부 타입 기반의 유틸리티 타입

#### 맵드 타입 기반의 유틸리티 타입

- Partial<T>
- Required<T>
- Readonly<T>
- Pick<T, K>
- Omit<T, K>
- Record<K, V>

1. Partial<T>

특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 바꿔주는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  type Partail<T> = {
    [key in keyof T]?: T[key];
  };

  const draft: Partial<Post> = {
    title: '',
    content: '',
  };
```

2. Required<T>

특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 바꿔주는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  type Required<T> = {
    [key in keyof T]-?: T[key];
  };

  const withThumbnailPost: Required<Post> = {
    title: '',
    tags: [],
    content: '',
    thumbnailURL: '',
  };
```

3. Readonly<T>

특정 객체 타입에서 모든 프로퍼티를 읽기 전용 프로퍼티로 만들어주는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  type Readonly<T> = {
    readonly[key in keyof T]: T[key];
  };

  const readonlyPost: Readonly<Post> = {
    title: '',
    tags: [],
    content: '',
    thumbnailURL: '',
  };
  // 프로퍼티가 읽기전용으로 되어 수정할 수 없음.
  readonlyPost.content = '';
```
4. Pick<T, K>

객체 타입으로부터 특정 프로퍼티만 골라내는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  type Pick<T, K extends keyof T> = {
    [key in K]: T[key];
  };

  const legacyPost: Pick<Post, 'title' | 'content'> = {
    title: '옛날 제목',
    content: '옛날 내용',
  };
```

5. Omit<T, K>

객체 타입으로부터 특정 프로퍼티를 제거하는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;

  const noTitlePost: Omit<Post, 'title'> = {
    tags: [],
    content: '',
    thumbnailURL: '',
  };
```

6. Record<K, V>

동일한 패턴의 객체 타입을 쉽게 정의할 수 있는 타입

```typescript
  interface Post {
    title: string;
    tags: string[];
    content: string;
    thumbnailURL?: string;
  };

  //bad case
  type ThumbnailLegacy = {
    large: {
      url: string;
    };
    medium: {
      url: string;
    };
    small: {
      url: string;
    };
    watch: {
      url: string;
    };
  };

  type Record<K extends keyof any, V> = {
    [key in K]: V;
  }

  type Thumbnail = Record<'large' | 'medium' | 'small' | 'watch',
    { url: string, size: number }
  >;
```

#### 조건부 타입 기반의 유틸리티 타입들

- Exclude<T, U>
- Extract<T, U>
- ReturnType<T>

1. Exclude<T, U>

T 타입에서 U 타입을 제외

```typescript
  type Exclude<T, U> = T extends U ? never : T;

  type A = Exclude<string | boolean, boolean>;
```

2. Extract<T, U>

T 타입에서 U 타입을 추출

```typescript
  type Extract<T, U> = T extends U ? T : never;

  type B = Extract<string | boolean, boolean>;
```

3. ReturnType<T>

함수의 반환값 타입을 추출하는 타입

```typescript
  type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : never;

  function funcA() {
    return 'hello';
  };

  function funcB() {
    return 10;
  };

  type ReturnA = ReturnType<typeof funcA>;  // string
  type ReturnB = ReturnType<typeof funcB>;  // number
```