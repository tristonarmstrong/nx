---
title: Overview of the Nx esbuild Plugin
description: The Nx Plugin for esbuild contains executors and generators that support building applications using esbuild. This page also explains how to configure esbuild on your Nx workspace.
---

# @nx/esbuild

The Nx Plugin for [esbuild](https://esbuild.github.io/api/), an extremely fast JavaScript bundler.

Why should you use this plugin?

- _Fast_ builds using esbuild.
- Type-checking using TypeScript, which esbuild does not handle.
- Intelligent `package.json` output.
- Additional [assets](/technologies/build-tools/esbuild/api/executors/esbuild#assets) for the output.

## Setting Up @nx/esbuild

### Installation

{% callout type="note" title="Keep Nx Package Versions In Sync" %}
Make sure to install the `@nx/esbuild` version that matches the version of `nx` in your repository. If the version numbers get out of sync, you can encounter some difficult to debug errors. You can [fix Nx version mismatches with this recipe](/recipes/tips-n-tricks/keep-nx-versions-in-sync).
{% /callout %}

In any Nx workspace, you can install `@nx/esbuild` by running the following command:

```shell {% skipRescope=true %}
nx add @nx/esbuild
```

This will install the correct version of `@nx/esbuild`.

## Using the @nx/esbuild Plugin

### Creating a new JS library

You can add a new library that builds using esbuild with:

```shell
nx g @nx/js:lib libs/mylib --bundler=esbuild
```

This command will install the esbuild plugin if needed, and set `@nx/esbuild:esbuild` executor for the `build` target.

### Adding esbuild target to existing libraries

If you already have a JS project that you want to use esbuild for, run this command:

```shell
nx g @nx/esbuild:configuration mylib
```

This generator validates there isn't an existing `build` target. If you want to overwrite the existing target you can pass the `--skipValidation` option.

```shell
nx g @nx/esbuild:configuration mylib --skipValidation
```

## Using esbuild

You can run builds with:

```shell
nx build mylib
```

Replace `mylib` with the name or your project. This command works for both applications and libraries.

### Copying assets

Assets are non-JS and non-TS files, such as images, CSS, etc. You can add them to the project configuration as follows.

```jsonc
"build": {
 "executor": "@nx/esbuild:esbuild",
  "options": {
    //...
    "assets": [
      { "input": "libs/mylib", "glob": "README.md", "output": "/" },
      { "input": "libs/mylib", "glob": "logo.png", "output": "/" },
      { "input": "libs/mylib", "glob": "docs/**/*.md", "output": "/docs" },
      //...
    ]
 }
}
```

Running `nx build mylib` outputs something like this.

```text
dist/libs/mylib/
├── README.md
├── docs
│   ├── CONTRIBUTING.md
│   └── TESTING.md
├── index.js
├── logo.png
└── package.json
```

### Generating a metafile

A metafile can be generated by passing the `--metafile` option. This file contains information about the build that can be analyzed by other tools, such as [bundle buddy](https://www.bundle-buddy.com/esbuild).

```shell
nx build mylib --metafile
```

This command will generate a `meta.json` file in the output directory.

```text
dist/libs/mylib/
├── README.md
├── index.js
├── meta.json
└── package.json
```

### Custom esbuild options

Extra API options for esbuild can be passed in the `esbuildOptions` object for your project configuration.

```jsonc
"build": {
  "executor": "@nx/esbuild:esbuild",
  "options": {
    //...
    "esbuildOptions": {
      "banner": { ".js": "// banner" },
      "footer": { ".js": "// footer" }
    }
  }
}
```

## More Documentation

- [Using JS](/technologies/typescript/introduction)
