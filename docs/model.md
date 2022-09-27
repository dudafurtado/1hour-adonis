# Base Model

1. Static create
   create a model instance and persist to database
   const user = await User.create({
   email: 'virk@adonisjs.com',
   password: 'secret',
   })

2. Static createMany
   Create multiple instances of a model and persist them to the database.
   const user = await User.createMany([
   {
   email: 'virk@adonisjs.com',
   password: 'secret',
   },
   {
   email: 'romain@adonisjs.com',
   password: 'secret',
   },
   ])

3. Static find
   find a row from database or return null
   const user = await User.find(1)
   if (!user) {
   return
   }

4. Static findOrFail
   try to find the row and if not will an exception
   const user = await User.findOrFail(1)

5. static findBy
   Find a row inside the database by using a key-value pair
   find on this column - this value
   const user = await User.findBy('email', 'virk@adonisjs.com')

6. static findByOrFail
   const user = await User.findByOrFail('email', 'virk@adonisjs.com')

7. static first
   Returns the first row from the database
   const user = await User.first()

8. static firstOrFail
   it will raise an exception when the row doesn't exists.
   const user = await User.firstOrFail()

9. static findMany
   Find multiple model instances of an array of values for the model primary key.
   where in SQL and always returns an array.
   const users = await User.findMany([1, 2, 3])

10. static firstOrNew
    const searchCriteria = {
    email: 'virk@adonisjs.com',
    }

const savePayload = {
name: 'Virk',
email: 'virk@adonisjs.com',
password: 'secret'
}

const user = await User.firstOrNew(searchCriteria, savePayload)

11. static updateOrCreate
    updates the existing row or creates a new one
    const searchCriteria = {
    id: user.id
    }

const savePayload = {
total: getTotalFromSomeWhere()
}

const cart = await Cart.updateOrCreate(searchCriteria, savePayload)

# Belongs to

https://docs.adonisjs.com/reference/orm/naming-strategy#relationlocalkey

1. relationName
   The relationship name. It is a property name defined on the parent model.
   class Post extends BaseModel {
   @belongsTo(() => User)
   public author: BelongsTo<typeof User>
   }

Post.$getRelation('author').relationName // 'author'

2. relatedModel
   Reference to the relationship model. The property value is a function that returns the related model.
   class Post extends BaseModel {
   @belongsTo(() => User)
   public author: BelongsTo<typeof User>
   }

Post.$getRelation('author').relatedModel() // User

3. localKey
   class Post extends BaseModel {
   @belongsTo(() => User, {
   localKey: 'id', // id column on "User" model
   })
   public author: BelongsTo<typeof User>
   }

4. foreignKey
   class Post extends BaseModel {
   @column()
   public userId: number

@belongsTo(() => User, {
foreignKey: 'userId', // userId column on "Post" model
})
public author: BelongsTo<typeof User>
}

5. onQuery
   to modify the relationship queries. You can define it at the time of declaring the relation.
   class Post extends BaseModel {
   @column()
   public userId: number

@belongsTo(() => User, {
onQuery(query) {
query.where('accountStatus', 'active')
}
})
public author: BelongsTo<typeof User>
}
