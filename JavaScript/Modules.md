# JavaScript Modules

Modules divide code into files with explicit public interfaces. Modern JavaScript uses ECMAScript modules (ESM) with `export` and `import`.

## Named exports

```javascript
// user-utils.js
export function normalizeName(name) {
  return name.trim().toLowerCase();
}

export const DEFAULT_LIMIT = 20;
```

```javascript
// app.js
import { DEFAULT_LIMIT, normalizeName } from "./user-utils.js";
```

Named exports make the imported names explicit and work well when a module exposes several related values.

## Default exports

```javascript
// api-client.js
export default class ApiClient {
  // ...
}
```

```javascript
import ApiClient from "./api-client.js";
```

A module can have one default export. The importer chooses its local name, which can make large codebases less consistent. Prefer named exports unless a default export clearly represents the module's single main value.

## Renaming imports and exports

```javascript
import { normalizeName as normalizeUserName } from "./user-utils.js";

export { normalizeUserName as normalizeName };
```

## Importing a namespace

```javascript
import * as userUtils from "./user-utils.js";

console.log(userUtils.normalizeName(" ADA "));
```

## Side-effect imports

An import can run a module without importing a value:

```javascript
import "./configure-logging.js";
```

Use side-effect imports sparingly because they make dependencies less visible.

## Dynamic imports

`import()` returns a promise and loads a module when needed:

```javascript
async function loadReport() {
  const { buildReport } = await import("./report.js");
  return buildReport();
}
```

Dynamic imports can support conditional loading and code splitting.

## ESM and CommonJS

Node.js projects may use either module system.

| ESM | CommonJS |
| --- | --- |
| `import` / `export` | `require()` / `module.exports` |
| Often enabled by `"type": "module"` | Traditional Node.js default |
| Supports top-level `await` | Does not support top-level `await` in the same way |

Follow the existing repository's module system. Do not mix the two styles without understanding the project's build and runtime configuration.

## Module design

A useful module:

- Has one clear responsibility.
- Exports only what other modules need.
- Avoids hidden work at import time.
- Keeps network or file side effects at explicit boundaries.
- Does not create circular dependencies.

## Common mistakes

- Omitting a required file extension in an environment that needs it.
- Confusing a named import with a default import.
- Mixing ESM and CommonJS syntax without configuration.
- Exporting internal helpers unnecessarily.
- Creating circular imports between modules.
- Performing surprising network or state-changing work as soon as a module is imported.
