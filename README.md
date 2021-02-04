# typescript-subpath-exports-workaround

Workaround for Node.js subpath exports in TypeScript

[![npm version][npm-image]][npm-url]
![license][license]


## Why?

Node.js v12+ extends [package entry points features](https://nodejs.org/api/packages.html#packages_package_entry_points) like [subpath exports](https://nodejs.org/api/packages.html#packages_subpath_exports) or [conditional exports](https://nodejs.org/api/packages.html#packages_conditional_exports) but TypeScript doesn't support them yet.

> [Support for NodeJS 12.7+ package exports · Issue #33079 · microsoft/TypeScript · GitHub](https://github.com/microsoft/TypeScript/issues/33079)

## How?

Use [`typesVersions`](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html#version-selection-with-typesversions) to workaround.

#### package.json

```json
{
  "main": "dist/index.js",
  "types": "dist-types/index.d.ts",
  "exports": {
    ".": "./dist/index.js",
    "./exported": "./dist/exported.js"
  },
  "typesVersions": {
    "*": {
      "exported": ["dist-types/exported"]
    }
  }
}
```

Users can import only the package root and the exported subpath `/exported`.

```ts
// Pass
import "typescript-subpath-exports-workaround"
import "typescript-subpath-exports-workaround/exported"

// Error
import "typescript-subpath-exports-workaround/not-exported"
import "typescript-subpath-exports-workaround/dist/exported"
import "typescript-subpath-exports-workaround/dist/not-exported"
```

## Demo

You can install and try this package.

```console
$ npm i typescript-subpath-exports-workaround
```

## License

MIT

[npm-image]: https://badgen.net/npm/v/typescript-subpath-exports-workaround?icon=npm&label=
[npm-url]: https://npmjs.org/package/typescript-subpath-exports-workaround
[npm-downloads-image]: https://badgen.net/npm/dm/typescript-subpath-exports-workaround
[ts-version]: https://badgen.net/badge/typescript/%3E=4.1?icon=typescript
[license]: https://badgen.net/npm/license/typescript-subpath-exports-workaround
