title: 전역 상태 관리의 트렌드 변화 : 초소형 전역 상태 관리
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->
.bottom-bar[
  {{title}}
]

---

class: impact

## {{title}}
### 김나람 @ 그로테스큐

---

class: impact

## 개발 트렌드의 방향 : 더 짧게! 더 빠르게!

---

# 리액트 : 함수 컴포넌트의 도입

* Stateless 한 컴포넌트를 목적으로 처음 도입

* State 가 존재하는 컴포넌트는 클래스 컴포넌트로 제작되며 병행 사용

* 리액트 16 버전에서 훅 hooks 도입

* 함수 컴포넌트가 상태를 가질 수 있게 되며 본격적으로 사용이 확대됨

---

# 함수 컴포넌트 도입 초기

* 클래스 컴포넌트는 라이프 싸이클에 직접 관여 가능  
  * getDerivedStateFromProps
  * shouldComponentUpdate
  * 등등...

* 고성능이 필요한 경우에는 클래스 컴포넌트를 사용하고, 단순 컴포넌트는 함수 컴포넌트가 사용될 것이라고 예상

---

# 함수 컴포넌트 도입 결과

* 신규 프로젝트는 거의 대부분 함수 컴포넌트 위주로 개발

* 리액트 팀에서도 함수 컴포넌트의 성능 개선에 무게를 두고 있음

* 대부분의 기술 문서들이 함수 컴포넌트 위주로 작성됨

* 구인글에 “함수 컴포넌트로만 개발함" 이라는 문구 등장

* 클래스 컴포넌트를 설명하는 게시물 대부분이 “옛날 코드의 유지보수를 위해 배워둘 것" 이라고 설명

---

# Clean Code에서 권고하는 함수 작성 원칙

* 함수는 작게 만들어라. 짧을 수록 좋다.

* 한 가지 일을 잘 하는 함수를 만들어라.

---

# 컴포넌트 패러다임의 변화

* 클린 코드의 함수 작성 원칙과 비슷

* 한가지 역할을 담당하는 짧은 함수 컴포넌트의 작성

  * 예) 아토믹 디자인 등

---

class: impact

## 초소형 전역 상태 : Micro Global Statement

---

# 초소형 전역 상태 관리 도구의 등장

* Recoil

* Jotai

* Zustand

---

# Recoil

* 페이스북이 발표한 라이브러리

* 공식 라이브러리라는 점에서 크게 주목 받음

* 직접적으로 Context API의 한계에 대해 언급

---

# Recoil : Code

## 선언

```js
const fontSizeState = atom({
  key: 'fontSizeState',
  default: 14,
});
```

---

# Recoil : Code

## 사용

```js
function FontButton() {
  const [fontSize, setFontSize] = useRecoilState(fontSizeState);
  return (
    <button onClick={() => setFontSize((size) => size + 1)}
            style={{fontSize}}>
      Click to Enlarge
    </button>
  );
}
```

---

# Jotai

* 개발그룹 Poimandres 에서 발표

* 일본어로 '상태'를 나타내는 단어

* Recoil 의 Atomic 한 디자인에 영감을 얻었다고 함

---

# Jotai : Code

## 선언

```js
import { atom } from 'jotai';

const countAtom = atom(0);
```

---

# Jotai : Code

## 사용

```js
function Counter() {
  const [count, setCount] = useAtom(countAtom);
  return (
    <h1>
      {count}
      <button onClick={() => setCount(c => c + 1)}>one up</button>
    </h1>
  );
}
```