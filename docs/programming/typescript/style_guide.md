# TypeScript Style Guide


TypeScript Style Guide provides a concise set of conventions and best practices used to create consistent, maintainable code.

## Introduction

TypeScript Style Guide provides a concise set of conventions and best practices used to create consistent, maintainable code.

As projects grow in size and complexity, maintaining code quality and ensuring consistent practices becomes increasingly challenging. Defining and following a standard way to write TypeScript applications brings consistent codebase and faster development cycles.

## Table of Contents

<TableOfContents items={toc} />

## About Guide

As any code style guide is opinionated, this is no different as it tries to set conventions (sometimes arbitrary) that govern our code.

Since "consistency is the key", style guide strives to enforce majority of the rules by using automated tooling as ESLint, TypeScript, Prettier, etc. Still certain design and architectural decisions must be followed which are described with conventions bellow.

Style Guide requires you to use:

- [TypeScript v5](https://github.com/microsoft/TypeScript)
- [typescript-eslint v6](https://github.com/typescript-eslint/typescript-eslint) with [`strict-type-checked`](https://typescript-eslint.io/linting/configs/#strict-type-checked),
  [`stylistic-type-checked`](https://typescript-eslint.io/linting/configs/#stylistic-type-checked) configurations enabled.

Style Guide assumes using, but is not limited to:

- [React](https://github.com/facebook/react) as UI library for frontend conventions.
- [Jest](https://jestjs.io/) and [Testing Library](https://testing-library.com/) for testing conventions.

## TLDR

- Strive for data immutability. [&#11107;](#data-immutability)
- Embrace const assertions. [&#11107;](#const-assertion)
- Avoid type assertions. [&#11107;](#type--non-nullability-assertions)
- Strive for functions to be pure, stateless and have single responsibility. [&#11107;](#functions)
- Majority of function arguments should be required (use optional sparingly). [&#11107;](#required--optional-args)
- Strong emphasis to keep naming conventions consistent and readable. [&#11107;](#naming-conventions)
- Use named exports. [&#11107;](#named-export)
- Code is organized and grouped by feature. Collocate code as close as possible to where it's relevant. [&#11107;](#code-collocation)
- UI components must only show derived state and send events, nothing more (no business logic). [&#11107;](#component-types)
- Test business logic, not implementation details. [&#11107;](#what--how-to-test)

## Data Immutability

Majority of the data should be immutable (use `Readonly`, `ReadonlyArray`, always return new array, object etc). To keep cognitive load for future developers low, try to keep data objects small.  
As an exception mutations should be used sparingly in cases where truly necessary: complex objects, performance reasoning etc.

## Types

### Type Definition

TypeScript offers two options for type definitions - `type` and `interface`. As they come with some functional differences, we try to limit syntax difference and pick one for consistency.

All types must be defined with `type` alias [(eslint rule)](https://typescript-eslint.io/rules/consistent-type-definitions/#type).

<Chip>
  Consider using interfaces if developing package that can be further extended, team is more comfortable working with
  interfaces etc. In such case disable lint rule where needed e.g. using type unions (type Status = 'loading' | 'error')
  etc.
</Chip>

```ts
// ❌ Avoid interface definitions
interface UserRole = 'admin' | 'guest'; // invalid - interface can't define (commonly used) type unions

interface UserInfo {
  name: string;
  role: 'admin' | 'guest';
}

// ✅ Use type definition
type UserRole = 'admin' | 'guest';

type UserInfo = {
  name: string;
  role: UserRole;
};

```

In case of declaration merging (e.g. extending third-party library types) use `interface` and disable lint rule.

```ts
// types.ts
declare namespace NodeJS {
  // eslint-disable-next-line @typescript-eslint/consistent-type-definitions
  export interface ProcessEnv {
    NODE_ENV: 'development' | 'production';
    PORT: string;
    CUSTOM_ENV_VAR: string;
  }
}

// server.ts
app.listen(process.env.PORT, () => {...}
```

### Type & Non-nullability Assertions

Type assertions `user as User` and non-nullability assertions `user!.name` are unsafe. Both only silence TypeScript compiler and increase the risk of crashing application at runtime.  
 They can only be used as last resort exceptions (e.g. third party library types mismatch etc.) with strong rational why being introduced into codebase.

```ts
// ❌ Avoid type & non-nullability assertions
type User = { id: string; username: string; avatar: string | null };
const user = { name: 'Nika' } as User;
renderUserAvatar(user!.avatar);
```

### Type any

`any` data type must not be used as it represents literally “any” value that TypeScript defaults to and skips type checking since it cannot infer the type. As such, any is dangerous, it can mask severe programming errors.  
 If error truly cannot be resolved as safer option use type `unknown` since it does not allow dereferencing all properties or as last resort use `@ts-expect-error` ([Type Error](#type-error)).

### Type Inference

<p>
  As rule of thumb, explicitly declare a type when it help narrows it.
  <Chip>
    Just because you don't need to add types, doesn't mean you shouldn't. In some cases explicit type declaration can
    increase code readability and intent.
  </Chip>
</p>

```ts
// ❌ Avoid
// Type can be inferred
const userRole: string = 'admin'; // Type 'string'
const employees = new Map<string, number>([['gabriel', 32]]);
const [isActive, setIsActive] = useState<boolean>(false);

// Type can be narrowed
const employees = new Map(); // Type 'Map<any, any>'
type UserRole = 'admin' | 'guest';
const [userRole, setUserRole] = useState('admin'); // Type 'string'

// ✅ Use
const USER_ROLE = 'admin'; // Type 'admin'
const employees = new Map([['gabriel', 32]]); // Type 'Map<string, number>'
const [isActive, setIsActive] = useState(false); // Type 'boolean'

const employees = new Map<string, number>(); // Type 'Map<string, number>'
employees.set('gabriel', 32);
type UserRole = 'admin' | 'guest';
const [userRole, setUserRole] = useState<UserRole>('admin'); // Type 'UserRole'

```

### Return Types

Including return type annotations is highly encouraged, altought not required ([eslint rule](https://typescript-eslint.io/rules/explicit-function-return-type/)).

Consider benefits when explicitly typing the return value of a function:

- Return values makes it clear and easy to understand to any calling code what type is returned.
- In the case where there is no return value, the calling code doesn't try to use the undefined value when it shouldn't.
- Surface potential type errors faster in the future if there are code changes that change the return type of the function.
- Easier to refactor, since it ensures that the return value is assigned to a variable of the correct type.
- Similar to writing tests before implementation (TDD), defining function arguments and return type, gives you the opportunity to discuss the feature functionality and its interface ahead of implementation.
- Although type inference is very convenient, adding return types can save TypeScript compiler a lot of work.

### Type Error

If TypeScript error can't be mitigated, as last resort use `@ts-expect-error` to suppress it ([eslint rule](https://typescript-eslint.io/rules/prefer-ts-expect-error/)). If at any future point suppressed line becomes error-free, TypeScript compiler will indicate it.  
`@ts-ignore` is not allowed, while `@ts-expect-error` can be used with provided description ([eslint rule](https://typescript-eslint.io/rules/ban-ts-comment/#allow-with-description)).

```ts
// ❌ Avoid @ts-ignore
// @ts-ignore
const result = doSomething('hello');

// ✅ Use @ts-expect-error with description
// @ts-expect-error: the library definition is wrong
const result = doSomething('hello');
```

### Array Types

Array types must be defined with generic syntax ([eslint rule](https://typescript-eslint.io/rules/array-type/#generic)).

<Chip>
  As there is no functional difference between 'generic' and 'array' definition, feel free to set the one your team
  finds most readable to work with.
</Chip>

```ts
// ❌ Avoid
const x: string[] = ['foo', 'bar'];
const y: readonly string[] = ['foo', 'bar'];

// ✅ Use
const x: Array<string> = ['foo', 'bar'];
const y: ReadonlyArray<string> = ['foo', 'bar'];
```

## Functions

Function conventions should be followed as much as possible (some of them derives from functional programming basic concepts):

### General

Function:

- should have single responsibility.
- should be stateless where the same input arguments return same value every single time.
- should accept at least one argument and return data.
- should not have side effects, but be pure. It's implementation should not modify or access variable value outside its local environment (global state, fetching etc.).

### Single Object Arg

To keep function readable and easily extensible for the future (adding/removing args), strive to have single object as the function arg, instead of multiple args.  
 As exception this not applies when having only one primitive single arg (e.g. simple functions isNumber(value), implementing currying etc.).

```ts
// ❌ Avoid having multiple arguments
transformUserInput('client', false, 60, 120, null, true, 2000);

// ✅ Use options object as argument
transformUserInput({
  method: 'client',
  isValidated: false,
  minLines: 60,
  maxLines: 120,
  defaultInput: null,
  shouldLog: true,
  timeout: 2000,
});
```

### Required & Optional Args

Strive to have majority of args required and use optional sparingly.  
 If function becomes to complex it probably should be broken into smaller pieces.  
 An exaggerated example where implementing 10 functions with 5 required args each, is better then implementing one "can do it all" function that accepts 50 optional args.

### Args as Discriminated Union

When applicable use **discriminated union type** to eliminate optional args, which will decrease complexity on function API and only necessary/required args will be passed depending on its use case.

```ts
// ❌ Avoid optional args as they increase complexity of function API
type StatusParams = {
  data?: Products;
  title?: string;
  time?: number;
  error?: string;
};

// ✅ Strive to have majority of args required, if that's not possible,
// use discriminated union for clear intent on function usage
type StatusParamsSuccess = {
  status: 'success';
  data: Products;
  title: string;
};

type StatusParamsLoading = {
  status: 'loading';
  time: number;
};

type StatusParamsError = {
  status: 'error';
  error: string;
};

type StatusParams = StatusSuccess | StatusLoading | StatusError;

export const parseStatus = (params: StatusParams) => {...
```

## Variables

### Const Assertion

Strive to use const assertion (`as const`):

- type is narrowed
- object gets `readonly` properties
- array becomes `readonly` tuple

  ```ts
  // ❌ Avoid declaring constants without const assertion
  const BASE_LOCATION = { x: 50, y: 130 }; // Type { x: number; y: number; }
  BASE_LOCATION.x = 10;
  const BASE_LOCATION = [50, 130]; // Type number[]
  BASE_LOCATION.push(10);

  // ✅ Use const assertion
  const BASE_LOCATION = { x: 50, y: 130 } as const; // Type '{ readonly x: 50; readonly y: 130; }'
  const BASE_LOCATION = [50, 130] as const; // Type 'readonly [10, 20]'
  ```

### Enums & Const Assertion

Const assertion must be used over enum.

While enums can still cover use cases as const assertion would, we tend to avoid it. Some of the reasonings as mentioned in TypeScript documentation - [Const enum pitfalls](https://www.typescriptlang.org/docs/handbook/enums.html#const-enum-pitfalls), [Objects vs Enums](https://www.typescriptlang.org/docs/handbook/enums.html#objects-vs-enums), [Reverse mappings](https://www.typescriptlang.org/docs/handbook/enums.html#reverse-mappings)...

```ts
// ❌ Avoid using enums
enum UserRole {
  GUEST,
  MODERATOR,
  ADMINISTRATOR,
}

enum Color {
  PRIMARY = '#B33930',
  SECONDARY = '#113A5C',
  BRAND = '#9C0E7D',
}

// ✅ Use const assertion
const USER_ROLES = ['guest', 'moderator', 'administrator'] as const;
type UserRole = (typeof USER_ROLES)[number]; // Type "guest" | "moderator" | "administrator"

// Use satisfies if UserRole type is already defined - e.g. database schema model
type UserRoleDB = ReadonlyArray<'guest' | 'moderator' | 'administrator'>;
const AVAILABLE_ROLES = ['guest', 'moderator'] as const satisfies UserRoleDB;

const COLOR = {
  primary: '#B33930',
  secondary: '#113A5C',
  brand: '#9C0E7D',
} as const;
type Color = typeof COLOR;
type ColorKey = keyof Color; // Type "PRIMARY" | "SECONDARY" | "BRAND"
type ColorValue = Color[ColorKey]; // Type "#B33930" | "#113A5C" | "#9C0E7D"
```

### Null & Undefined

In TypeScript types `null` and `undefined` many times can be used interchangeably.  
For consistency strive to:

- Use `null` to explicitly state it has no value - assignment, return function type etc.
- Use `undefined` assignment when trying to exclude fields. E.g. in form fields, request payload, querying database ([Prisma differentiation](https://www.prisma.io/docs/concepts/components/prisma-client/null-and-undefined))...

## Naming

Setting aside convention on cache invalidation, but for the second hardest thing, clear naming with important context should be provided.

Strive to keep naming conventions consistent and readable, because another person will maintain the code you have written.

### Named Export

Named exports must be used to ensure that all imports follow a uniform pattern ([eslint rule](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-default-export.md)).  
This keeps variables, functions... names consistent across the entire codebase.  
Named exports have the benefit of erroring when import statements try to import something that hasn't been declared.

In case of exceptions e.g. Next.js pages, disable rule:

```ts
// .eslintrc.js
overrides: [
  {
    files: ["src/pages/**/*"],
    rules: { "import/no-default-export": "off" },
  },
],
```

### Naming Conventions

While it's often hard to find the best name, try optimize code for consistency and future reader by following conventions:

#### Abbreviations & Acronyms

Treat acronyms as whole words, with capitalized first letter only.

```ts
// ❌ Avoid
const FAQList = ['qa-1', 'qa-2'];
const generateUserURL(params) => {...}

// ✅ Use
const FaqList = ['qa-1', 'qa-2'];
const generateUserUrl(params) => {...}
```

In favor of readability, strive to avoid abbreviations, unless they are widely accepted and necessary.

```ts
// ❌ Avoid
const GetWin(params) => {...}

// ✅ Use
const GetWindow(params) => {...}
```

#### Variables

- **Locals**  
  Camel case  
  `products`, `productsFiltered`
- **Booleans**  
  Prefixed with `is`, `has` etc.  
  `isDisabled`, `hasProduct`
- **Constants**  
  Capitalized  
  `PRODUCT_ID`
- **Object constants**

  Singular, capitalized with const assertion and optionally satisfies type (if there is one).

  ```ts
  const ORDER_STATUS = {
    pending: 'pending',
    fulfilled: 'fulfilled',
    error: 'error',
  } as const satisfies OrderStatus;
  ```

#### Functions

Camel case  
`filterProductsByType`, `formatCurrency`

#### Generics

A name starts with the capital letter T `TRequest`, `TFooBar` (similar to [.Net internal](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-5.0) implementation or [Google style guide](https://google.github.io/styleguide/javaguide.html#s5.2.8-type-variable-names)).  
 Avoid (popular convention) naming generics with one character `T`, `K` etc., the more variables we introduce, the easier it is to mistake them.

```ts
// ❌ Avoid naming generics with one character
const createPair = <T, K extends string>(first: T, second: K): [T, K] => {
  return [first, second];
};
const pair = createPair(1, 'a');

// ✅ Name starts with the capital letter T
const createPair = <TFirst, TSecond extends string>(first: TFirst, second: TSecond): [TFirst, TSecond] => {
  return [first, second];
};
const pair = createPair(1, 'a');
```

[Eslint rule](https://typescript-eslint.io/rules/naming-convention) implements:

```ts
// .eslintrc.js
'@typescript-eslint/naming-convention': [
  'error',
  {
    selector: 'typeParameter',
    format: ['PascalCase'],
    custom: { regex: '^T[A-Z]', match: true },
  },
],
```

#### React Components

Pascal case  
 `ProductItem`, `ProductsPage`

#### Prop Types

React component name following "Props" postfix  
 `[ComponentName]Props` - `ProductItemProps`, `ProductsPageProps`

#### Callback Props

Event handler (callback) props are prefixed as `on*` - e.g. `onClick`.  
 Event handler implementation functions are prefixed as `handle*` - e.g. `handleClick` ([eslint rule](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-handler-names.md)).

```tsx
// ❌ Avoid inconsistent callback prop naming
<Button click={actionClick} />
<MyComponent userSelectedOccurred={triggerUser} />

// ✅ Use prop prefix 'on*' and handler prefix 'handle*'
<Button onClick={handleClick} />
<MyComponent onUserSelected={handleUserSelected} />
```

#### React Hooks

Camel case, prefixed as 'use' ([eslint rule](https://github.com/facebook/react/tree/main/packages/eslint-plugin-react-hooks)), symmetrically convention as `[value, setValue] = useState()` ([eslint rule](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/hook-use-state.md#rule-details))

```ts
// ❌ Avoid inconsistent useState hook naming
const [userName, setUser] = useState();
const [color, updateColor] = useState();
const [visible, setVisible] = useState();

// ✅ Use
const [name, setName] = useState();
const [color, setColor] = useState();
const [isActive, setIsActive] = useState();
```

Custom hook must always return an object:

```ts
// ❌ Avoid
const [products, errors] = useGetProducts();
const [fontSizes] = useTheme();

// ✅ Use
const { products, errors } = useGetProducts();
const { fontSizes } = useTheme();
```

### Comments

Comments in general should be avoided. Try to write expressive code and name things what they are before adding comments.

As an exception use comments only when you need to add context or explain choices that cannot be expressed through code.  
Comments should always be complete sentences. As rule of a thumb try to explain `why` in comments, not `how` and `what`.

```ts
// ❌ Avoid
// convert to minutes
const m = s * 60;
// avg users per minute
const myAvg = u / m;

// ✅ Use
const SECONDS_IN_MINUTE = 60;
const minutes = seconds * SECONDS_IN_MINUTE;
const averageUsersPerMinute = noOfUsers / minutes;

// TODO: Filtering should be moved to the backend once API changes are released.
// Issue/PR - https://github.com/foo/repo/pulls/55124
const filteredUsers = frontendFiltering(selectedUsernames);

// Use Fourier transformation to minimize information loss - https://github.com/dntj/jsfft#usage
const frequencies = signal.FFT();
```

## Source Organization

### Code Collocation

- Every application or package in monorepo has project files/folders organized and grouped by **feature**.
- **Collocate code as close as possible to where it's relevant.**
- Deep folder nesting should not represent an issue.

### Imports

Import paths can be relative, starting with `./` or `../`, or they can be absolute `@common/utils`.

To make import statements more readable and easier to understand:

- Relative imports `./sortItems` must be used when importing files within the same feature, that are 'close' to each other, which also allows moving feature around the codebase without introducing changes in these imports.
- Absolute imports `@common/utils` must be used in all other cases.
- All imports must be auto sorted by tooling e.g. [prettier-plugin-sort-imports](https://github.com/trivago/prettier-plugin-sort-imports), [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)...

```ts
// ❌ Avoid
import { bar, foo } from '../../../../../../distant-folder';

// ✅ Use
import { locationApi } from '@api/locationApi';

import { foo } from '../../foo';
import { bar } from '../bar';
import { baz } from './baz';
```

### Project Structure

Example frontend monorepo project, where every application has following file/folder structure:

```shell
apps/
├─ product-manager/
│  ├─ common/
│  │  ├─ components/
│  │  │  ├─ Button/
│  │  │  ├─ ProductTitle/
│  │  │  ├─ ...
│  │  │  └─ index.tsx
│  │  ├─ consts/
│  │  │  ├─ paths.ts
│  │  │  └─ ...
│  │  ├─ hooks/
│  │  └─ types/
│  ├─ modules/
│  │  ├─ HomePage/
│  │  ├─ ProductAddPage/
│  │  ├─ ProductPage/
│  │  ├─ ProductsPage/
│  │  │  ├─ api/
│  │  │  │  └─ useGetProducts/
│  │  │  ├─ components/
│  │  │  │  ├─ ProductItem/
│  │  │  │  ├─ ProductsStatistics/
│  │  │  │  └─ ...
│  │  │  ├─ utils/
│  │  │  │  └─ filterProductsByType/
│  │  │  └─ index.tsx
│  │  ├─ ...
│  │  └─ index.tsx
│  ├─ eslintrc.js
│  ├─ package.json
│  └─ tsconfig.json
├─ warehouse/
├─ admin-dashboard/
└─ ...
```

- `modules` folder is responsible for implementation of each individual page, where all custom features for that page are being implemented (components, hooks, utils functions etc.).
- `common` folder is responsible for implementations that are truly used across application. Since its a "global folder" it should be used sparingly.  
  If same component e.g. `common/components/ProductTitle` starts being used on more the one page, it shall be moved to common folder.

In case file-system based router (e.g. Nextjs) is being used as frontend framework, `pages` folder serves only as a router, where its responsibility is to define routes (no business logic implementation).

## Appendix - React

Since React components and hooks are also functions, [respective function conventions](#functions) applies.

### Props Type

```tsx
// ❌ Avoid using React.FC type
type FooProps = {
  name: string;
  score: number;
};

export const Foo: React.FC<FooProps> = ({ name, score }) => {

// ✅ Use props argument with type
type FooProps = Readonly<{
  name: string;
  score: number;
}>;

export const Foo = ({ name, score }: FooProps) => {...
```

### Props - Required & Optional

**Strive to have majority of props required and use optional sparingly.**

Especially when creating new component for first/single use case majority of props should be required. When component starts covering more use cases, introduce optional props.  
There are potential exceptions, where component API needs to implement optional props from the start (e.g. shared components covering multiple use cases, UI design system components - button `isDisabled` etc.)

As mentioned in function example - implementing 10 React components with 5 required props each, is better then implementing one "can do it all" function that accepts 50 optional props.  
If component becomes to complex it probably should be broken into smaller pieces.

**Use Discriminated Type**

When applicable use **discriminated type** to eliminate optional props, which will decrease complexity on component API and only necessary/required props will be passed depending on its use case.

```ts
// ❌ Avoid optional props as they increase complexity of component API
type StatusProps = {
  data?: Products;
  title?: string;
  time?: number;
  error?: string;
};

// ✅ Strive to have majority of props required, if that's not possible,
// use discriminated union for clear intent on component usage
type StatusSuccess = {
  status: 'success';
  data: Products;
  title: string;
};

type StatusLoading = {
  status: 'loading';
  time: number;
};

type StatusError = {
  status: 'error';
  error: string;
};

type StatusProps = StatusSuccess | StatusLoading | StatusError;

export const Status = (status: StatusProps) => {...
```

### Component Types

#### Container

- All container components have postfix "Container" or "Page" `[ComponentName]Container|Page`. Use "Page" postfix to indicate component it's an actual web page.
- Each feature has a container component (`AddUserContainer.tsx`, `EditProductContainer.tsx`, `ProductsPage.tsx` etc.)
- Includes business logic.
- API integration.
- Structure:
  ```
  ProductsPage/
  ├─ api/
  │  └─ useGetProducts/
  ├─ components/
  │  └─ ProductItem/
  ├─ utils/
  │  └─ filterProductsByType/
  └─ index.tsx
  ```

#### UI - Feature

- Representational components that are designed to fulfill feature requirements.
- Nested inside container component folder.
- Should follow [functions conventions](#functions) as much as possible.
- No API integration.
- Structure:
  ```
  ProductItem/
  ├─ index.tsx
  ├─ ProductItem.stories.tsx
  └─ ProductItem.test.tsx
  ```

#### UI - Design system

- Global Reusable/shared components used throughout whole codebase.
- Structure:
  ```
  Button/
  ├─ index.tsx
  ├─ Button.stories.tsx
  └─ Button.test.tsx
  ```

### Store & Pass Data

- Utilize storing state in the URL, especially for filtering, sorting etc.
- Don't sync URL state with local state.
- Consider passing data simply through props, using the URL, or composing children. Use global state (Zustand, Context) as a last resort.
- Use React compound components when components should belong and work together: `menu`, `accordion`,`navigation`, `tabs`, `list`,...  
  Always export compound components as:

  ```tsx
  // PriceList.tsx
  const PriceListRoot = ({ children }) => <ul>{children}</ul>;
  const PriceListItem = ({ title, amount }) => <li>Name: {name} - Amount: {amount}<li/>;

  // ❌
  export const PriceList = {
    Container: PriceListRoot,
    Item: PriceListItem,
  };
  // ❌
  PriceList.Item = Item;
  export default PriceList;

  // ✅
  export const PriceList = PriceListRoot as typeof PriceListRoot & {
    Item: typeof PriceListItem;
  };
  PriceList.Item = PriceListItem;

  // App.tsx
  import { PriceList } from "./PriceList";

  <PriceList>
    <PriceList.Item title="Item 1" amount={8} />
    <PriceList.Item title="Item 2" amount={12} />
  </PriceList>;
  ```

- UI components should show derived state and send events, nothing more.
- As in many programming languages functions args can be passed to the next function and on to the next etc.  
  Rect components are no different, where prop drilling should not become an issue.  
  If with app scaling prop drilling truly becomes an issue, try to refactor render method, local states in parent components, using composition etc.
- Data fetching is only allowed in container components.
- Use of server-state library is encouraged ([react-query](https://github.com/tanstack/query), [apollo client](https://github.com/apollographql/apollo-client)...).
- use of client-state library for global state is discouraged.  
  Reconsider if something should be truly global across application, e.g. `themeMode`, `Permissions` or even that can be put in server-state (e.g. user settings - `/me` endpoint). If still truly needed use [Zustand](https://github.com/pmndrs/zustand) or Context (not Redux, Mobx etc.).

## Appendix - Tests (Unit & Integration)

### What & How To Test

Automated test comes with benefits that helps us write better code and makes it easy to refactor, while bugs are caught earlier in the process.  
 Consider trade-offs of what and how to test to achieve confidence application is working as intended, while writing and maintaining tests doesn't slow the team down.

✅ Do:

- Implement test to be short, simple, and pleasant to work with. Intent of a test should be immediately visible.
- Strive to write tests in a way your app/package is used by a user, meaning test business logic.  
  E.g. given some user input, they receive the expected output for a process.
- All tests must be setup and implemented to run as standalone in isolation, where they don't depend on other tests order of execution.
- Tests should be resilient to changes.  
  Query HTML elements based on attributes that are unlikely to change. Order of priority must be followed as specified in [Testing Library](https://testing-library.com/docs/queries/about/#priority) - [role](https://testing-library.com/docs/queries/byrole), [label](https://testing-library.com/docs/queries/bylabeltext), [placeholder](https://testing-library.com/docs/queries/byplaceholdertext), [text contents](https://testing-library.com/docs/queries/bytext), [display value](https://testing-library.com/docs/queries/bydisplayvalue), [alt text](https://testing-library.com/docs/queries/byalttext), [title](https://testing-library.com/docs/queries/bytitle), [test ID](https://testing-library.com/docs/queries/bytestid).

❌ Don't:

- Don't test implementation details. When refactoring code, tests shouldn't change.
- Don't re-test the library/framework.
- Don't mandate 100% code coverage for applications.
- Don't test just to test.

  ```ts
  // ❌ Avoid
  it('should render user list', () => {
    render(<UserList />);
    expect(screen.getByText('Users List')).toBeInTheDocument();
  });
  ```

### Test Description

All test descriptions must follow naming convention as `it('should ... when ...')`.  
 [Eslint rule](https://github.com/jest-community/eslint-plugin-jest/blob/main/docs/rules/valid-title.md#mustmatch--mustnotmatch) implements regex:

```ts
// .eslintrc.js
'jest/valid-title': [
  'error',
  {
    mustMatch: { it: [/should.*when/u.source, "Test title must include 'should' and 'when'"] },
  },
],
```

```ts
// ❌ Avoid
it('accepts ISO date format where date is parsed and formatted as YYYY-MM');
it('after title is confirmed user description is rendered');

// ✅ Name test description as it('should ... when ...')
it('should return parsed date as YYYY-MM when input is in ISO date format');
it('should render user description when title is confirmed');
```

### Tooling Extension

Test can be run through npm scripts, but to improve development experience it's highly encouraged to use [Jest Runner](https://marketplace.visualstudio.com/items?itemName=firsttris.vscode-jest-runner) VS code extension so any single test can be run [instantly](https://github.com/mkosir/typescript-style-guide/raw/main/misc/vscode-jest-runner.gif), especially if testing app/package in larger codebase (monorepo).

```sh
code --install-extension firsttris.vscode-jest-runner
```

### Snapshot

Snapshot tests are discouraged in order to avoid fragility, which leads to "just update it" turn of mind, to achieve all the tests pass.  
 Exceptions can be made, with strong rational behind it, where test output has short and clear intent, whats actually being tested (e.g. design system library critical elements that shouldn't deviate).