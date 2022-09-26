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

## Rules

1. Alpha - to only have letters
(works only with the string schema type)

username: schema.string({}, [
  rules.alpha()
])

name: schema.string([
  rules.alpha({
    allow: ['space', 'underscore', 'dash']
  })
])

2. Distinct - ensure that all values of a array are unique
(works only with the array schema type)

const products = [
  {
    id: 1,
    quantity: 4
  },
  {
    id: 3,
    quantity: 12
  },
  {
    id: 7,
    quantity: 8
  }
]
products: schema
.array([
  rules.distinct('id')
])
.members(schema.object().members({
  id: schema.number(),
  quantity: schema.number(),
}))

const tags = [1, 4, 10, 25, 28, 0, 7, 13, 2]
tags: schema.array([
  rules.distinct('*')
])
.members(schema.number())

3. Email - fomat of email valid
(works only with string type schema)

email: schema.string([
  rules.email({
    ignoreMaxLength: true,
    allowIpDomain: true,
    domainSpecifValidation: true,
  })
])

email: schema.string({
  rules.email(),
  rules.normalizeEmail({
    allLowercase: true,
    gmailRemoveDots: true,
    gmailRemoveSubaddres: true,
  })
})

! validator.js --> isEmail()
https://www.npmjs.com/package/validator
{ 
  allow_display_name: true --> the validator will also match Display Name <email-address>, 
  require_display_name: true --> the validator will reject strings without the format,
  allow_utf8_local_part: false --> the validator will not allow any non-English UTF8 character in email address' local part, 
  require_tld: false --> e-mail addresses without having TLD in their domain will also be matched, 
  ignore_max_length: true --> the validator will not check for the standard max length of an email,
  allow_ip_domain: true --> the validator will allow IP addresses in the host part, 
  domain_specific_validation: true --> some additional validation will be enabled, e.g. disallowing certain syntactically valid email addresses that are rejected by GMail, 
  blacklisted_chars: 'something' -->  the validator will reject emails that include any of the characters in the string, in the name part, 
  host_blacklist: ['duda', 'furtado', 'melo'] --> is set to an array of strings and the part of the email after the @ symbol matches one of the strings defined in it, the validation fails
}

4. Exists - exists inside a database and a column
rules.exists({ table: 'categories', column: 'shop' })
rules.exists({
  table: 'users',
  column: 'username',
  caseInsensitive: true,
})
rules.exists({
  table: 'categories',
  column: 'slug',
  where: {
    tenant_id: 1,
    status: 'active',
  },
})

5. Unique - ensure that the value does not exists inside the database and column
email: schema.string({}, [
  rules.unique({ table: 'users', column: 'email' })
])

6. maxLength || minLength
rules.maxLength(40)
.array([
  rules.maxLength(10)
])

rules.minLength(4)
.array([
  rules.minLength(1)
])

! possibility of custom message 
{
  'minLength': 'The array must have minimum of {{ options.minLength }} items',
}

7. Trim
rules.trim()

8. Range

9. url - the value need to be formatted as a valid url string
also to be from a certain domain
website: schema.string([
  ules.url()
])

website: schema.string([
  rules.url({
    bannedHosts: [
      'acme.com',
      'example.com'
    ]
  })
])

twitterProfile: schema.string([
  ules.url({
    allowedHosts: ['twitter.com']
  })
])

website: schema.string([
  rules.url({
    protocols: ['http', 'https', 'ftp'],
    requireTld: true,
    requireProtocol: false,
    requireHost: true,
    allowedHosts: [],
    bannedHosts: [],
    validateLength: false
  })
])