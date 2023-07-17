### TypeScript는 JavaScript의 확장판

-> 자바스크립트를 더 안전하게 사용할 수 있도록 "타입 관련 기능들을 추가한" 언어

-> 자바스크립트의 기본적인 모든 문법을 포함한 언어


### TypeSystem

<strong>1. 정적 타입 시스템</strong>

    코드 실행 이전에 모든 변수의 타입을 고정적으로 결정함

    엄격하고 고정적인 시스템

    ex : C, Java


<strong>2. 동적 타입 시스템</strong>

    코드를 실행하고 나서 그때 그때 마다 유동적으로 변수의 타입을 결정함

    자유롭고 유연한 시스템
  
    ex : Python, JavaScript


<strong>3. 점진적 타입 시스템</strong>

    실행 전 검사를 통한 타입의 안정성 확보

    자동으로 변수의 타입을 추론

    ex : TypeScript

---

### VS Code에 TypeScript 환경설정


#### 1. pakage.json이 생성

<br>

> npm init : node js 패키지 초기화


#### 2. types/node 패키지 설치

TypeScript 실습을 위해 node js가 제공하는 내장기능들에 대한 type 정보를 가지고있는 패키지 설치


해당 패키지를 설치하지 않으면 typesrcript가 작성한 코드를 컴파일하는 과정에서 node js의 기본으로 제공하는 기능에 대한 타입을 읽지못하기 때문에 무조건 설치해야함.


> npm i @types/node

#### 3. TypeScript 컴파일러 설치

> npm install typescript -g

    -g : 글로벌 옵션 (컴퓨터 모든 곳에서 해당 패키지를 사용 가능)


#### 4. TypeScript 파일 컴파일

typescript로 작성한 코드를 컴파일

> tsc ./(파일경로)/파일명.ts


#### 5. 그 외


    node : typescript로 작성한 코드를 컴파일하여 js파일이 나오면 해당 js파일을 실행


    ts-node : typescript 파일을 컴파일하여 js를 만들지 않아도 ts파일을 바로 실행가능

---

### TypeScript 컴파일러 옵션 설정


    1. 얼마나 엄격하게 타입 오류를 검사할지
    2. 자바스크립트 코드의 버전을 어떻게 할지
    ...


#### 1. tsconfig.json 생성

  기본적인 옵션이 자동으로 설정된 컴파일러 옵션 파일을 자동으로 생성하는 명령어

  > tsc --init

#### 2. 옵션

``` json
{
  "compilerOptions": {
    "target": "ESNext", //타입스크립트를 컴파일하여 만들어진 자바스크립트 코드의 버전을 설정
    "module": "ESNext", //변환되는 자바스크립트 코드의 모듈 시스템을 설정
    "outDir": "dist", //tsc로 타입스크립트 파일을 컴파일하면 해당 폴더가 아닌 설정한 폴더에 자바스크립트 파일을 생성
    "strict": true, //타입스크립트 컴파일러가 타입을 검사할 때 얼마나 엄격하게 검사하는지에 설정
    "moduleDetection": "force"  //모든 타입스크립트 파일은 전역모듈로 취급되는데 개별 모듈로 취급되도록 설정
  },
  "ts-node": {  
    "esm" : true  //ts-node은 기본적으로 CommonJS를 사용하기때문에 ESM을 모듈로 설정
  },
  "include": ["src"] //컴파일 할 폴더의 범위를 지정
}
```