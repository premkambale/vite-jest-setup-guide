
# 🚀  Vite + Jest Setup Boilerplate (React, JS)
A complete setup guide for adding Jest, Testing Library, and Babel for testing React apps. All config files should be created in the **root directory**, outside of `src/`.

---

## 📦 Step 1: Install Testing Libraries

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

---

## 📦 Step 2: Install Babel + Jest Transformers

```bash
npm install --save-dev babel-jest @babel/preset-env @babel/preset-react
```

Also install the JS DOM environment:

```bash
npm install --save-dev jest-environment-jsdom
```

---

## 🛠️ Step 3: Create `babel.config.cjs` (in root)

```js
// babel.config.cjs
module.exports = {
  presets: [
    ['@babel/preset-env', { targets: { node: 'current' } }],
    ['@babel/preset-react', { runtime: 'automatic' }],
  ],
};
```

> ⚠️ If you get ESLint config issues, add this to `eslint.config.js`:

```js
{
  files: ['**/babel.config.js', '**/jest.config.js', '**/vite.config.js'],
  languageOptions: {
    ecmaVersion: 2020,
    sourceType: 'script',
    globals: globals.node,
  },
}
```

---

## ⚙️ Step 4: Create `jest.config.cjs` (in root)

```js
// jest.config.cjs
module.exports = {
  testEnvironment: 'jsdom',
  transform: {
    '^.+\.(js|jsx)$': 'babel-jest',
  },
  coverageDirectory: 'coverage',
  collectCoverageFrom: ['src/**/*.{js,jsx}'],
  moduleFileExtensions: ['js', 'jsx'],
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
    '\.(css|scss|sass)$': 'identity-obj-proxy',
  },
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  testPathIgnorePatterns: ['/node_modules/', '/dist/'],
};
```

---

## 🧩 Step 5: Create `jest.setup.js` (in root)

```js
// jest.setup.js
import '@testing-library/jest-dom';
```

---

## 🧪 Step 6: Create First Test Case

Create a test file: `src/__tests__/App.test.jsx`

```jsx
// src/__tests__/App.test.jsx
import { render, screen } from '@testing-library/react';
import App from '../App';

test('renders main app heading', () => {
  render(<App />);
  expect(screen.getByText(/hello/i)).toBeInTheDocument();
});
```

> ⚠️ If ESLint complains about `'test' is not defined`, add this block to `eslint.config.js`:

```js
// ✅ Jest environment for test files
{
  files: ['**/*.test.{js,jsx}', '**/__tests__/**/*.{js,jsx}'],
  languageOptions: {
    ecmaVersion: 2020,
    globals: {
      ...globals.browser,
      ...globals.jest,
    },
    parserOptions: {
      ecmaVersion: 'latest',
      ecmaFeatures: { jsx: true },
      sourceType: 'module',
    },
  },
}
```

---

## 🚀 Step 7: Add Scripts in `package.json`

```json
"scripts": {
  "test": "jest",
  "test:watch": "jest --watch",
  "test:coverage": "jest --coverage"
}
```

---

## ✅ Final Checklist

Make sure these files are in the **root (NOT inside `src/`)**:

- `babel.config.cjs`
- `jest.config.cjs`
- `jest.setup.js`

Now you're ready to:

- Run tests: `npm run test`
- Watch mode: `npm run test:watch`
- Code coverage: `npm run test:coverage`

Happy Testing! 🧪✨
