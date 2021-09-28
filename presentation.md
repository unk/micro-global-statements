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

  * 주로 너무 많은 리렌더링과 코드 분할의 어려움에 대한 내용

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

# Recoil : 특이사항

* Provider 역할을 하는 RecoilRoot 가 필요

* 일반적으로 프로젝트 루트에 위치

```js
function App() {
  return (
    <RecoilRoot>
      <Componoent />
    </RecoilRoot>
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

---

# Zustand

* 개발그룹 Poimandres 에서 발표

* 독일어로 '상태'를 나타내는 단어

---

# Zustand : Code

## 선언

```js
const useStore = create(set => ({
  bears: 0,
  increasePopulation: () => set(state => ({ bears: state.bears + 1 })),
  removeAllBears: () => set({ bears: 0 })
}));
```

---

# Zustand : Code

## 사용

```js
function Component() {
  const bears = useStore(state => state.bears);
  const increasePopulation = useStore(state => state.increasePopulation);
  return <>
    <h1>{bears} around here ...</h1>
    <button onClick={increasePopulation}>one up</button>
  </>
}
```

---

# Jotai & Zustand

* 둘 다 개발그룹 Poimandres 에서 발표

* Jotai는 Recoil에 가까운 형태

* Zustand는 Redux에 가까운 형태

---

class: impact

## 초소형 전역 상태의 도입으로 달라지는 점들

---

# 기존 전역 상태 관리의 문제

온갖 것들이 모두 들어간 거대한 데이터 덩어리

```js
function App() {
  return <Provider state={
    auth: ...
    service: ...
    others: ...
  }>
    <Component />
  </Provider>
}
```

---

# 기존 전역 상태 관리의 문제

* 기존의 전역 상태 관리 도구도 가능한 상태를 역할별로 자르도록 권고 → 잘 지켜지지 않음
* 이유 : 중첩되는 Provider의 귀찮음 등등...

```js
function App() {
  return <AuthProvider>
    <ServiceProvider>
      <Component />
    </ServiceProvider>
  </AuthProvider>
}
```

**"가능한 것"과 "편리한 것"은 다르기 때문**

---

# 도입 후 : 역할별로 아톰 생성

```js
export const authAtom = atom( ... );

export const serviceAtom = atom( ... );

export const othersAtom = atom( ... );
```

## 전역 상태 관리의 패러다임 변화

클래스 컴포넌트의 this.state 가 함수 컴포넌트의 useState() 로 대체되는 정도의 변화

---

# 남은 고민들

**Redux의 미들웨어들을 대체할 수 있는가?**  
→ 커스텀 훅과 헬퍼 함수들로 가능할 것  
→ 하지만 미들웨어 의존성이 높다면 그냥 Redux를 쓰는 것이...

**성능 향상이 있는가?**  
→ 리렌더링 횟수를 체크해보면 확실히 줄어듬  
→ 사실 기존에도 그렇게 느리지 않았음  
→ 도입한 이유는 성능보다는 개발 생산성 측면이 큼

---

class: impact

## 질문 받습니다

### 감사합니다