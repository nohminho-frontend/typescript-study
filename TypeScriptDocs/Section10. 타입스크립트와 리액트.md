### React With Typescript

#### 리액트에 타입스크립트 기초 설정
1. 리액트 앱설치

> npx create-react-app . : 현재 경로에 설치하기를 위해 .을 붙혀야 함

2. 타입 선언 패키지 설치

> npm i @types/node @types/react @types/react-dom @types/jest

package.json 파일과 node_modules/types 경로에 추가된 패키지 확인 가능

3. 컴파일러 기본옵션 추가

  - tsconfig.json 파일 생성

  - 해당 파일에 기본 옵션 추가
    > 타입스크립트는 기본적으로 js확장자로부터 jsx문법을 해석할 수 없기때문에 src 경로에 js확장자를 jsx로 수정해야함.

  ```json
    {
      "compilerOptions": {
        "target": "ES5",
        "module": "CommonJS",
        "strict": true,
        "allowJs": true,
      },
      "include": ["src"]
    }
  ```

#### jsx 확장자 파일을 tsx 확장자 파일로 수정

1. index.jsx와 App.jsx 파일을 .tsx 확장자로 수정

- tsconfig.json에 새로운 옵션 추가

    > "esModuleInterop": true -> default로 내보낸 값이 없는 모듈에서도 값을 허용하는 옵션
    > "jsx" : "react-jsx" -> 타입스크립트 컴파일러가 jsx를 해석할 수 있도록 하는 옵션
```json
    {
      "compilerOptions": {
        "target": "ES5",
        "module": "CommonJS",
        "strict": true,
        "allowJs": true,
        "esModuleInterop": true,
        "jsx" : "react-jsx",
      },
      "include": ["src"]
    }
```

- root 변수 선언 코드 수정

  > getElementById 메서드가 nulll 타입을 반환 할 수도 있는데 creatRoot 메서드는 null 타입의 반환값을 인수로 받지 않기 때문에 수정

```typescript
  //수정 전
  const root = ReactDOM.createRoot(document.getElementById('root'));
  //수정 후
  const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
```