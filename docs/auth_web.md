# Web guard

auth.use('web')
uses a session/cookie to login user
used when creating a server rendered application or API havung a client running on the same domain

npm i @adonisjs/session
node ace configure @adonisjs/session

## Login

auth.attempt(email) - look user on database an verify their password, if its correct it will call auth.login

Route.post('login', async ({ auth, request }) => {
const email = request.input('email')
const password = request.input('password')

await auth.use('api').attempt(email, password)
})

**or**

look user manually:

const user = await User
.query()
.where('email', email)
.whereNull('is_deleted')
.firstOrFail()

await auth.use('web').login(user) - create a session
await auth.use('web').loginViaId(1)

all of this option accept a boolean value as the last argument to a remeber me cookie

const rememberMe = true

await auth.use('web').attempt(email, password, rememberMe)
await auth.use('web').login(user, rememberMe)
await auth.use('web').loginViaId(1, rememberMe)

If the user session expires, the remember me cookie will be used to create another session for the user. The remember me token is stored inside the users table itself and currently only one remember me token is allowed.

- auth.use('web').isLoggedIn = true
- auth.use('web').verifyCredentials(email, password) = true

## Logged

### Authenticate subsequent request

Route.get('dashboard', async ({ auth }) => {
await auth.use('api').authenticate()

// ✅ Request authenticated
console.log(auth.user!)
})

await auth.use('web').authenticate() - verify the session and the user inside the database

- auth.use('web').isLoggedIn = true
- auth.use('web').isAuthenticated = true
- auth.use('web').check()= true

## Logout

await auth.use('web').logout() - destroy the session and the remember me cookie setting to null

- auth.use('web').isLoggedOut = true
