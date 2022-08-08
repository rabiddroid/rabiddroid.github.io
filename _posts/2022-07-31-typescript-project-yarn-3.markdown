---
layout: post
title:  "Starting a typescript project with yarn 3"
date:   2022-07-31 20:45:39 -0700
categories: typescript yarn yarn3 nodejs
---
# Starting a typescript project with yarn 3


Are you interested in putting together your first typescript project with yarn as package manager.

Then here is a quick recipe to get you on your way.
>All sources for this example can be referenced here in *[github](https://github.com/rabiddroid/example-typescript-template)*.
>

### Install yarn into your project

The preferred way to install yarn is through *[Corepack](https://github.com/nodejs/corepack)* ( An experimental tool to help with managing versions of your package managers). This comes with node.js depending on the version you have. At the time of writing this blog, I had node v18.6.0. For node versions < 16.10 see *[Yarn Install](https://yarnpkg.com/getting-started/install).*

> *I highly recommend using node version > 16.0.0 for yarn 3 to work as expected.*
>

Create your project folder (… and enter in one line 🤓)

```bash
mkdir project-template-typescript && cd $_
```

Run to install and initialize yarn.

```bash
yarn init -2
```

> ***Note:** By default, `yarn init -2` will setup your project to be compatible with [Zero-Installs](https://yarnpkg.com/features/zero-installs), which requires checking-in your cache in your repository; check your [.gitignore](https://yarnpkg.com/getting-started/qa#which-files-should-be-gitignored) if you wish to disable this.*
>

To see what the basic yarn project state looks like, run

```bash
yarn install
```

> *One can also simply run “yarn”*
>

The folder contents look should like follows

<img src="{{ "/assets/img/2022-07-31-typescript-project-yarn-3-folder-contents.png" | prepend: site.baseurl | prepend: site.url}}" alt="Folder Contents" style="height: 200px; width:300px;"/>

*Some details of the files/folders seen*

- .editorconfig

    [https://editorconfig.org/](https://editorconfig.org/)

- .pnp.cjs

    In this install mode (the default starting from Yarn 2.0), Yarn generates a single `.pnp.cjs` file instead of the usual `node_modules` folder containing copies of various packages. The `.pnp.cjs` file contains various maps: one linking package names and versions to their location on the disk and another one linking package names and versions to their list of dependencies. With these lookup tables, Yarn can instantly tell Node where to find any package it needs to access, as long as they are part of the dependency tree, and as long as this file is loaded within your environment (more on that in the next section).. [*Details*](https://yarnpkg.com/features/pnp/#fixing-node_modules)

- .yarn

    artifacts folder for yarn

- .yarnrc.yml

    configure Yarn's internal settings.

- package.json

    describes the project details and settings for the package manager.

- yarn.lock

    Important info from the install process is stored in the `yarn.lock` lockfile so that it can be shared between every system installing the dependencies.


### Install typescript

Add typescript

```bash
yarn add --dev typescript
```

You can optionally enable [Yarn's TypeScript plugin](https://github.com/yarnpkg/berry/tree/master/packages/plugin-typescript), which helps manage `@types/*` dependencies automatically. If the project supports it, the type definitions are automatically installed when adding the dependency.

```bash
yarn plugin import typescript
```

Optionally add the VSCode Integration for typescript. Follow additional steps to have VSCode use the new  typescript version in workspace. [https://yarnpkg.com/getting-started/editor-sdks](https://yarnpkg.com/getting-started/editor-sdks)

```bash
yarn dlx @yarnpkg/sdks vscode
```

Initialize tsconfig.json with basic settings

```bash
yarn tsc --init
```

### Running your code

Let’s run our first code.

Create index.ts under <project_root>/src.

Add the following code

```tsx
function hello(){
    console.info('hello world!');
}

hello();
```

To keep this simple and clean without having to transpile to javascript, let’s run this with ts-node.

Add ts-node and dependency @types/node to the project by:

```bash
yarn add ts-node @types/node --dev
```

Now from the project root, run the command as follows:

```bash
yarn ts-node src/index.ts
```

You should see the following on the console.

```bash
$ yarn ts-node src/index.ts
hello world!
```

Congratulations on setting up your first typescript project. Wishing you happy times on your coding journey!
