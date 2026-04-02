---
description: Run TypeScript type checking and fix type errors
agent: build
temperature: 0.2
---

Run TypeScript type checker and analyze type errors. Provide fixes for type issues.

**Type Check Execution:**
!`npx tsc --noEmit 2>&1 || npx tsc 2>&1 || echo "No TypeScript config found"`

!`cat tsconfig.json 2>/dev/null || echo "No tsconfig.json found"`

!`find . -name "*.ts" -o -name "*.tsx" ! -path "./node_modules/*" ! -path "./dist/*" ! -path "./build/*" | wc -l`

## Type Checking Results

### Summary
- **Total Files**: [count]
- **Total Errors**: [count]
- **Total Warnings**: [count]
- **Status**: [PASS/FAIL/WITH WARNINGS]

### Error Breakdown
| Error Code | Count | Description |
|------------|-------|-------------|
| TS2322 | [count] | Type mismatch |
| TS2345 | [count] | Argument type mismatch |
| TS2538 | [count] | Null/undefined check |
| TS7006 | [count] | Implicit any |
| [code] | [count] | [description] |

---

## Critical Type Errors (Must Fix)

### 1. Type Mismatch Errors (TS2322)

**Files Affected**:
- [file:line] - Expected [type A], got [type B]

```typescript
// Current (error)
const user = {
  name: 'John',
  age: 30
};

const name: string = user; // Error: Object not assignable to string

// Fixed
const name: string = user.name; // OK
```

**Patterns Found**:
1. **Object vs Property**
   ```typescript
   // Wrong
   const userData: User = getUsers();
   
   // Right
   const userData: User[] = getUsers();
   ```

2. **Union Type Handling**
   ```typescript
   // Wrong
   const value: string = someUnion; // Type 'string | number'
   
   // Right
   const value: string | number = someUnion;
   // or
   const value: string = typeof someUnion === 'string' ? someUnion : '';
   ```

3. **Generic Type Mismatch**
   ```typescript
   // Wrong
   const map = new Map<string, number>();
   map.set('key', 'value'); // Error: string not assignable to number
   
   // Right
   map.set('key', 42);
   ```

### 2. Argument Type Mismatch (TS2345)

**Files Affected**:
- [file:line] - Argument of type [A] not assignable to parameter of type [B]

```typescript
// Current (error)
function greet(name: string): string {
  return `Hello, ${name}`;
}

greet(123); // Error: number not assignable to string

// Fixed
greet('John'); // OK
```

**Common Patterns**:

1. **Function Arguments**
   ```typescript
   // Wrong
   const numbers = [1, 2, 3];
   numbers.forEach(num => console.log(num * '1')); // Error
   
   // Right
   numbers.forEach(num => console.log(num * 1));
   ```

2. **Callback Types**
   ```typescript
   // Wrong
   Promise.then((result: string) => {
     return result.toUpperCase();
   });
   
   // Right
   Promise.resolve<string>('test').then((result) => {
     return result.toUpperCase();
   });
   ```

3. **Event Handlers**
   ```typescript
   // Wrong
   <button onClick={(e) => e.target.value}>Submit</button>
   
   // Right
   <button onClick={(e: React.MouseEvent<HTMLButtonElement>) => 
     e.currentTarget.value
   }>Submit</button>
   ```

### 3. Null/Undefined Checks (TS2538)

**Files Affected**:
- [file:line] - Object is possibly 'undefined'

```typescript
// Current (error)
const user = getUser();
console.log(user.name); // Error: user might be undefined

// Fixed (Option 1 - Optional Chaining)
const user = getUser();
console.log(user?.name);

// Fixed (Option 2 - Type Guard)
const user = getUser();
if (user) {
  console.log(user.name);
}

// Fixed (Option 3 - Non-null Assertion)
const user = getUser()!;
console.log(user.name);
```

**Patterns**:

1. **Array Access**
   ```typescript
   // Wrong
   const first = items[0];
   console.log(first.name); // Error
   
   // Right
   const first = items[0];
   console.log(first?.name);
   
   // Or
   const [first] = items;
   if (first) console.log(first.name);
   ```

2. **Object Properties**
   ```typescript
   // Wrong
   const name = user?.profile?.name; // Multiple optional chains
   
   // Right - Type narrowing
   const user = getUser();
   if (user?.profile) {
     const name = user.profile.name;
   }
   ```

3. **Function Returns**
   ```typescript
   // Wrong
   function getUser(): User | undefined {
     return database.findUser();
   }
   
   const name = getUser().name; // Error
   
   // Right
   const user = getUser();
   const name = user ? user.name : 'Anonymous';
   ```

---

## Major Type Errors (Should Fix)

### 1. Implicit Any (TS7006)

**Files Affected**: [count]

**Example**:
```typescript
// Current (warning)
function processData(data) { // Implicit any parameter
  return data.items.map(item => item.value);
}

// Fixed
interface Data {
  items: { value: string }[];
}

function processData(data: Data) {
  return data.items.map(item => item.value);
}
```

**Common Fixes**:

1. **Parameters**
   ```typescript
   // Wrong
   function fetchData(url, options) {}
   
   // Right
   interface FetchOptions {
     method: string;
     headers?: Record<string, string>;
   }
   
   function fetchData(url: string, options: FetchOptions) {}
   ```

2. **Variables**
   ```typescript
   // Wrong
   let result; // Implicit any
   
   // Right
   let result: string[] = [];
   ```

3. **Object Properties**
   ```typescript
   // Wrong
   const config = {
     debug: true,
     timeout: 30
   };
   
   // Right
   interface Config {
     debug: boolean;
     timeout: number;
   }
   
   const config: Config = {
     debug: true,
     timeout: 30
   };
   ```

### 2. Property Does Not Exist

**Files Affected**:
- [file:line] - Property [X] does not exist on type [Y]

```typescript
// Current (error)
const user = { name: 'John' };
console.log(user.email); // Error: email doesn't exist

// Fixed (Option 1 - Add property)
interface User {
  name: string;
  email?: string; // Optional property
}

const user: User = { name: 'John' };
console.log(user.email); // OK (undefined)

// Fixed (Option 2 - Type assertion)
console.log((user as any).email); // Not recommended
```

### 3. Generic Type Inference Issues

**Files Affected**:
- [file:line] - Generic type inference failed

```typescript
// Current (error)
function createPair<T>(a: T, b: T) {
  return [a, b];
}

const pair = createPair('hello', 123); // Error: types don't match

// Fixed
const pair1 = createPair<string>('hello', 'world'); // Explicit
const pair2 = createPair('hello', 'world'); // TypeScript infers string
```

---

## Minor Type Warnings (Consider Fixing)

### 1. Unused Type Imports

```typescript
// Wrong
import { User, Role, Permission, Department } from './types';

// Only User used...

// Right
import { User } from './types';
```

### 2. Redundant Type Annotations

```typescript
// Wrong (redundant)
const name: string = 'John';

// Right (inferable)
const name = 'John';
```

### 3. Type Assertion Warnings

```typescript
// Warning (needless assertion)
const user = { name: 'John' } as User;

// Better
interface User {
  name: string;
}
const user: User = { name: 'John' };
```

---

## Configuration Improvements

### Recommended tsconfig.json
```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitThis": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "noPropertyAccessFromIndexSignature": true,
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "build"]
}
```

---

## Type Safety Best Practices

### 1. Use Strict Mode
```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true
  }
}
```

### 2. Define Clear Interfaces
```typescript
// Good
interface CreateUserRequest {
  email: string;
  name: string;
  password: string;
  role?: 'admin' | 'user';
}

interface UserResponse {
  id: string;
  email: string;
  name: string;
  role: 'admin' | 'user';
  createdAt: string;
}
```

### 3. Use Type Guards
```typescript
// Type guard function
function isUser(obj: unknown): obj is User {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    'id' in obj &&
    'email' in obj
  );
}

// Usage
if (isUser(data)) {
  console.log(data.email); // TypeScript knows it's User
}
```

### 4. Prefer Interfaces for Objects
```typescript
// Interface (extensible)
interface User {
  id: string;
  name: string;
}

// Type (for unions, primitives)
type Status = 'pending' | 'approved' | 'rejected';
```

---

## Auto-Fix Commands

### Generate Missing Types
```bash
# Infer types from JavaScript
npx tsc --noEmit --allowJs

# Quick fixes
npx tsc --noEmit --fix
```

### IDE Integration
- **VSCode**: Use "Quick Fix" (Ctrl+.)
- **JetBrains**: Use "Inspect Code"
- **Command Line**: `npx tsc --noEmit`

---

## Migration Guide

### From JavaScript to TypeScript
1. **Add tsconfig.json**
   ```json
   {
     "compilerOptions": {
       "allowJs": true,
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": false
     }
   }
   ```

2. **Rename Files**
   - `file.js` → `file.ts`
   - `file.jsx` → `file.tsx`

3. **Incrementally Add Types**
   - Start with function signatures
   - Add interfaces for complex objects
   - Enable strict mode gradually

---

## Compliance Checklist

- [ ] All critical type errors resolved
- [ ] Type safety improved
- [ ] Interfaces defined
- [ ] Type guards added
- [ ] Configuration optimized
- [ ] CI updated with type checking
- [ ] Documentation updated

---

## Next Steps

### Immediate Actions
1. Run `npx tsc --noEmit` to see all errors
2. Fix critical type mismatches
3. Add missing type definitions

### Short-term Improvements
1. Enable strict mode
2. Add `noImplicitAny`
3. Create type definitions for APIs

### Long-term Goals
1. 100% TypeScript coverage
2. Strict null checks
3. Complete type safety
