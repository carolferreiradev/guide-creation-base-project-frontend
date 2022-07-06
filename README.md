</br>
<p align="center">
<h1 align="center">Guide creation base project frontend</h1>
<img alt="main" src="https://github.com/carolferreiradev/guide-creation-base-project-frontend/blob/main/guide.svg" width="100%">
</br>
</p>
Guide for creating and configuring a frontend base project containing: NextJs with Typescript, ESLint, Prettier, Husky, Jest, Testing Library, Storybook, Styled Components, configure absolute path in Typescript, Storybook and Jest.
</br>

## Getting Started

</br>

- NAVIGATION
  - [GETTING STARTED](#getting-started)
  - [PROJECT WITH NEXTJS AND TYPESCRIPT](#project-with-nextjs-and-typescript)
  - [STYLED COMPONENTS](#styled-components)
  - [ESLINT](#eslint)
  - [PRETTIER](#prettier)
  - [JEST WITH REACT TESTING LIBRARY](#jest-with-react-testing-library)
  - [STORYBOOK](#storybook)
  - [HUSKY](#husky)
  - [CONFIGURE ABSOLUTE PATHS](#configure-absolute-paths)
  - [DOCUMENTATION AND ARTICLES USED](#documentation-and-articles-used)
    </br>
    </br>
    </br>
    </br>
    </br>

## PROJECT WITH NEXTJS AND TYPESCRIPT

</br>

```bash
npx create-next-app@latest --typescript
# or
yarn create next-app --typescript
# or
pnpm create next-app --typescript
```

Clean up the project by deleting the following files and folders:

- pages/api
- public/ All icons
- styles/ All files

By default, I usually create a src folder and move the pages folder to it, and later create the other files my project needs in that same folder. This configuration is not mandatory.

Warning: the "pages" folder can only be inside "src" or in the project root.

</br>

In `package.json` add this script:

```
"types": "tsc --pretty --noEmit",
```

Legend:

--pretty -> Enable color and formatting in TypeScript's output to make compiler errors easier to read.

--noEmit -> Disable emitting files from a compilation.
</br>
</br>

### Create \_document.tsx

</br>

This file is used when we need to inject information inside the html and body tag. Inserting fonts is usually done in this file.
Inside pages folder, create a file named \_document.tsx, he must look like:

```
import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
return (

<Html>
  <Head>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap" rel="stylesheet"/>
  </Head>
  <body>
    <Main />
    <NextScript />
  </body>
</Html>
);
}
```

To customize the tab title per page, here's an example:

```
import Head from 'next/head'

<Head>
  <title>Home | nameProject</title>
</Head>
```

</br>
</br>
</br>

## STYLED COMPONENTS

</br>
For the styles we will use Styled Components.

```
npm install --save styled-components
npm install @types/styled-components
# or
yarn add styled-components
yarn add @types/styled-components
```

</br>

Usage

```
const Button = styled.button`
  background: transparent;
  color: white;
  border: 2px solid white;
`

render(<Button>GitHub</Button>)

```

Global styled file suggestion.

```
import { createGlobalStyle } from "styled-components";

export const GlobalStyle = createGlobalStyle`

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;

  @media (max-width: 1080px) {
    font-size: 93.75%;
  }
  @media (max-width: 720px) {
    font-size: 87.5%;
  }
}

body{
  font-family: "Jost", sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
}

h1,
h2,
h3,
h4,
h5,
h6,
strong {
  font-weight: 700;
}

button {
  cursor: pointer;
}

[disabled] {
  opacity: 0.8;
  cursor: not-allowed;
}
`;

```

### Theme

</br>
To use themes, we need the theme declaration file and the files containing the theme variables and values. Here's an example:

</br>

(File content theme dark)
`styles/theme/dark.ts`

```
export default {
  title: "dark",

  colors: {
    backgroundPrimary: "#000000",
    info: "#000000",
    danger: "#000000",
    success: "#000000",
    warning: "#000000",
  },
};

```

</br>

(File declaration)
`styles/theme/styled.d.ts`

```
import "styled-components";

declare module "styled-components" {
  export interface DefaultTheme {
    title: string;

    colors: {
      backgroundPrimary: string;
      info: string;
      danger: string;
      success: string;
      warning: string;
    };
  }
}

```

</br>

To use:

```
import type { AppProps } from "next/app";
import { ThemeProvider } from "styled-components";
import { GlobalStyle } from "@/styles/global";
import dark from "@/styles/themes/dark";

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ThemeProvider theme={dark}>
      <Component {...pageProps} />
      <GlobalStyle />
    </ThemeProvider>
  );
}

export default MyApp;

```

and in their styles files:

```
body{
  background: ${(props) => props.theme.colors.backgroundPrimary};
}
```

To insert more than one theme, it is possible to create a context that encompasses a application, and a switch that will make a change according to the click.

</br>
</br>
</br>

## ESLINT

</br>

For code analysis, in order to detect problems that deviate from the defined code standard.
Rules list [click here](https://eslint.org/docs/latest/rules/)

Since version 11.0.0, Next.js provides an integrated ESLint. If the version you want to install is earlier than this one, it can be followed according to the following settings.

```
npm install eslint --save-dev

# or

yarn add eslint --dev
```

After install, execute `yarn create @eslint/config` or `npm init @eslint/config` for configuration. You can choose your own settings or follow the ones below:

```
? How would you like to use ESLint?
> To check syntax, find problems, and enforce code style

? What type of modules does your project use?
> JavaScript modules (import/export)

? Which framework does your project use?
> React

? Does your project use TypeScript?
> Yes

? Where does your code run?
> Browser

? How would you like to define a style for your project?
> Use a popular style guide

? Which style guide do you want to follow?
> Google: https://github.com/google/eslint-config-google

? What format do you want your config file to be in?
> JavaScript

The config that you've selected requires the following dependencies:

eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest eslint-config-google@latest eslint@>=5.16.0 @typescript-eslint/parser@latest

? Would you like to install them now with npm?
> Yes

```

A file `.eslintrc.js` will be created, you can add rules, extensions, etc. Here's an example

```
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    "plugin:react/recommended",
    "google",
    "prettier",
    "next/core-web-vitals",
    "plugin:storybook/recommended",
  ],
  parser: "@typescript-eslint/parser",
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: "latest",
    sourceType: "module",
  },
  plugins: ["react", "@typescript-eslint"],
  ignorePatterns: ["/**/*.d.ts", "next.config.js"],
  rules: {
    "react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off",
    "require-jsdoc": "off",
    semi: ["error", "always"],
  },
};

```

If you are on a version lower than 11.0.0 of next or if you are using some other frame, in `package.json` add this script:

```
"lint": "eslint . --ext ts --ext tsx --ext js"
```

Legend:

--ext -> Specify JavaScript file extensions
</br>
</br>
</br>

## PRETTIER

</br>

Prettier formats the code according to predefined rules.

(If prettier is not installed.)

```
npm install --save-dev --save-exact prettier
# or
yarn add --dev --exact prettier
```

Add prettier config.

```
npm install --save-dev eslint-config-prettier
# or
yarn add --dev eslint-config-prettier
```

Add prettier on .eslintrc

```
{
  "extends": ["prettier"]
}
```

In `package.json` add this script:

```
"prettier": "prettier --check ./src",
```

Legend:

--check -> When you want to check if your files are formatted. This will output a human-friendly message and a list of unformatted files, if any.

> Note:
> ESLint identifies code patterns that are not following pre-established rules. Prettier formats the code according to these rules.

</br>
</br>
</br>

## JEST WITH REACT TESTING LIBRARY

</br>

For testing we will use jest and testing-library.

```
npm install --save-dev jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom

# or

yarn add jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom -D
```

In `jest.config.js`

```
const nextJest = require("next/jest");

const createJestConfig = nextJest({
  dir: "./",
});

const customJestConfig = {
  testPathIgnorePatterns: ["<rootDir>/.next/", "<rootDir>/node_modules/"],
  moduleDirectories: ["node_modules", "<rootDir>/src/"],
  testEnvironment: "jest-environment-jsdom",
};

module.exports = createJestConfig(customJestConfig);

```

Example of test:

`login.spec.tsx`

```
import { render, screen } from "@testing-library/react";
import "@testing-library/jest-dom";

import Login from "@/pages/login";

import { ThemeProvider } from "@/context/theme";

describe("Pages => Login", () => {
  it("Render a component", () => {
    render(
      <ThemeProvider>
        <Login />
      </ThemeProvider>
    );

    const heading = screen.getByText(/Lorem ipsum/i);

    expect(heading).toBeInTheDocument();
  });
});

```

### Test Coverage

</br>

In `jest.config.js`

```
  collectCoverage: true,
  collectCoverageFrom: [
    "./src/**",
    "!./src/**/*.d.ts",
    "!./src/**/*.stories.tsx",
    "!./src/**/global.ts",
    "!./src/**/_document.tsx",
  ],
  coverageThreshold: {
    global: {
      branches: 90,
      functions: 90,
      lines: 90,
      statements: 90,
    },
  },
```

For run `yarn coverage` or `npm run coverage`. You can add the --watch flag to listen for file changes and run the test related to those changes, or --watchAll to run all tests when there are changes.

</br>

In `package.json` add this script:

```
"test": "jest",
"coverage": "jest --coverage --watch",
```

</br>
</br>
</br>

## STORYBOOK

</br>

For documentation we will use the Storybook.

```
npx storybook init

```

For run

```
yarn storybook
# or
npm run storybook
```

Define paths in `.storybook/main.js`

```
module.exports = {
  stories: [
    "../src/stories/**/*.stories.mdx",
    "../src/**/*.stories.mdx",
    "../src/stories/**/*.stories.@(js|jsx|ts|tsx)",
    "../src/**/*.stories.@(js|jsx|ts|tsx)",
  ],
}
```

### BONUS

Case you project have theme, define a ThemeProvider in `.storybook/preview.jsx`

```
import { ThemeProvider } from "@/context/theme";
import { GlobalStyle } from "@/styles/global";

export const decorators = [
  (Story) => (
    <ThemeProvider>
      <Story />
      <GlobalStyle />
    </ThemeProvider>
  ),
];
```

</br>
Theme select 
</br>
To configure a select for all components that changes the theme according to its value:

In `.storybook/preview.jsx`

```
export const argTypes = {
  theme: {
    control: "select",
    options: ["Light", "Dark"],
    description: "Choose theme to check component style changes.",
  },
};
export const args = { theme: "Dark" };
```

In Storybook components

```
export const Intro = ({ theme }: Props) => {
  const { setThemeStorybook } = useContext(ThemeStoryBookContext);

  useEffect(() => {
    setThemeStorybook(theme);
  }, [theme, setThemeStorybook]);

  return <IntroductionComponent />;
};
```

</br>
</br>
</br>

## HUSKY

</br>

Husky runs commands that perform pre-commit checks.
</br>

> Husky improves your commits and more 🐶 woof!
> You can use it to lint your commit messages, run tests, lint code, etc... when you commit or push. (By Husky docs)
> </br>

```
npx husky-init && npm install
# or
npx husky-init && yarn
```

</br>

</br>
In `.husky/pre-commit` we run our commands before the commit is completed, making the necessary checks.

```
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo 'Checking before committing...'

# Check Prettier
npm run prettier ||
(
        echo '❌ Prettier Check Failed ❌';
        false;
)

# Check ESLint
npm run lint ||
(
        echo '❌ ESLint Check Failed ❌'
        false;
)

# Check tsconfig
npm run types ||
(
        echo '❌ Typescript Check Failed ❌ '
        false;
)

# Check test
npm run test ||
(
        echo '❌ Test Check Failed ❌ '
        false;
)


echo '✔️ Success... Its committing now. ✔️'


```

</br>
</br>
</br>

## CONFIGURE ABSOLUTE PATHS

</br>

In `tsconfig.json`

```
    "baseUrl": ".",
    "paths": {
      "@/context/*": ["src/context/*"],
      "@/hooks/*": ["src/hooks/*"],
      "@/pages/*": ["src/pages/*"],
      "@/services/*": ["src/services/*"],
      "@/styles/*": ["src/styles/*"],
      "@/utils/*": ["src/utils/*"],
      "@/stories/*": ["src/stories/*"],
      "@/assets/*": ["src/assets/*"]
    }
```

In `jest.config.js`

```
moduleNameMapper: {
    "^@/pages/(.*)": "<rootDir>/src/pages/$1",
    "^@/context/(.*)": "<rootDir>/src/context/$1",
    "^@/hooks/(.*)": "<rootDir>/src/hooks/$1",
    "^@/services/(.*)": "<rootDir>/src/services/$1",
    "^@/styles/(.*)": "<rootDir>/src/styles/$1",
    "^@/utils/(.*)": "<rootDir>/src/utils/$1",
    "^@/stories/(.*)": "<rootDir>/src/stories/$1",
    "^@/assets/(.*)": "<rootDir>/src/assets/$1",
  },
```

In `.storybook/main.js`

```
const path = require("path");


webpackFinal: async (config) => {
    config.resolve.alias = {
      ...config.resolve.alias,
      "@/pages": path.resolve(__dirname, "../src/pages"),
      "@/context": path.resolve(__dirname, "../src/context"),
      "@/hooks": path.resolve(__dirname, "../src/hooks"),
      "@/services": path.resolve(__dirname, "../src/services"),
      "@/styles": path.resolve(__dirname, "../src/styles"),
      "@/utils": path.resolve(__dirname, "../src/utils"),
      "@/stories": path.resolve(__dirname, "../src/stories"),
      "@/assets": path.resolve(__dirname, "../src/assets"),
    };
    return config;
  },
```

</br>
Imports look like this:

```
import { Introduction } from "@/stories/Introduction";
```

</br>
</br>
</br>

## DOCUMENTATION AND ARTICLES USED

</br>

- [NextJs](https://nextjs.org/)
- [Husky](https://typicode.github.io/husky/#/)
- [Prettier](https://prettier.io/)
- [Jest](https://jestjs.io/pt-BR/)
- [Testing Library](https://testing-library.com/)
- [Eslint](https://eslint.org/)
- [Styled Components](https://styled-components.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [Storybook ](https://storybook.js.org/)
- [React Architecture Patterns for Your Projects - Aman Mittal](https://blog.openreplay.com/react-architecture-patterns-for-your-projects)
- [Storybook - Mario Souto(Dev Soutinho)](https://www.youtube.com/watch?v=R41_Qedrzik&ab_channel=MarioSouto-DevSoutinho)
- [ESLint Prettier Husky - Jarrod Watts](https://blog.jarrodwatts.com/nextjs-eslint-prettier-husky)
