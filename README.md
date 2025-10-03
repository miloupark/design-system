# Design System

- [🔖 Storybook for React & Vite](https://storybook.js.org/docs/get-started/frameworks/react-vite?renderer=react)
- [🎨 Tailwind CSS v4 + Vite](https://tailwindcss.com/docs/installation/using-vite)

<br>

### Install Storybook

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

### Install Tailwind CSS (v4)

```bash
$ npm install tailwindcss @tailwindcss/vite
```

#### 📁 index.css

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

- `@theme`
  - 토큰(디자인 값) 정의 → Tailwind 기본 유틸리티에 자동 반영됨
  - 즉, `--text-xs: 12px { line-height: 18px; }` 같은 형태라면 text-xs 클래스에 적용
- `@layer`
  - Tailwind 기본 유틸리티를 완전히 덮어쓰거나, 새로운 유틸리티 클래스를 직접 정의할 때.
  - `@theme`는 토큰 정의 → Tailwind 내부가 반영이고,
  - `@layer utilities`는 내가 직접 CSS 클래스를 작성하는 방식.

<br>

#### 📁 vite.config.ts

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

### Install Font

- [fontsource](https://fontsource.org/)
- [@fontsource/noto-sans-kr](https://www.npmjs.com/package/@fontsource/noto-sans-kr)

```bash
$ npm i @fontsource/noto-sans-kr
```

#### 📁 main.tsx

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

#### 📁 index.css

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
