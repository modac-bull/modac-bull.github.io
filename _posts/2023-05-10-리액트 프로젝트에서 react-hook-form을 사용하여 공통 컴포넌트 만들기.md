---
title: 리액트 프로젝트에서 react-hook-form을 사용하여 공통 컴포넌트 만들기
exerpt: "리액트 프로젝트에서 react-hook-form을 사용하여 공통 컴포넌트 만들기"
last_modified_at: 2023-05-10T22:02:06
header:
  teaser: "assets/images/teaser-image1.png"
  overlay_image: /assets/images/teaser-full-width.png
  overlay_filter: 0.1 # same as adding an opacity of 0.5 to a black background
  caption: "instagram. @modacbull"
categories:
  - react
tags:
  - react-hook-form
  - react
toc: true
toc_sticky: true
share: false
related: false
comments: false

---

# 리액트 프로젝트에서 react-hook-form을 사용하여 공통 컴포넌트 만들기

실무에서 리액트 프로젝트를 진행하면서 폼 컴포넌트를 다루는 일이 많았다. 순수 리액트만으로 폼을 관리하기는 번거롭다. 폼 컴포넌트를 효율적으로 관리하기 위해 react-hook-form 라이브리를 선택했다. 그리고 재사용 가능한 공통 폼 컴포넌트를 개발했다. 이번 글은 react-hook-form을 사용하여 공통 폼 컴포넌트 만드는 과정을 기록하고자 한다.

## react-hook-form 소개

폼이 많아질 수록 리액트로 관리하기는 번거롭다고 판단했다. 폼 관리를 react-hook-form을 선택하게 된 이유는 다음과 같다. React Hook Form은 리액트에서 폼을 쉽게 관리할 수 있도록 도와주는 라이브러다. 이 라이브러리를 사용하면 입력값의 유효성 검사, 에러 처리 등을 간편하게 처리할 수 있다.

1. 높은 성능

   react-hook-form은 최소한의 리렌더링으로 높은 성능을 제공한다. 각 입력 필드에서 이벤트 리스너를 통해 값을 수집하므로, 필드가 업데이트될 때마다 전체 폼이 리렌더링 되지 않는다. 이로 인해 대규모 양식의 경우에도 성능을 향상시킬 수 있다.

2. 유효성 검사와 에러 처리

   react-hook-form은 유효성 검사를 쉽게 수행할 수 있습니다. 각 입력 필드에 대한 검사 규칙을 설정하고, 해당 규칙에 따라 에러 메시지를 출력할 수 있습니다. 이를 통해 폼 유효성 검사를 간편하게 처리할 수 있습니다.

3. Hook을 활용한 간편한 폼 관리

   리액트 훅을 사용하여 폼 관리를 효과적으로 처리할 수 있다. useForm이라는 훅을 통해 여러 가지 폼 관련 메소드와 속성을 제공한다. 이로써 클래스 컴포넌트에서의 복잡한 폼 관리 작업을 줄이고, 함수형 컴포넌트에서 쉽게 폼을 다룰 수 있다.

4. 외부 라이브러리와의 호환성

   react-hook-form은 다른 유효성 검사 라이브러리 (예: Yup, Joi, Superstruct)와도 호환되어 사용할 수 있습니다. 이를 통해 개발자가 선호하는 라이브러리를 선택하여 사용할 수 있으며, 기존 프로젝트에 쉽게 통합할 수 있다고 한다. (실무에서는 사용해보지 않았다.)

   위와 같은 이유로 react-hook-form 라이브러리를 사용하여 공통 컴포넌트화 작업을 진행했다.

## 설치 및 설정

루트 디렉토리에서 터미널 명령어 실행.

```bash
npm install react-hook-form
```

## 간단한 사용 방법 소개 - useForm 훅

### useForm 훅 사용하기

useForm 훅으로 input 태그 관리하기. 간다한 사용 방법을 소개한다. 그 외 자세한 사용 방법은 공식 사이트를 참고하면 된다.
(공식 사이트)

- register
  - register란? - useForm에 폼을 관리하겠다고 등록한다.
- validation 방법
  - required 소개
- error 표시 방법
  - error 확인하는 방법, 노출하는 방법 소개
- handleSubmit
  - 전송할 때 register 한 폼들 확인가능

```javascript
import React from "react";
import { useForm } from "react-hook-form";

function LoginForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="email">이메일</label>
      <input {...register("email", { required: "이메일을 입력해주세요." })} />
      {errors.email && <p>{errors.email.message}</p>}

      <label htmlFor="password">비밀번호</label>
      <input
        {...register("password", { required: "비밀번호를 입력해주세요." })}
      />
      {errors.password && <p>{errors.password.message}</p>}

      <button type="submit">로그인</button>
    </form>
  );
}
```

## 3. 공통 폼 컴포넌트 만들기

### 공통 컴포넌트로 만들게 된 이유

폼 컴포넌트를 공통 컴포넌트화 하여 코드 재사용을 향상 시키고, 개발자들 끼리의 협업을 향상 시키고자 했다. 디자인 시스템의 하나인 atom system을 차용하여 최소 단위의 컴포넌트를 구성하고 이를 조합하여 폼 컴포넌트를 공통 컴포넌트화 해보자는 관점에서 시작하게 되었다.

Input 태그를 기준으로 소개한다. 추후에 Textarea, checkbox, radio 컴포넌트들도 공통 컴포넌트화 작업을 진행한 과정을 기록으로 남겨보자.

### 폴더 구조

폴더 구조 기술

### 컴포넌트 구조

컴포넌트 구조 기술

### 가장 작은 단위의 폼 태그 컴포넌트 정의 - Input, Textarea

Input.js - atom

```javascript
// Input.js
```

### 폼 컴포넌트 + 에러문구 - molecules

InputForm.js - molecules (atom + error)

```javascript
// InputForm.js
```

### 사용 예시

페이지 컴포넌트 - 일반, 유효성 검사, 에러 문구 표기

```javascript
// page-ex.js
```

## 4. 결론 / 한계점 / 개선 방향

디자인 시스템 차용하여 react-hook-form 을 활용한 폼 컴포넌트를 개발해봤다.

폼 컴포넌트를 공통 컴포넌트화 한 이후 개발 과정과 사용 방법을 공유한 시점 이후로는 폼 컴포넌트 사용 관련해서는 개발자 간 협업이 향상되었다고 생각한다.

한계점으로는 해당 공통 컴포넌트화를 함으로써 react-hook-form에서 제공하는 다양한 기능들이을 일부분 커스텀하기 어렵다는 점, 해당 컴포넌트의 사용법에 익숙해야 한다는 점, react-hook-form 없이 폼 태그만 단독으로 사용할 수 없다는 점 드이 있다.

폼 컴포넌트를 공통 컴포넌트화 하면서 느낀 한계점을 개선하고자 한다.
