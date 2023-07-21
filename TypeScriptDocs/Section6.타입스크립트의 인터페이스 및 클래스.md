### 인터페이스

#### 인터페이스란?

상호간에 약속된 규칙

- 타입 별칭과 동일하게 타입에게 이름을 지어주는 또 다른 문법
- 객체의 구조를 정의하는데 특화된 문법 (상속, 합침 등의 특수한 기능을 제공)

    인터페이스로 만든 타입도 타입 별칭으로 만든 타입들과 동일하게 타입 주석을 이용해 변수의 타입을 정의할 때 사용이 가능.


    인터페이스는 타입 별칭과 타입을 정의하는 문법이 다를 뿐 기본적인 기능은 같음.

타입별칭과의 차이점

- 인터페이스에서는 유니온과 인터섹션 타입을 제공하지 않음.

```typescript
  //union false example
  interface Person {
  name: string;
  age: number;
  sayHi(): void;
  sayHi(a: number, b: number): void;
  } | number

  //intersection false example
  interface Person {
  name: string;
  age: number;
  sayHi(): void;
  sayHi(a: number, b: number): void;
  } & number

  /**
   * 인터페이스를 유니온 또는 인터섹션 하기 위해서는
   * 1. 타입 별칭에 활용하거나
   * 2. 타입 주석에 활용
   */
  interface Person {
    name: string;
    age: number;
    sayHi(): void;
    sayHi(a: number, b: number): void;
  };

  // 1. 타입 별칭에 활용
  type UnionType = number | string | Person;
  type IntersectionType = number & string & Person;

  // 2. 타입 주석에 활용
  const person: Person | number= {
    name: '민호',
    age: 28,
    sayHi: function() {
      console.log('hi');
    },
  };
```

#### 인터페이스 확장(상속)

타입 별칭에는 없는 인터페이스의 고유 기능

```typescript
  interface Animal {
    name: string;
    age: number;
  };

  interface Dog extends Animal {
    isBark: boolean;
  };

  const dog: Dog = {
    name: '',
    age: 1,
    isBark: true,
  };

  interface Cat extends Animal {
    isScratch: boolean;
  };

  // 슈퍼타입으로부터 상속받은 프로퍼티를 재정의 가능
  // 타입을 재정의 할 때는 반드시 서브타입 되도록 정의해야함.
  interface Chicken extends Animal {
    name: 'bbq',
    isFly: boolean;
  };

  //다중 확장
  interface DogCat extends Dog, Cat {};

  const dogCat: DogCat = {
    name: '',
    age: 1,
    isBark: true,
    isScratch: false,
  };
```

#### 인터페이스의 선언 합치기(=병합)

타입 별칭과 다르게 동일한 이름의 인터페이스를 선언할 수 있음

라이브러리에 타입 정의가 부실할 경우 사용하게 되는 경우가 많음 (모듈 보강)

-> 동일한 이름으로 정의한 인터페이스들은 결국 다 합쳐짐.

    동일한 프로퍼티를 중복정의 할 때, 타입이 달라지면 안됨.

```typescript
  interface Person {
    name: string,
  };

  interface Person {
    age: number,
  };

  const person: Person = {
    name: '',
    age: 1,
  };

  //false example
  interface Person {
    name: number,
    age: number,
  };
```

### 클래스

#### 자바스크립트의 클래스

똑같은 객체를 간편하게 만들 수 있는 문법

- 클래스를 선언할 때 클래스 내부에 생성자를 정의해줘야 함
- 클래스는 상속을 시킬 수 있음

    인스턴스 : 클래스를 이용해서 만든 객체

```javascript
  // bad case
  let StudentA = {
    name: 'minho',
    grade: 'A',
    age: 28,
    study() {
      console.log('열심히 공부함');
    },
  };

  let StudentB = {
    name: 'gildong',
    grade: 'B+',
    age: 29,
    study() {
      console.log('열심히 공부를 안함');
    },
  };
  // good case
  class Student {
    name;
    grade;
    age;
    
    // 생성자
    constructor(name, grade, age) {
      this.age = age;
      this.grade = grade;
      this.age = age;
    }
  };

  class StudentIntroduce extends Student{
    message;

    // 메서드
    constructor(name, grade, age, message) {
      super(name, grade, age);
      this.message = message;
    }

    study() {
      console.log(`${this.message}`);
    }
  }

  // 인스턴스
  let studentC = new StudentIntroduce('민호', 'A', 28, '열심히 공부함');
  studentC.study();
```

#### 타입스크립트의 클래스

타입스크립트의 클래스는 클래스로도 취급이되면서 하나의 타입으로도 취급을 함.

  클래스의 프로퍼티(필드)에 타입 정의를 해도 오류가 발생하는 경우

    1. 초기값 설정이 되어 있지 않음.

    2. 생성자가 할당되어 있지 않음.

```typescript
  const employeeA = {
    name: '민호',
    age: 28,
    study: 'typescript',
    work() {
      console.log('일하면서 공부 중');
    }
  };

  class Employee {
    name: string;
    age: number;
    study: string;

    constructor(name: string, age: number, study: string){
      this.name = name;
      this.age = age;
      this.study = study;
    }

    work() {
      console.log('일하면서 공부 중');
    }
  };

  // 클래스로 선언
  const employeeB = new Employee('민호', 28, '일하면서 일을 함');
  employeeB.work();

  // 타입으로 선언
  const employeeC: Employee = {
    name: '길동',
    age: 29,
    study: 'javascript',
    work() {

    },
  };

  // 클래스 확장
  class ExecutiveOfficer extends Employee {
    officeRoomNumber : number;

    constructor(
      name: string, 
      age: number, 
      study: string,
      officeRoomNumber: number
      ) {
      super(name, age, study);
      this.officeRoomNumber = officeRoomNumber;
    }
  };
```

#### 클래스의 접근 제어자

클래스를 만들 때 특정 필드나 메서드에 접근할 수 있는 범위를 설정하는 문법

    객체지향 프로그래밍을 할 때 중요한 문법

- public : 아무 제약 없는 상태 (지정하지 않으면 기본으로 적용)
- private : 클래스 내에서만 액세스할 수 있는 상태
- protected : 파생 클래스까지느 액세스할 수 있는 상태

```typescript
  class Employee {
    private name: string;
    protected age: number;
    study: string;

    constructor(name: string, age: number, study: string){
      this.name = name;
      this.age = age;
      this.study = study;
    }

    work() {
      console.log('일하면서 공부 중');
    }
  };

  class ExecutiveOfficer extends Employee {
    officeRoomNumber : number;

    constructor(
      name: string, 
      age: number, 
      study: string,
      officeRoomNumber: number
      ) {
      super(name, age, study);
      this.officeRoomNumber = officeRoomNumber;
    }

    //name은 private 제어자라 파생 클래스에서 사용 불가
    //age는 protected 제어자라 파생 클래스에서도 사용 가능
    introduce() {
      console.log(`${this.name}은 ${this.age}살`)
    }
  };

  const employeeB = new Employee('민호', 28, '일하면서 일을 함');
  employeeB.work();
```

#### 인터페이스와 클래스

인터페이스가 정의하는 타입의 객체를 클래스가 생성하도록 정의하는 문법

    인터페이스는 무조건 public 제어자만 정의할 수 있으므로 클래스에서 private나 protected 제어자로 정의할 수 없음.

    => private나 protected 제어자를 필요로한다면 클래스내에 정의

```typescript
  interface CharacterInterface {
    name: string;
    moveSpeed: number;
    move(): void;
  };

  class Charater implements CharacterInterface {
    constructor(
      public name: string, 
      public moveSpeed: number,
      private extra: string,
      ) {
      this.name = name;
      this.moveSpeed = moveSpeed;
      this.extra = extra;
    }

    move(): void {
      console.log(`${this.moveSpeed} 속도로 이동`);
    }
  };
```