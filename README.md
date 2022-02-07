# Fluent Schema Validator
Small validator for [Fluent JSON Schema](https://github.com/fastify/fluent-json-schema).

## Why
Fastify use Fluent JSON Schema for verifying schema of incoming request.
Although, Fastify support basic validation by default, it's not enough for real world application.

For instance, having unintentionally field in the value but the schema bypass it, or having string format of email but also bypass by fastify default validator.

In fact, Fastify docs recommended that you use custom validator instead.
This library help just that

## Feature
- Type strict to the schema
    - No unintentional field is bypassed
- Support most of the API
    - Whether it's `pattern` for RegEx, string `format`, `required` field or an advances `anyOf`, `allOf`, `not` is supported.
- Handle all type
    - Even if it's a simple string, complex array or full-fledge recursive object, it's all support.
- It just work.
    - Edgecase like unicode email format is handled, recursive object is handled, etc.
    - No headache of setting up compiler, Fluent Schema Validator provide just a simple function which works out of the box
    - Full compatability with Fluent Schema, Fast JSON Stringify, Fastify Schema or plain object
    - As long as your schema is formatted to plain Fast JSON Stringify schema, it will work
- Small and fast
    - No dependencies required, small bundle-size considering full-fledge feature.
- CJS, ESM, MJS, TypeScript
    - Yes, it's all support, TypeScript first bundled by SWC.

## Getting Start
Install the package.
```bash
npm install fluent-schema-validator fluent-json-schema

# or yarn
yarn add fluent-schema-validator fluent-json-schema

# or pnpm
pnpm add fluent-schema-validator fluent-json-schema
```

Then write a sample validation.
```typescript
// Assumming you're using TypeScript or mjs
import S from 'fluent-json-schema'
import validate from 'fluent-schema-validator'

const schema = S.object()
    .prop('username', S.string().required())
    .prop('password', S.string().required())

const value = {
    username: "saltyaom",
    password: "12345678
}

validate(value, schema) // true
```

### Unintentional field
Field which is not define in schema will not be tolerate
```typescript
import S from 'fluent-json-schema'
import validate from 'fluent-schema-validator'

const schema = S.object()
    .prop('username', S.string().required())
    .prop('password', S.string().required())

const value = {
    username: "saltyaom",
    password: "12345678",
    "not-in-schema": true
}

validate(value, schema) // false
```

### Optional field
Field which is not mark required, will be fine if not presented
```typescript
import S from 'fluent-json-schema'
import validate from 'fluent-schema-validator'

const schema = S.object()
    .prop('username', S.string().required())
    .prop('password', S.string())

const value = {
    username: "saltyaom",
}

validate(value, schema) // true
```

It would work the way you expected, see more in `test-case (__specs__ folder)`.