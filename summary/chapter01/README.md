# 01장 타입스크립트와 개발 환경 만들기

<details><summary>Table of Contents</summary>

-   타입스크립트란 무엇인가? [🔗](#01-1-타입스크립트란-무엇인가)
    -   자바스크립트의 종류 [🔗](#자바스크립트의-종류)
    -   타입 기능의 필요성 [🔗](#타입-기능의-필요성)
    -   트랜스파일 [🔗](#트랜스파일)
-   타입스크립트 주요 문법 살펴보기 [🔗](#01-2-타입스크립트-주요-문법-살펴보기)
    -   ESNext 주요 문법 살펴보기 [🔗](#esnext-주요-문법-살펴보기)
        -   비구조화 할당 [🔗](#1-비구조화-할당)
        -   화살표 함수 [🔗](#2-화살표-함수)
        -   클래스 [🔗](#3-클래스)
        -   모듈 [🔗](#4-모듈)
        -   생성기 [🔗](#5-생성기)
        -   Promise와 async/await 구문 [🔗](#6-promise와-asyncawait-구문)
    -   TypeScript 고유 문법 살펴보기 [🔗](#typescript-고유-문법-살펴보기)
        -   타입 주석과 타입 추론 [🔗](#1-타입-주석과-타입-추론)
        -   인터페이스 [🔗](#2-인터페이스)
        -   튜플 [🔗](#3-튜플)
        -   제네릭 타입 [🔗](#4-제네릭-타입)
        -   대수 타입 [🔗](#5-대수-타입)
-   타입스크립트 개발 환경 만들기 [🔗](#01-3-타입스크립트-개발-환경-만들기)
    -   Typescript 사용에 필요한 설정 [🔗](#typescript-사용에-필요한-설정)
    -   Typescript 컴파일러 설치 [🔗](#typescript-컴파일러-설치)
    -   Typescript 컴파일과 실행 [🔗](#typescript-컴파일과-실행)

</details>

## 01-1 타입스크립트란 무엇인가?

타입스크립트는 마이크로소프트가 개발하고 유지하고 있는 오픈 소스 프로그래밍 언어

### 자바스크립트의 종류
<div align="center">
    <img src="./images/js-relationship.png" alt="JS Relationships" width="200">
</div>

1. **ES5**
2. **ESNext**: ES5의 모든 문법을 포함한다
3. **TypeScript**: ESNext + xkdlq rlsmd

### 타입 기능의 필요성
대규모 프로젝트에서 타입스크립트의 타입 기능을 통해 자바스크립트에서 잡기 힘든 오류를 쉽게 찾아낼 수 있다

```javascript
function makePerson(name, age) {}

makePerson(32, "Jack") // Error cannot easily be caught
```

```typescript
function makePerson(name: string, age: number) {}

makePerson(32, "Jack") // Error caught by TS compiler
```

### 트랜스파일
ESNext 자바스크립트가 바벨(Babel) 트랜스파일러(transpiler)를 거쳐서 ES5 자바스크립트 코드로 변환  
타입스크립트는 TSC(TypeScript Compiler)를 거쳐서 ES5 자바스크립트 코드로 변환  
  
**트랜스파일러와 컴파일러의 차이**  
컴파일러는 소스 코드를 바이너리 코드로 변환  
트랜스파일러는 소스 코드를 다른 프로그래밍 언어의 소스 코드로 변환

[[🔝위로가기]](#01장-타입스크립트와-개발-환경-만들기)

## 01-2 타입스크립트 주요 문법 살펴보기

타입스크립트 문법 = ESNext의 문법 + 타입스크립트 고유 문법

### ESNext 주요 문법 살펴보기
#### 1. 비구조화 할당
비구조화 할당(deconstruction assignment)는 객체와 배열에 적용할 수 있다.  
비구조화 할당을 통해 객체에서 멤버 추출, 배열 분해, 교환(swap)을 용이하게 할 수 있다.

```javascript
// Extract member from object
let person = { name: "Janve", age: 22 };
let { name, age } = person;

// Split list
let array = [1, 2, 3, 4];
let [head, ...rest] = array;

// Swap
let a = 1, b = 2;
[a, b] = [b, a];
```

#### 2. 화살표 함수
`function` 키워드 대신에 `=>`로 함수를 선언해서 간결하게 만든다.

```javascript
function add(a, b) { return a + b; }
const add = (a, b) => a + b;
```

#### 3. 클래스
클래스 기능을 통해서 객체 지향 프로그래밍 지원  
캡슐화(capsulation), 상속(inheritance), 다옇성(polymorphism) 지원

```javascript
abstract class Animal {
    constructor(public name?: string, public age?: number) { }
    abstract say(): string
}

class Cat extends Animal {
    say() { return '야옹' }
}

class Dag extends Animal {
    say() { return '멍멍' }
}

let animals: Animal[] = [new Cat("야옹이", 2), new Dog("멍멍이", 3)]
let sounds = animals.map(a => a.say())
```

#### 4. 모듈
모듈을 통해 코드를 여러 개의 파일로 분할해서 `import`와 `export` 키워드로 관리
```javascript
import * as fs from 'fs'
export function writeFile(filepath: string, content: any) { }
```

#### 5. 생성기
iterable, iterator, generator를 지원한다.  
`function*`과 `yield` 구문을 활용해서 생성기(generator)를 만들 수 있다.

```javascript
function* gen() {
    yield* [1, 2]
}

for (let value of gen()) {
    console.log(value) // 1, 2
}
```

#### 6. Promise와 async/await 구문
ES5에서 불편했던 비동기 콜백 함수(asynchronous callback function)을 `Promise`로 쉽게 지원  
C# 4.5 버전의 `async/await` 구문을 빌려서 여러 개의 `Promise` 호출도 간결하게 구현 가능하도록 지원

```javascript
async function get() {
    let values = []
    values.push(await Promise.resolve(1))
    values.push(await Promise.resolve(2))
    values.push(await Promise.resolve(3))
    return values
}

get().then(values => console.log(values)) // [1, 2, 3]
```

### TypeScript 고유 문법 살펴보기

#### 1. 타입 주석과 타입 추론

타입스크립트는 `:` 뒤에 타입의 이름을 타입 주석으로 정의했다.  
하지만 타입스크립트에서 타입 주석은 생략될 수 있으며, 이러한 경우 오른쪽 값을 기준으로 타입이 추론된다.

```typescript
let n: number = 1;
let m = 2;
```

#### 2. 인터페이스

타입스크립트는 인터페이스 구문을 지원한다

```typescript
interface Person {
    name: string,
    age?: number
}

let person: Person = {name: "Jane"}
```

#### 3. 튜플

튜플은 배열과 다르게 데이터 타입이 달라도 아이템을 저장할 수 있는 자료구조이다.

```typescript
let numberArray: number[] = [1, 2, 3]
let tuple: [boolean, number, string] = [true, 1, 'OK']
```

#### 4. 제네릭 타입

제네릭 타입은 다양한 타입을 한 번에 처리할 수 있도록 지원하는 문법이다.

```typescript
class Container<T> {
    constructor(public value: T) { }
}

let numberContainer: Container<number> = new Container<number>(1)
let stringContainer: Container<string> = new Container<string>("Hello world")
```

#### 5. 대수 타입

ADT(Abstract Data Type or Algebrain Data Type)을 지원한다

```typescript
type NumberOrString = number |string
type AnimalAndPerson = Animal &Person
```

[[🔝위로가기]](#01장-타입스크립트와-개발-환경-만들기)

## 01-3 타입스크립트 개발 환경 만들기

### Typescript 사용에 필요한 설정

설치 프로그램으로 `scoop`을 사용한다. MacOS인 경우 `brew` 사용 가능하다. 설치 프로그램을 통해 `node`를 설치한다
```shell
$ brew install node
```

### Typescript 컴파일러 설치

`node`를 설치하면 패키지 관리 도구로 `npm`을 사용할 수 있다. `npm`으로 `typescript` 패캐지를 설치한다.
```shell
$ npm install -g typescript
$ tsc -v
```

### Typescript 컴파일과 실행
`.ts` 확장자 파일을 생성하고 `tsc` 명령을 통해 타입스크립트 파일을 자바스크립트 파일로 트랜스파일한다.  
트랜스파일된 자바스크립트 파일은 `node`로 실행할 수 있다.
```shell
$ tsc hello.ts
$ node hello.js
```

타입스크립트를 자바스크립트로 트랜스파일하고 실행하기 위해서는 `ts-node` 패키지를 설치해서 사용한다.
```shell
$ npm install -g ts-node
$ ts-node hello.ts
```

[[🔝위로가기]](#01장-타입스크립트와-개발-환경-만들기)