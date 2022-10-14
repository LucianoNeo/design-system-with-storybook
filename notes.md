# Instalar story book

npx sb init --builder @storybook/builder-vite --use-npm


## Ativar dark mode

Criar arquivo manager.js com o conteudo a seguir:
```js
import { addons } from '@storybook/addons'
import { themes } from '@storybook/theming'

addons.setConfig({
    theme: themes.dark
})
```

Substituir conteudo do arquivo preview.cjs

```js
import { themes } from '@storybook/theming'

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
  docs: {
    theme: themes.dark,
  },
}

```

- Deletar pasta stories


# Instalar CLSX

npm install --save clsx

# Usar SLOT para trocar tags de components

npm install @radix-ui/react-slot


# Para deploy do storybook
npm install @storybook/storybook-deployer
npm install @storybook/storybook-a11y

# Criar arquivo para CI/CD no github pages .github/workflows/deploy-docs.yml
```js
name: Deploy Storybook

on:
  push:
    branches:
      - master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 16

      - name: Install dependencies
        run: npm ci --legacy-peer-deps

      - name: Build Storybook
        run: npm run build-storybook

      - name: Deploy Storybook
        run: npm run deploy-storybook -- --ci --existing-output-dir=storybook-static
        env:
          GH_TOKEN: ${{ github.actor }}:${{ secrets.GITHUB_TOKEN }}
          
```

## Configurar arquivo main.cjs
```js
module.exports = {
  "stories": [
    "../src/**/*.stories.mdx",
    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
  "addons": [
    "@storybook/addon-links",
    "@storybook/addon-essentials",
    "@storybook/addon-interactions",
    "@storybook/addon-a11y"
  ],
  "framework": "@storybook/react",
  "core": {
    "builder": "@storybook/builder-vite"
  },
  "features": {
    "storyStoreV7": true
  },
  viteFinal: (config, { configType }) => {
    if (configType == "PRODUCTION") {
      config.base = '/design-system-with-storybook/'
    }
    return config
  }
}
```

# Para testes no storybook
npm i @storybook/addon-interactions @storybook/jest @storybook/testing-library @storybook/test-runner -D

# Adicionar ao main.cjs
```
"features": {
    "storyStoreV7": true,
    "interactionsDebugger": true,
  },
```

# Adicionar script
 "test-storybook": "test-storybook"


# Para fazer mock de API
npm i msw msw-storybook-addon -D


# Para rodar o msw
npx msw init public/

# Adicionar ao main.cjs
```
"staticDirs":[
  "../public"
]
```

# Adicionar ao preview.cjs
```js
import { initialize, mswDecorator } from 'msw-storybook-addon';

// Initialize MSW
initialize();

// Provide the MSW addon decorator globally
export const decorators = [mswDecorator];
```