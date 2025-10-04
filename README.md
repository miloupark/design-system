# Design System

- [🔖 Storybook for React & Vite](https://storybook.js.org/docs/get-started/frameworks/react-vite?renderer=react)
- [🎨 Tailwind CSS v4 + Vite](https://tailwindcss.com/docs/installation/using-vite)

<br>

## 📖 Install Storybook

```bash
# 프로젝트에 스토리북 추가
$ npx storybook@latest init

# 스토리북 실행
$ npm run storybook

# v9에선 별도 설치가 필요 없음
$ npm install @storybook/builder-vite --save-dev
```

<!--
<br>

### 📁 .storybook > main.ts

```ts
const config: StorybookConfig = {
  // ...
  core: {
    builder: "@storybook/builder-vite", // 👈 Add this
  },
};
``` -->

<br>

## 🎨 Install Tailwind CSS (v4)

```bash
$ npm install tailwindcss @tailwindcss/vite
```

<br>

### index.css

v4에서는 tailwind.config.js 없이도 동작 (커스텀 토큰은 @theme로 CSS에 정의)

```css
/* index.css */
@import "tailwindcss";

/* (선택) 디자인 토큰 정의 - Tailwind v4 @theme */
@theme {
  --color-primary: #1d2745;
  /* ... */

  --text-xs: 12px {
    line-height: 18px;
    font-weight: 400;
  }
  /* ... */
}

@layer utilities {
  .text-xs {
    font-size: 12px;
    line-height: 18px;
    letter-spacing: 0px;
    font-weight: 400;
  }
}
```

`@theme`

- 토큰(디자인 값) 정의 → Tailwind 기본 유틸리티에 자동 반영됨
- 즉, `--text-xs: 12px { line-height: 18px; }` 같은 형태라면 text-xs 클래스에 적용

`@layer`

- Tailwind 기본 유틸리티를 완전히 덮어쓰거나, 새로운 유틸리티 클래스를 직접 정의할 때.
- `@theme`는 토큰 정의 → Tailwind 내부가 반영이고,
- `@layer utilities`는 내가 직접 CSS 클래스를 작성하는 방식.

<br>

### vite.config.ts

```ts
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwind from "@tailwindcss/vite"; // 👈 Add this

export default defineConfig({
  plugins: [react(), tailwind()], // 👈 Add this
});
```

<br>

## Install Font

- [fontsource](https://fontsource.org/)
- [@fontsource/noto-sans-kr](https://www.npmjs.com/package/@fontsource/noto-sans-kr)

```bash
$ npm i @fontsource/noto-sans-kr
```

<br>

### main.tsx

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App.tsx";
import "@fontsource/noto-sans-kr/400.css"; // 👈 Add this
import "@fontsource/noto-sans-kr/700.css"; // 👈 Add this
import "./index.css";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

<br>

### index.css

```css
/* index.css */
@import "tailwindcss";

@theme {
  --color-primary: #1d2745;

  --font-sans: "Noto Sans KR", ui-sans-serif, system-ui, sans-serif;
  /* 👈 Add this */

  --text-xs: 12px {
    line-height: 18px;
    font-weight: 400;
  }
}

body {
  font-family: var(--font-sans);
}
```

<br>

## 📘 Metadata

`meta`는 Storybook이 이 컴포넌트를 어떻게 표시하고 문서화할지를 알려주는 설정 정보다.  
즉, 컴포넌트의 메타데이터(설명서 역할)이다.

```tsx
const meta = {
  title: "Text/Label", // 스토리북 내에서 표시될 경로/이름
  component: Label, // 연결할 실제 리액트 컴포넌트
  parameters: {
    layout: "centered", // 미리보기 레이아웃
  },
  tags: ["autodocs"], // 자동 문서화 태그
  argTypes: {
    // props의 설명과 컨트롤 UI 정의
    htmlFor: { control: "text", description: "label의 for 속성" },
    children: { control: "text", description: "label의 내용" },
  },
} satisfies Meta<typeof Label>;

export default meta;

type Story = StoryObj<typeof meta>;

// Default → 실제 스토리(=컴포넌트 예시)
export const Default: Story = {
  // args → 컴포넌트에 전달할 props 값
  // 즉, Label 컴포넌트를 아래와 같이 렌더링하겠다라는 뜻
  args: {
    htmlFor: "username",
    children: "이메일",
  },
};
```

| 키         | 설명                                                                 |
| :--------- | :------------------------------------------------------------------- |
| title      | Storybook 사이드바에서 컴포넌트가 보이는 이름 (폴더 구조처럼 표시됨) |
| component  | 실제 렌더링할 React 컴포넌트                                         |
| parameters | 추가 설정 (레이아웃, 배경색, 액션 핸들러 등)                         |
| tags       | 자동 문서화(autodocs) 등 Storybook 기능용 태그                       |
| argTypes   | props의 타입, 설명, 컨트롤(슬라이더, 입력창 등)을 지정               |

<br>

## 🧭 Storybook의 구조와 동작 흐름

### 1. 리액트 컴포넌트

```tsx
interface ILabelProps {
  htmlFor: string;
  children: string;
}

export default function Label({ htmlFor, children }: ILabelProps) {
  return (
    <label className="text-sm text-primary" htmlFor={htmlFor}>
      {children}
    </label>
  );
}
```

<br>

### 2. 메타데이터

이 컴포넌트가 Storybook에서 어떻게 보여야 하는지를 설명하는 설정 정보

```tsx
const meta = {
  title: "Text/Label", // 사이드바 경로/이름
  component: Label, // 어떤 컴포넌트를 문서화할지
  parameters: {
    layout: "centered", // 미리보기 레이아웃
  },
  tags: ["autodocs"], // 자동 문서 생성
  argTypes: {
    // props 제어 UI 설정
    htmlFor: { control: "text", description: "label의 for 속성" },
    children: { control: "text", description: "label의 내용" },
  },
} satisfies Meta<typeof Label>;

export default meta;
```

<br>

### 3. 스토리

`meta`에 연결된 컴포넌트의 실제 예시. 즉, `이 props로 Label을 보여줘`라는 구체적인 사례다.

```tsx
type Story = StoryObj<typeof meta>;

export const Default: Story = {
  args: {
    htmlFor: "username",
    children: "이메일",
  },
};
```

<br>

### 4. Storybook의 내부 흐름

- ① meta가 Storybook에 컴포넌트 정보를 등록한다.
- ② Storybook은 meta를 읽어 스토리 목록을 생성한다.
- ③ Default 스토리의 args를 Label 컴포넌트에 전달한다.
- ④ 그 결과가 Preview/Docs 패널에서 렌더링된다.

<br>

> 💡 meta는 컴포넌트의 설정 정보, Story는 그 설정을 기반으로 한 구체적인 예시,  
> StoryBook은 이를 조합해 `컴포넌트 문서화 + 시각적 테스트`를 제공한다.
