# Response

Route.get('/', (ctx) => {
  ctx.response.send('hello world')
})

return 'This is the homepage'
return { page: 'home' }
return new Date()
response.status(401)

// Redirect back
response.redirect().back()

// Redirect to a url
response.redirect().toPath('/some/url')

response.redirect().status(301).toPath('/some/url')
response.redirect().toRoute('PostsController.show', { id: 1 })

response.badRequest({ error: 'Invalid login credentials' })
response.forbidden({ error: 'Unauthorized' })
response.created({ data: user })