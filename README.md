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

<br>

### 📁 .storybook > main.ts

```ts
const config: StorybookConfig = {
  // ...
  core: {
    builder: "@storybook/builder-vite", // 👈 Add this
  },
};
```

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
    font-weight: 700;
  }
  /* ... */
```

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

### Install Font (선택)

- [fontsource](https://fontsource.org/)
- [@fontsource/noto-sans-kr](https://www.npmjs.com/package/@fontsource/noto-sans-kr)

```bash
$ npm i @fontsource/noto-sans-kr
```
