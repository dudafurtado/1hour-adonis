npm i @adonisjs/auth
node ace configure @adonisjs/auth
! # CREATE: app/Models/User.ts
! # CREATE: database/migrations/1619578304190_users.ts
! # CREATE: contracts/auth.ts
! # CREATE: config/auth.ts
! # CREATE: app/Middleware/Auth.ts file already exists
! # CREATE: app/Middleware/SilentAuth.ts file already exists
! # UPDATE: .adonisrc.json { providers += "@adonisjs/auth" }
! # CREATE: ace-manifest.json file

config/auth.ts

You can access the auth instance inside your route handlers using the ctx.auth property. 

## Login user
Route.post('login', async ({ auth, request }) => {
  const email = request.input('email')
  const password = request.input('password')

  await auth.use('api').attempt(email, password)
})

## Authenticate subsequent request
Route.get('dashboard', async ({ auth }) => {
  await auth.use('api').authenticate()

  // ✅ Request authenticated
  console.log(auth.user!)
})

# Basic auth

import Route from '@ioc:Adonis/Core/Route'

Route
  .get('posts', async ({ auth }) => {
    await auth.use('basic').authenticate()

    return `You are logged in as ${auth.user!.email}`
  })


@ looks funny https://docs.adonisjs.com/guides/auth/social