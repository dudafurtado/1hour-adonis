# Controllers

## Firts commands
node ace make:controller Post
-r, --resource --> adds resourceful methods to the controller class
-e, --exact --> create controller with the exact name as provided

## Sintaxe
public async index (ctx: HttpContextContract)
Route.get('/posts', 'PostsController.index')

Route.get('/posts', () => {
  return 'List all posts'
})

Route.get('/posts/create', () => {
  return 'Display a form to create a post'
})

Route.post('/posts', async () => {
  return 'Handle post creation form request'
})

Route.get('/posts/:id', () => {
  return 'Return a single post'
})

Route.get('/posts/:id/edit', () => {
  return 'Display a form to edit a post'
})

Route.put('/posts/:id', () => {
  return 'Handle post update form submission'
})

Route.delete('/posts/:id', () => {
  return 'Delete post'
})

## Use of resource = all metodes of CRUD in one

Route.resource('posts', 'PostsController')
Route.resource('posts', 'PostsController').as('articles')

1. Except - delete the existence of other route that would be auto-generate. Probably because it's not been used.
`Route
.resource('/comments', 'CommentsController')
.except(['update', 'destroy'])`

2. Only - will use just the routes specified in the array
`Route
.resource('/comments', 'CommentsController')
.only(['index', 'show', 'store'])`

3. About a middleware that should be use only in some routes
`Route.resource('/comments', 'CommentsController').middleware({
  '*': ['auth'],
})`

`Route.resource('/comments', 'CommentsController').middleware({
  create: ['auth'],
  store: ['auth'],
  destroy: ['auth'],
})`

4. Params For is a way to define what should be named the param for the routes
`Route
.resource('/comments', 'CommentsController')
.paramFor('users', 'user')`
=
GET /users/:user
GET /users/:user/edit
PUT,PATCH /users/:user
DELETE /users/:user
