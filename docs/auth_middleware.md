# Auth middleware

import Route from '@ioc:Adonis/Core/Route'

Route
  .get('posts', async ({ auth }) => {
    return `You are logged in as ${auth.user!.email}`
  })
  .middleware('auth', { guards: ['basic'] })


### start/kernel.ts
Server.middleware.registerNamed({
  auth: () => import('App/Middleware/Auth')
})


Route.group(() => {
  
}).middleware('auth')


## Silent Auth middleware
The silent auth middleware silently checks if the user is logged-in or not. 
This middleware does not force the users to be logged-in, but will fetch their details if they are logged-in and provide it you through out the request lifecycle.
(public webpage)

### start/kernel.ts
Server.middleware.register([
  () => import('@ioc:Adonis/Core/BodyParser'),
  () => import('App/Middleware/SilentAuth')
])
