# Redis

npm i @adonisjs/redis
node ace configure @adonisjs/redis

config/redis.ts
import { redisConfig } from '@adonisjs/redis/build/config'

export default redisConfig({
  connection: Env.get('REDIS_CONNECTION'),

  connections: {
    local: {
      host: Env.get('REDIS_HOST'),
      port: Env.get('REDIS_PORT'),
      password: Env.get('REDIS_PASSWORD', ''),
      db: 0,
      keyPrefix: '',
    },
  },
})

import Redis from '@ioc:Adonis/Addons/Redis'

await Redis.set('foo', 'bar')
const value = await Redis.get('foo')

## Pub/Sub
node ace make:prldfile redis

import Redis from '@ioc:Adonis/Addons/Redis'

Redis.subscribe('user:signup', (user: string) => {
  console.log(JSON.parse(user))
})

Next, create a dummy route to publish to the user:signup channel on every new HTTP request.

import Route from '@ioc:Adonis/Core/Route'
import Redis from '@ioc:Adonis/Addons/Redis'

Route.get('/signup', async () => {
  await Redis.publish('user:signup', JSON.stringify({ id: 1 }))

  return 'handled'
})

The Redis.subscribe method listens for messages on a given channel.
The Redis.publish method is used to publish events to a given channel.
The messages are passed as string, since Redis doesn't support other data types during Pub/sub.

https://github.com/luin/ioredis