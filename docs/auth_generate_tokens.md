# API tokens 

auth.attempt() --> check the user from database and verifies their password  
! If the user credentials are correct, it will internally call the auth.generate method and returns the token

`Route.post('login', async ({ auth, request, response }) => {  
  const email = request.input('email')  
  const password = request.input('password')  

  try {  
    const token = await auth.use('api).attempt(email, password)  
    return token  
  } catch (error) {  
    return response.unauthorized('Invalid credentials')  
  }  
})`

auth.genrate() || auth.login() -->  manually lookup the user, verify the passqord and call the auth.generate to generate the token

import User from 'App/Models/User'
import Route from '@ioc:Adonis/Core/Route'
import Hash from '@ioc:Adonis/Core/Hash'

Route.post('login', async ({ auth, request, response }) => {
  const email = request.input('email')
  const password = request.input('password')

  // Lookup user manually
  const user = await User
    .query()
    .where('email', email)
    .where('tenant_id', getTenantIdFromSomewhere)
    .whereNull('is_deleted')
    .firstOrFail()

  // Verify password
  if (!(await Hash.verify(user.password, password))) {
    return response.unauthorized('Invalid credentials')
  }

  // Generate token
  const token = await auth.use('api').generate(user)
})

## Expires time when creating then

await auth.use('api').attempt(email, password, {
  expiresIn: '7 days'
})

await auth.use('api').generate(user, {
  expiresIn: '30 mins'
})

! for SQL storage, you will have write a custom script and delete token with expires_at timestamp smaller than today.

## Meta data

await auth.use('api').attempt(email, password, {
  ip_address: '192.168.1.0'
})

## toJSON

{
  type: 'bearer',
  token: 'the-token-value',
  expires_at: '2021-04-28T17:43:37.235+05:30'
  expires_in: 604800
}

##  Authenticate subsequent requests
Once the client receives the API token, they must send it back on every HTTP request under the Authorization header.
Authorization = Bearer TOKEN_VALUE

##  Revoke tokens
During the logout phase, you can revoke the token by deleting it from the database. The token again must be sent under the Authorization header.

import Route from '@ioc:Adonis/Core/Route'

Route.post('/logout', async ({ auth, response }) => {
  await auth.use('api').revoke()
  return {
    revoked: true
  }
})

## isLoggedIn
await auth.use('api').attempt(email, password)
auth.use('api').isLoggedIn

auth.use('api').isLoggedOut
auth.use('api').isAuthenticated
