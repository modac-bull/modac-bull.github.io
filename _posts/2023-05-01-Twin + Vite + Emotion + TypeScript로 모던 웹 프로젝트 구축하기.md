---
title: "Twin + Vite + Emotion + TypeScript로 모던 웹 프로젝트 구축하기"
exerpt: "이 글에서는 Twin, Vite, Emotion, 그리고 TypeScript를 함께 사용하여 모던 웹 프로젝트를 구성하는 방법, 이러한 구성의 특징, 그리고 이 구성을 선택한 이유에 대해 알아봅니다."
last_modified_at: 2023-05-01T21:15:06
header:
  teaser: "assets/images/teaser.png"
categories:
  - settings
tags:
  - vite
  - emotion
  - typescript
  - twin
toc: true
---

# 블로그를 시작하기에 앞서

이 글의 목적은 '나'의 기억력을 믿지 못하기 때문이며 미래의 '나'에게 설명하는 마음으로 작성했습니다. 해당 내용이 부족하거나 잘못된 부분이 있으면 너그러운 마음으로 이해해주시기 바랍니다.

# 프로젝트 소개

이번 프로젝트는 개발하고 싶은 무언가가 생겼을 때, 개념을 실습하고 싶을 때, 빠르게 시작하고 빠르게 실패하기 위한 스케치 공간을 구성하는 것이다.
프로젝트 구성은 Twin + Vite + Emotion + Typescript를 선택했다.
선택한 이유는 실무에서 써보지 못한 라이브러리들을 경헙해보고 싶은 이유가 제일 컸다.
그 다음으로 헷갈리는 개념들을 직접 실습하며 이해하고자 하기 위함이다.
마지막으로 앞에서 말했던 것처럼 빠르게 시도하고 빠르게 실패하기 위한 모던 웹을 구축하고자 했다.

이 글은 사이드 프로젝트를 진행하면서 배운점과 과정을 기록으로 남기기 위해 작성한 글이다.
프로젝트 세팅 과정은 ben-rogerson/twin.examples[^1]를 참고했다.
이곳에 세팅 과정에 대한 설명이 상세하게 기술되어 있다.
Vite 패키지 뿐만 아니라 twin.macro롤 다양한 패키지, next.js 등에 세팅하는 예시 데모도 있다.

[^1]: <https://github.com/ben-rogerson/twin.examples/tree/master/vite-emotion-typescript>

# 프로젝트 구성 요소 소개

본 프로젝트에서 사용되는 주요 기술 요소들은 다음과 같다.

- Vite: 빠른 개발 환경과 최적화된 빌드를 제공하는 프론트엔드 빌드 도구. 개발 서버를 제공하며, Rollup 기반의 빌드로 최적화를 지원한다.
- TypeScript: 정적 타입 검사와 최신 JavaScript 기능을 제공하는 슈퍼셋 언어. 런타임 오류를 줄이고 유지 보수성을 높여준다.
- Emotion: 컴포넌트 기반의 CSS-in-JS 라이브러리로서, 스타일과 컴포넌트의 관심사를 분리하며 동적 스타일링에 유연하게 대처할 수 있다.
- Twin.macro: Emotion과 결합하여 Tailwind CSS를 사용할 수 있게 해주는 라이브러리. 이를 통해 반응형 디자인을 빠르게 구현하고 일관된 디자인 시스템을 제공할 수 있다.

# 프로젝트 구성 선택 이유

Twin, Vite, Emotion, Typescript를 활용해 프로젝트를 구성하게 된 이유는 다음과 같다.

1. 실무에서는 Unity Class를 활용한 스타일 작업 방식에 익숙했다. SCSS를 패키지에 구성하고 싶었으나 Unity Class를 작성하기 번거롭다고 판단했다. 이번 기회에 tailwind의 Unity Class 방식을 경험해보고 싶었다.

2. CSS-in-JS 방식은 스타일과 컴포넌트를 분리하며 동적 스타일링에 이점이 있다고 했는데 이를 경험해보고 싶었다.

3. 실무에서 Next.js 프레임워크를 사용하고 있었고 Next.js로 작업하기 전에 순수 리액트로만 사이드 프로젝트를 진행하여 순수 리액트를 더 잘 이해하고 싶었다. CRA 방식과 Vite 패키지를 고민하던 중에 Vite의 속도가 훨씬 빠르다는 이야기 들었다. 개발 환경에서, 빌드할 때의 속도가 얼마나 빠른지 체험해보고 싶었다.

4. Typescript는 사실 사이드 프로젝트를 진행하는 가장 큰 이유중 하나이다. 일단 TS로 실습을 진행해보고 싶었다.

# 프로젝트 설정하기

Twin, Vite, Emotion, Typescript를 함께 사용하는 프로젝트 설정하는 방법을 단계별로 설명한다.

## 1. Vite를 이용한 리액트 프로젝트 생성

```
npm create vite@latest your-vite-project -- --template react-ts
```

## 2. Emotion과 twin.macro 설치

```
npm install @emotion/react @emotion/styled
npm install --save-dev twin.macro @emotion/babel-plugin-jsx-pragmatic @babel/plugin-transform-react-jsx babel-plugin-macros tailwindcss
```

## 3. Global styles 추가

기타 다른 공통 CSS 적용을 위한 GlobalStyles.tsx 추가.

```
// src/styles/GlobalStyles.tsx
import React from 'react'
import { Global } from '@emotion/react'
import tw, { css, theme, GlobalStyles as BaseStyles } from 'twin.macro'

const customStyles = css({
  body: {
    WebkitTapHighlightColor: theme`colors.purple.500`,
    ...tw`antialiased`,
  },
})
const GlobalStyles = () => (
  <>
    <BaseStyles />
    <Global styles={customStyles} />
  </>
)
export default GlobalStyles
```

src/main.tsx 에 다음과 같이 적용한다.

```
// src/main.tsx
import React from 'react'
import { createRoot } from 'react-dom/client'
import GlobalStyles from './styles/GlobalStyles'
import App from './App'

const container = document.getElementById('root')
const root = createRoot(container!)
root.render(
  <React.StrictMode>
    <GlobalStyles />
    <App />
  </React.StrictMode>,
)
```

## 4. twin config 설정 (선택사항)

package.json에 다음 설정값 적용

```
// package.json
"babelMacros": {
  "twin": {
    "preset": "emotion"
  }
},
```

## 5. vite config 추가

```
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  optimizeDeps: {
    esbuildOptions: {
      target: 'es2020',
    },
  },
  esbuild: {
    // https://github.com/vitejs/vite/issues/8644#issuecomment-1159308803
    logOverride: { 'this-is-undefined-in-esm': 'silent' },
  },
  plugins: [
    react({
      babel: {
        plugins: [
          'babel-plugin-macros',
          [
            '@emotion/babel-plugin-jsx-pragmatic',
            {
              export: 'jsx',
              import: '__cssprop',
              module: '@emotion/react',
            },
          ],
          [
            '@babel/plugin-transform-react-jsx',
            { pragma: '__cssprop' },
            'twin.macro',
          ],
        ],
      },
    }),
  ],
})
```

## 6. TypeScript 설정

```
npm install -D @types/react
```

types/twin.d.ts 파일 추가

```
// types/twin.d.ts
import 'twin.macro'
import { css as cssImport } from '@emotion/react'
import styledImport from '@emotion/styled'
import { CSSInterpolation } from '@emotion/serialize'

declare module 'twin.macro' {
  // The styled and css imports
  const styled: typeof styledImport
  const css: typeof cssImport
}

declare module 'react' {
  // The tw and css prop
  interface DOMAttributes<T> {
    tw?: string
    css?: CSSInterpolation
  }
}

```

tsconfig.json에 다음 옵션값도 추가

```
{
  "compilerOptions": {
    "skipLibCheck": true,
    "jsxImportSource": "@emotion/react"
  },
  "include": ["src", "types"]
}
```

## 7. ESLint 설정 (선택 사항)

```

```

# 구성 요소별 특징 분석

# 결론
