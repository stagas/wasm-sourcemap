# @stagas/wasm-sourcemap

Fix AssemblyScript generated WebAssembly source map URLs.

AssemblyScript can emit a `sourceMappingURL` custom section in a `.wasm`
binary. This package reads, removes, or replaces that section directly from a
WASM byte buffer.

## Install

```sh
npm install @stagas/wasm-sourcemap
```

## Usage

```ts
import { readFile, writeFile } from 'node:fs/promises'
import { wasmSourceMap } from '@stagas/wasm-sourcemap'

const wasm = await readFile('build/module.wasm')

const url = wasmSourceMap.getSourceMapURL(wasm)
console.log(url)

const fixed = wasmSourceMap.setSourceMapURL(wasm, 'module.wasm.map')
await writeFile('build/module.wasm', fixed)
```

## API

### `wasmSourceMap.getSourceMapURL(buf)`

Returns the source map URL from the WASM `sourceMappingURL` custom section, or
`null` when the section does not exist.

```ts
const url = wasmSourceMap.getSourceMapURL(wasm)
```

### `wasmSourceMap.removeSourceMapURL(buf)`

Returns a new `Uint8Array` with the `sourceMappingURL` custom section removed.
If the section does not exist, the original bytes are returned as a `Uint8Array`.

```ts
const withoutSourceMap = wasmSourceMap.removeSourceMapURL(wasm)
```

### `wasmSourceMap.setSourceMapURL(buf, url)`

Returns a new `Uint8Array` with the `sourceMappingURL` custom section set to
`url`. Any existing `sourceMappingURL` section is removed first.

```ts
const withSourceMap = wasmSourceMap.setSourceMapURL(wasm, 'module.wasm.map')
```

## License

MIT
