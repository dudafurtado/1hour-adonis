# Validator 
Parsing / review / analysis with validation schema types and validating the request body itself.

`const postSchema = schema.create({  
  title: schema.string(),  
  description: schema.string(),  
  categories: schema.array().members(schema.number()),  
})`

`const payload = await request.validate({ schema: postSchema })`

## Schema Composition 

extracts the static types from the schema definition

import { schema, rules } from '@ioc:Adonis/Core/Validator'

- schema.create - define the shape
- schema.string() - data type
- schema.number() - data type
- rules.email() - addicional validation
- rules.confirmed() - addicional validation
- rules.minLength(4) - addicional validation

! All the fields are required by default 

.optional() and .nullableAndOptional - allows null and undefined
.nullable() - allows null

schema: schema.create({
  fullName: schema.string.nullable(),
})

const payload =  await request.validate({
  schema: schema.create({
    fullName: schema.string.nullableAndOptional(),
  })
})

email: schema.string.optional()

## Validator classes 

`node ace make:validator CreateUser`
-> # CREATE: app/Validators/CreateUserValidator.ts

## Custom Messages

Messages just for the validation rule:
required: 'The {{ field }} is required to create a new account'
(used in all the field which fails this rule)

For individual fields 
'username.unique': 'Username not available'
(used especific for the field username and your rule unique)

### Nested Objects and arrays
`const user = {
  username: 'dudafurtado'
}`
'user.username.required': 'Missing value for username',

`const tags = [ x, y, z ]`
'tags.*.number': 'Tags must be an array of numbers',

`const products = [
  {
    title: 'One'
  },
  {
    title: 'Two'
  }
]`
'products.*.title.required': 'Each product must have a title'

### Dynamic placeholders

! nested objects = user.profile.username

- {{ field }}
- {{ rule }}
- {{ option }}
The option passed by the validation maethods

`const arrayOfEnums: enum = [
  {
    choices: 'first'
  },
  {
    choices: 'second'
  }
]`
enum: 'The value of {{ field }} must be in {{ options.choices }}
(The enum rule will pass an array of choices, and some rules may not pass any options at all)

### Wildcard callback

'*': (field, rule, arrayExpressionPointer, options) => {
  return `${rule} validation error on ${field}`
}

'date.format': '{{ field }} must be formatted as {{ options. format }}',

.minLength(), .maxLength()

.range() - validation rule with 'start' and 'stop' options
'range': 'Candidate age must be between {{ options.start }} and {{ options.stop }} years'