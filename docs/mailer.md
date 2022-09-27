# Mailer 

npm i @adonisjs/mail
node ace configure @adonisjs/mail

config/mail
env - 
MAILGUN_API_KEY
MAILGUN_DOMAIN

await Mail.use('mailgun').send((message) => {
  message.subject('Welcome Onboard!')
}, {
  oTags: ['signup'],
})

import Mail from '@ioc:Adonis/Addons/Mail'

class UsersController {
  public async store() {
    await Mail.send((message) => {
      message
        .from('info@example.com')
        .to('virk@adonisjs.com')
        .subject('Welcome Onboard!')
        .htmlView('emails/welcome', { name: 'Virk' })
    })
  }
}

await Mail.use('mailgun').sendLater(() => {
})

##  Email templates

node ace make:view emails/welcome
! # create    resources/views/emails/welcome.edge

<h1> Welcome {{ user.fullName }} </h1>
<p>
  <a href="{{ url }}">Click here</a> to verify your email address.
</p>

await Mail.sendLater((message) => {
  message.htmlView('emails/welcome', {
    user: { fullName: 'Some Name' },
    url: 'https://your-app.com/verification-url',
  })
})

Route.get('/mailgun', async ({ response }: HttpContextContract) => {
  await Mail.send((message: any) => {
    message
      .from('no-reply@wdit.in')
      .to('furtadoduda@hotmail.com')
      .subject('Welcome Onboard!')
      .htmlView('email/auth', { link: 'http://localhost:3333/' })
  })

  return response.status(200).json({
    message: "Everything going well"
  })
})