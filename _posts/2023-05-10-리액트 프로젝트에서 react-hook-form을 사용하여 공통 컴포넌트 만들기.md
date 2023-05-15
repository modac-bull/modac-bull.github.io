---
title: 리액트와 react-hook-form을 사용하여 공통 폼 컴포넌트 만들기
exerpt: "리액트 프로젝트에서 react-hook-form을 활용하여 공통 폼 컴포넌트 만들기"
last_modified_at: 2023-05-15T22:02:06
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

실무에서 리액트 프로젝트를 진행하면서 폼 컴포넌트를 다루는 일이 많았다. 순수 리액트로 폼을 관리하기는 번거로웠다. 폼 컴포넌트를 효율적으로 관리하기 위해 react-hook-form 라이브리를 선택했다. 더 나아가 재사용 가능한 공통 폼 컴포넌트를 개발했다. 이번 글은 react-hook-form을 사용하여 공통 폼 컴포넌트 만드는 과정을 기록하고자 한다.

## react-hook-form 소개

폼이 많아질 수록 리액트로 폼의 상태를 각각 관리하기는 상당히 번거로웠다. 폼 관리를 하기 위해 react-hook-form을 선택하게 된 이유는 다음과 같다. React Hook Form은 리액트에서 폼을 쉽게 관리할 수 있도록 도와주는 라이브러다. 이 라이브러리를 사용하면 입력값의 유효성 검사, 에러 처리, 폼 상태 관리 등을 간편하게 처리할 수 있다.

1. 높은 성능

   react-hook-form은 최소한의 리렌더링으로 높은 성능을 제공한다. 각 입력 필드에서 이벤트 리스너를 통해 값을 수집하므로, 필드가 업데이트될 때마다 전체 폼이 리렌더링 되지 않는다. 이로 인해 대규모 양식의 경우에도 성능을 향상시킬 수 있다.

2. 유효성 검사와 에러 처리

   react-hook-form은 유효성 검사를 쉽게 수행할 수 있다. 각 입력 필드에 대한 검사 규칙을 설정하고, 해당 규칙에 따라 에러 메시지를 출력할 수 있다. 이를 통해 폼 유효성 검사를 간편하게 처리할 수 있다.

3. Hook을 활용한 간편한 폼 관리

   리액트 훅을 사용하여 폼 관리를 효과적으로 처리할 수 있다. useForm이라는 훅을 통해 여러 가지 폼 관련 메소드와 속성을 제공한다. 이로써 클래스 컴포넌트에서의 복잡한 폼 관리 작업을 줄이고, 함수형 컴포넌트에서 쉽게 폼을 다룰 수 있다.

4. 외부 라이브러리와의 호환성

   react-hook-form은 다른 유효성 검사 라이브러리와도 호환되어 사용할 수 있습니다. 이를 통해 개발자가 선호하는 라이브러리를 선택하여 사용할 수 있으며, 기존 프로젝트에 쉽게 통합할 수 있다고 한다. (실무에서는 사용해보지 않았다.)

   위와 같은 이유로 react-hook-form 라이브러리를 사용하여 공통 컴포넌트화 작업을 진행했다.

## 설치 및 설정

루트 디렉토리에서 터미널 명령어 실행.

```bash
npm install react-hook-form
```

## 간단한 사용 방법 소개 - useForm 훅

### useForm 훅 사용하기

useForm 훅으로 input 태그 관리하기. 간단한 사용 방법을 소개한다. 그 외 자세한 사용 방법은 react-hook-form 공식 사이트[^1]를 참고하면 된다.
(공식 사이트)

[^1]: <https://react-hook-form.com/get-started/>

 useForm 훅은 React Hook Form의 핵심 기능 중 하나로, 폼 데이터와 유효성 검사에 필요한 다양한 기능들을 제공한다. 

- register
  
  register 함수는 React Hook Form에 필드를 등록하는 데 사용된다. 이 함수를 사용하여 각각의 입력 필드를 React Hook Form에 등록함으로써, 해당 필드의 값을 추적하고 유효성 검사에 사용할 수 있다. register 함수는 폼 컴포넌트의 ref와 함께 사용되며, 필드의 이름과 추가 구성 옵션을 전달한다.

- validation 방법

  다양한 유효성 검사 규칙을 지원한다. 유효성 검사 규칙은 register 함수의 두 번째 매개변수로 설정할 수 있다. 예를 들어, required, minLength, pattern 등의 규칙을 사용할 수 있다. 유효성 검사 규칙은 폼 데이터가 제출될 때 자동으로 실행되며, 검증에 실패한 경우에는 에러가 생성된다.

- error 표시 방법

  error 객체는 유효성 검사를 통과하지 못한 필드의 에러 메시지를 포함한다. 에러 메시지는 각각의 필드 이름을 키로 가지고 있으며, errors 객체를 통해 접근할 수 있다.

- handleSubmit

  handleSubmit 함수는 폼의 제출 이벤트를 처리하는 데 사용된다. 폼 컴포넌트에서 제출 버튼을 클릭하거나 엔터 키를 누를 때 handleSubmit 함수가 호출되며, 등록된 필드의 유효성 검사를 실행한 후 제출 로직을 수행한다.

### 예시 코드
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

      // 이메일 입력 폼 컴포넌트
      <label htmlFor="email">이메일</label>
      // register로 입력 필드 등록하고 필수 값으로 입력 받겠다는 것을 알린다. 
      <input {...register("email", { required: "이메일을 입력해주세요." })} /> 

      // 유효성 검사를 통과하지 못할 경우에 에러 문구를 표기한다.
      {errors.email && <p>{errors.email.message}</p>}

      // 비밀번호 입력 폼 컴포넌트
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

### 개발 이유

폼 컴포넌트를 공통 컴포넌트화 하여 코드 재사용을 향상 시키고, 개발자들 끼리의 협업을 향상 시키고자 했다. 디자인 시스템의 하나인 atmoic design system[^2]을 차용하여 최소 단위의 컴포넌트를 구성하고 이를 조합하여 폼 컴포넌트를 공통 컴포넌트화 해보자는 관점에서 시작하게 되었다.

Input 태그를 기준으로 소개한다. Textarea는 과정이 비슷하기에 생략한다. 추후에는 checkbox, radio 태그도 폼 공통 컴포넌트화 과정을 기록으로 남겨볼 예정이다.

[^2]: <https://atomicdesign.bradfrost.com/chapter-2/#the-part-and-the-whole/>

### 개발 방향
가장 작은 단위의 컴포넌트(atom)는 단일 폼태그 컴포넌트(Input, Textarea, Checkbox, Radio 등)와 에러문구를 표기하는 컴포넌트로 정의했다. 단인 폼태그 컴포넌트와 에러 문구 컴포넌트(ErrorMessage)를 묶은 것을 molecules 컴포넌트로 구성해서 개발했다.


### 폴더 구조

```
===================================================
├── components
|   └── common
|   └── Form
|   |   └── index.js => import 취합
|   |   └── Input.js
|   |   └── InputForm.js
|   |   └── Textarea.js
|   |   └── TextareaForm.js
|   |   └── Checkbox.js
|   |   └── CheckboxForm.js
|   |   └── Radio.js
|   |   └── RadioForm.js
|   |   └── ErrorMessage.js => 에러 메세지 컴포넌트
|   |   └── ...
===================================================
```

### 컴포넌트 구조

컴포넌트 구조는 다음과 같다. 

```
===================================================
├── InputForm.js => input 태그 컴포넌트 + 메세지 컴포넌트 (molecules)
|   └── Input.js => input 태그 컴포넌트 (atom)
|   └── ErrorMessage.js => 메세지 컴포넌트 (atom)
===================================================
```

### 가장 작은 단위(atom)의 컴포넌트 정의 (Input, ErrorMessage) 
#### 1. Input
```jsx
// 코드 주석
const Input = forwardRef(
  ({ id, name, label, type, placeholder, ...props }, ref) => {
    return (
      <StyledInput 
        id={id}
        name={name}
        aria-label={label} // 생략 가능
        type={type}
        placeholder={placeholder}
        ref={ref}
        {...props}
      />
    );
  },
);

const StyledInput = styled.input`
 // ... 스타일
`

export default Input;
```
useForm 훅을 사용하기 위해 register 하는 단계에서 필요한 속성 값들을 props로 전달받는다. register 과정에서 ref가 활용되기 때문에  Input 태그 컴포넌트는 forwardRef로 생성해야 한다.

##### 폼 컴포넌트 + 에러문구 - molecules

InputForm.js - molecules (Input + error)

```jsx
export default function InputForm({
  name, 
  register,
  errors,
  rules,
  size,
  ...props
}) {
  const errorMessages = errors[name] ? errors[name].message : '';
  const hasError = !!(errors && errorMessages);

  return (
    <div
      css={[
        tw`w-full`,
        size === 'default' &&
          css`
            width: 400px;
          `,
      ]}
    >
      <Input
        name={name}
        errorstyle={hasError}
        {...props}
        {...(register && register(name, rules))}
      />
      {hasError && <ErrorMessage>{errorMessages}</ErrorMessage>}
    </div>
  );
}
```
Input 태그에 useForm 훅에 필요한 props를 전달받아 Input 태그에 전달해주는 과정이 필요하다. 유효성 검사를 통과하지 못했을 경우 에러 문구 컴포넌트가 렌더링 되도록 처리한다.


##### 사용 예시

예시 페이지 컴포넌트에서 InputForm 컴포넌트를 사용하는 방법은 다음과 같다. 

```jsx
import { useForm } from 'react-hook-form';

import tw, { styled, css, theme } from 'twin.macro';
import { InputForm }from '@components/common/Form';
import Button from '@components/common/Button';

// 패턴 예시
const emailPattern = {
  value: new RegExp('^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+.[a-zA-Z]{2,4}$', 'ig'),
  message: 'Enter a valid email address.',
};

export default function FormExamplePage() {
  // react-hook-form 훅 사용예시
	const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = handleSubmit((data) => {
    console.log('submitting');
    console.log(data);
  });

  return (
		<form onSubmit={onSubmit}>
			<InputForm
			  id="input1"
			  name="title1"
			  label="인풋1"
			  type="text"
			  placeholder="인풋1 - 내용을 입력해주세요."
			  register={register}
			  rules={{ required: '필수 입력입니다' }} // 필수 입력일 경우 rules객체 props전달
			  errors={errors} // 에러 메세지 사용하기 위해 errors props전달
			/>

			<InputForm
			  id="input2"
			  name="input2"
			  label="인풋2"
			  type="text"
			  size="default" // 너비 400px로 고정할 경우 size="default" props전달
			  placeholder="인풋2 - 내용을 입력해주세요."
			  register={register}
			  rules={{
			    required: 'You must enter your email.',
			    pattern: emailPattern, // 이메일 패턴 사용 예시
			  }}
			  errors={errors}
			/>
		
			<Button type="submit" size={'md'} variant={'point'}>
		    버튼
		  </Button>
		 
		</form>
  );
}
```

## 4. 결론 / 한계점 / 개선 방향
아토믹 디자인 시스템 개념과 react-hook-form 라이브러리를 활용하여 폼 태그를 공통 컴포넌트로 개발해보았다. 

공통 폼 컴포넌트를 개발한 이후 초창기 시점에는 팀원들과 개발 과정을 공유하고 사용 방법을 공유하는 시간을 필요로 했다. 이후로는 공통 폼 컴포넌트를 활용하는 측면에서는 효율성이 어느정도 향상되었다고 생각한다.

한계점으로는 해당 공통 컴포넌트화를 함으로써 react-hook-form에서 제공하는 다양한 기능들을 일부분 커스텀하기 어렵다는 점, 공통 폼 컴포넌트의 사용법을 학습해야 한다는 점, react-hook-form 없이 폼 태그만 단독으로 사용할 수 없다는 점 등이 있다. 

재사용이 용이하면서 커스텀도 어느정도 자유로운 공통 컴포넌트를 개발하는 것은 어려운 일임을 느꼈다. 어느정도 자유성을 보장하기 위한 공통 컴포넌트를 만들기 위해서 고려해야 할 것들이 참 많은 것 같다. 

