# Astro Starter Kit: Basics

```sh
npm create astro@latest -- --template basics
```

> 🧑‍🚀 **Seasoned astronaut?** Delete this file. Have fun!

## 🚀 Project Structure

Inside of your Astro project, you'll see the following folders and files:

```text
/
├── public/
│   └── favicon.svg
├── src
│   ├── assets
│   │   └── astro.svg
│   ├── components
│   │   └── Welcome.astro
│   ├── layouts
│   │   └── Layout.astro
│   └── pages
│       └── index.astro
└── package.json
```

To learn more about the folder structure of an Astro project, refer to [our guide on project structure](https://docs.astro.build/en/basics/project-structure/).

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 👀 Want to learn more?

Feel free to check [our documentation](https://docs.astro.build) or jump into our [Discord server](https://astro.build/chat).

## Порядок создания нового проекта

1. Создаем в терминале при помощи команды npm create astro@latest новый проект
2. Открой astro.config.mjs и убедись, что там указан статический режим:

```js
import { defineConfig } from "astro/config";

export default defineConfig({
    output: "static", // Важно: должно быть 'static', а не 'server'
    site: "https://github.com/web22des/astro-cource.git", // Замени на свой новый проект
});
```

2. Копируем или создаем в корне проекта .github/workflows/deploy.yml файл с конфигурацией

```yaml
name: Deploy to GitHub Pages

on:
    push:
        branches: [main]
    pull_request:
        branches: [main]

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout code
              uses: actions/checkout@v4

            - name: Setup Node.js
              uses: actions/setup-node@v4
              with:
                  node-version: "20"
                  cache: "npm"

            - name: Install dependencies
              run: npm ci

            - name: Build project
              run: npm run build

            - name: Setup Pages
              uses: actions/configure-pages@v4

            - name: Upload artifact
              uses: actions/upload-pages-artifact@v3
              with:
                  path: ./dist

    deploy:
        if: github.ref == 'refs/heads/main'
        needs: build
        permissions:
            pages: write
            id-token: write
        environment:
            name: github-pages
            url: ${{ steps.deployment.outputs.page_url }}
        runs-on: ubuntu-latest
        steps:
            - name: Deploy to GitHub Pages
              id: deployment
              uses: actions/deploy-pages@v4
```

4. Создаем новый репозиторий через веб интерфейс
5. Копируем все строки для синхронизации в терминал vs Code и запускаем
6. Финальные настройки на GitHub
    1. Зайди в Settings → Pages твоего репозитория
    2. В разделе "Build and deployment" выбери: Source: GitHub Actions
    3. Сохрани настройки
7. Сoздай новый коммит непосредственно из терминала Vs Code
8. Во вкладке Actions / All Workflows деплой должен пройти успешно
