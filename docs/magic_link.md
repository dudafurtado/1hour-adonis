# Magic Link

1. Model
id, user id, magic code, date created, date updated, date deleted
magic code = nanoid (string)

2. Nano id 
npm i @nanoid@3.3
import { nanoid } from 'nanoid'

const code = nanoid(32)

3. Pass through the email/link that the user are gonna click and them delete from the database, to dont click again. 
One to receive the email
Another route to have acess