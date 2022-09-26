# Migrations
new migration to every schema change, create and alter table

node ace make:migration user 
! # CREATE: database/migrations/1630981615472_users.ts

up - to make happen 
down - to rollback

import BaseSchema from '@ioc:Adonis/Lucid/Schema'

export default class extends BaseSchema {
  protected tableName = 'users'

  public async up() {
    this.schema.createTable(this.tableName, (table) => {
      table.increments('id')
      table.timestamp('created_at', { useTz: true })
      table.timestamp('updated_at', { useTz: true })
    })
  }

  public async down() {
    this.schema.dropTable(this.tableName)
  }
}

node ace migration:run 

## Rollback the latest batch
node ace migration:rollback

## Rollback until the start of the migration
node ace migration:rollback --batch=0
node ace migration:reset
(executes the down)

## Rollback until batch 1
node ace migration:rollback --batch=1

## Re-creates the entire database
node ace migration:refresh

## Drop all the database tables
node ace migration:fresh

!! Rollback just when development stage, if the aplication is on production we should just alter table or create other migration to implement


this.schema.createTable()
this.schema.alterTable()
this.schema.renameTable()
this.schema.dropTable()

## To view the migrations in teh console instead of execute them
- Run
node ace migration:run --dry-run

- Rollback
node ace migration:rollback --dry-run

## A list of all the migrations
node ace migrations:status
await migrator.getList()


https://moment.github.io/luxon/api-docs/index.html#datetime