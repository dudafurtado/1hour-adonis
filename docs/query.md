# Static query

const users = await User
.query()
.where('age', '>', 18)
.orderBy('id', 'desc')
.limit(20)

1. preload
   const users = await User.query().preload('posts')

2. sum
   query.sum('balance').as('accountsBalance')

3. has
   const users = await User.query().has('posts')
   await User.query().has('posts', '>=', 2)

4. where
   const subQuery = Database
   .from('user_teams')
   .select('team_id')
   .where('user_teams.user_id', user.id)

query.whereIn('id', subQuery)
postsQuery.where('status', 'published')