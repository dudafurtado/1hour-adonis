# Views & Templates 

npm i @adonisjs/view
node ace configure @adonisjs/view

node ace make:view home
resources/views/home.edge
<p> Hello world. You are viewing the {{ request.url() }} page </p>

 ./app/views
 {
  "directories": {
    "views": "./app/views"
  }
}

## Component 

<button type="{{ type }}">
  {{ text }}
</button>

@!component('button', {
  text: 'Login',
  type: 'submit'
})

<button type="{{ type }}">{{ text }}</button>

@!component('button', {
  text: 'Login',
  type: 'submit',
  class: 'py-2 px-8 text-white bg-gray-800',
})