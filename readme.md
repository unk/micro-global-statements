# Backslide Template

## 기본 템플릿과 다른 점

* Noto Sans KR 적용 (애플 시스템의 경우 애플 네오 고딕 우선 적용)
* 단어 단위 줄바꿈 적용 (`word-break: keep-all`)
* 기본 행간 넓힘 (`line-height: 1.3`)
* 목록 속 목록 간격 조정 (`margin-top: .5rem`)

## Backslide 설치

최초 1회 `backslide`를 글로벌 설치 해야 합니다.

```shell
npm i -g backslide
```

## 클론

```shell
gh repo clone grotesq/bs-template
# 새 디렉토리명을 지정해서 클론할 수 있습니다.
gh repo clone grotesq/bs-template new-name
```

`cd {디렉토리명}` 명령으로 디렉토리 이동 후 다음 과정으로 넘어갑니다.

## 실행

```shell
bs serve
```

## HTML 내보내기

```shell
bs export presentation.md
```

## PDF 생성

PDF 생성에는 Decktape 패키지가 필요합니다.

```shell
npm i -g decktape
```

```shell
bs pdf presentation.md
```
