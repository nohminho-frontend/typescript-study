### 유틸리티 타입

제네릭, 맵드, 조건부 타입 등의 타입 조작 기능을 이용해 실무에서 사용되는 타입을 미리 만들어 놓는 것

- 맵드 타입 기반의 유틸리티 타입
- 조건부 타입 기반의 유틸리티 타입

#### 맵드 타입 기반의 유틸리티 타입

- Partial<T>
- Required<T>
- Readonly<T>
- Pick
- Omit
- Record

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