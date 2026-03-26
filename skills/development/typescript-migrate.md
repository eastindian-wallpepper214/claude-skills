---
name: typescript-migrate
description: Incremental JavaScript to TypeScript migration with proper types, interfaces, and config
category: development
tags: [typescript, migration, types, interfaces, tsconfig]
author: claude-skills
version: 1.0.0
---

# JavaScript to TypeScript Migration

Migrate a JavaScript codebase to TypeScript incrementally. Add types file-by-file without breaking the existing build.

## Execution Steps

### 1. Assess the Codebase

Inventory the project:
- Count `.js` and `.jsx` files to migrate
- Identify the module system (CommonJS `require` vs ESM `import`)
- Check for existing JSDoc type annotations (these can be preserved or converted)
- Identify the build tool (webpack, vite, babel, esbuild)
- Check for existing `tsconfig.json` or TypeScript dependencies

### 2. Install TypeScript

```bash
npm install --save-dev typescript @types/node
# Install framework-specific types
npm install --save-dev @types/react @types/express  # as needed
```

### 3. Configure tsconfig.json

Create or update `tsconfig.json` with permissive settings for migration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": false,
    "allowJs": true,
    "checkJs": false,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "noEmit": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.*"]
}
```

Key: `allowJs: true` and `strict: false` let JS and TS coexist during migration.

### 4. Plan Migration Order

Migrate files in dependency order (leaves first, entry points last):
1. **Utility/helper files** — no imports from other project files
2. **Types and constants** — shared type definitions
3. **Data layer** — models, repositories, database access
4. **Business logic** — services, use cases
5. **API/UI layer** — controllers, components, routes
6. **Entry points** — index.ts, app.ts, main.ts

### 5. Create Shared Type Definitions

Before migrating individual files, create a types directory:

```
src/
  types/
    index.ts        # Re-exports all types
    models.ts       # Domain entity interfaces
    api.ts          # Request/response types
    common.ts       # Shared utility types
```

Define interfaces for core domain objects:

```typescript
// src/types/models.ts
export interface User {
  id: string;
  email: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### 6. Migrate One File at a Time

For each file:

1. Rename `.js` to `.ts` (or `.jsx` to `.tsx`)
2. Fix import paths (add `.js` extension if using ESM resolution, or remove if bundler handles it)
3. Add type annotations to function parameters and return types
4. Replace `module.exports` with `export` if switching to ESM
5. Handle common patterns:

```typescript
// Before (JS)
function getUser(id) {
  return db.query('SELECT * FROM users WHERE id = ?', [id]);
}

// After (TS)
async function getUser(id: string): Promise<User | null> {
  return db.query<User>('SELECT * FROM users WHERE id = ?', [id]);
}
```

6. Run `npx tsc --noEmit` after each file to verify no type errors

### 7. Handle `any` Types

During migration, some `any` types are unavoidable. Track them:
- Add `// TODO: type this properly` comments next to intentional `any`
- Use `unknown` instead of `any` where possible (forces type narrowing)
- Create a migration tracker:

```typescript
// src/types/migration.ts
// Files remaining: 23/45
// TODO-any count: 12
// Target: strict mode by [date]
```

### 8. Tighten Strictness Incrementally

After all files are `.ts`, enable strict checks one at a time in tsconfig:

```json
{
  "compilerOptions": {
    "noImplicitAny": true,          // Step 1
    "strictNullChecks": true,       // Step 2
    "strictFunctionTypes": true,    // Step 3
    "strictBindCallApply": true,    // Step 4
    "strict": true                  // Final: enables all strict checks
  }
}
```

Fix errors after each flag. This is the most labor-intensive step.

### 9. Update Build and Lint Configuration

- Update build tool config to handle `.ts` files
- Add `@typescript-eslint/parser` and `@typescript-eslint/eslint-plugin` to ESLint
- Update CI pipeline to run `tsc --noEmit` as a type-check step
- Update test runner config to handle TypeScript (ts-jest, vitest, etc.)

### 10. Verify and Clean Up

- Run `npx tsc --noEmit` with zero errors
- Run the full test suite
- Search for remaining `any` types: `grep -rn ': any' src/`
- Remove `allowJs: true` from tsconfig once all files are migrated
- Delete any leftover `.js` source files
