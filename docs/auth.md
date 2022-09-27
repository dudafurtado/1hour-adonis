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

guard = provider + driver

guards.provider:
1. Lucid -  uses *data models* to lookup users <-->
2. Database - uses *database querybuilder* directly to lookup users

guards.driver:
1. Web - **sessions** for auth state <-->
session driver makes use of sessions/cookies to login and authenticate requests.
2. API tokens - database **opaque tokens**
oat stands for opaque access token and uses stateless tokens for authenticating requests.
normally used with mobile application and to consume othe rs api 
3. Basic auth - **basic** authenticating requests
throught header 
Authentication - Bearer xyz
look for a email and password

contracts/auth.ts --> ProvidersList || GuardsList

- model name for authentication
- migration = true or false

provider store:
1. Database - uses SQL *table* for storing API tokens <-->
2. Redis - uses *Redis* for storing API tokens
  
You can access the auth instance inside your route handlers using the ctx.auth property.  

# Basic auth

import Route from '@ioc:Adonis/Core/Route'

Route
  .get('posts', async ({ auth }) => {
    await auth.use('basic').authenticate()

    return `You are logged in as ${auth.user!.email}`
  })


@ looks funny https://docs.adonisjs.com/guides/auth/social