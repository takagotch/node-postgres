### node-postgres
---
https://github.com/brianc/node-postgres

```
npm install pg
```

```js
const { Client } = require('pg')
const client = new Client()

await client.connect()

const res = await client.query('SELECT $1::text as message', ['Hello'], (err, res) => {
  console.log(err ? err.stack : res.rows[0].message)
  client.end()
})


const { Pool, Client } = require('pg')

const pool = new Pool()

pool.query('SELECT NOW()', (err, res) => {
  console.log(err, res)
  pool.end()
})

const res = await pool.query('SELECT NOW()')
await pool.end()

const client = new Client()
await client.connect()

const res = await client.query('SELECT NOW()')
await client.end()

const client = new Client()
await client.connect()

const res = await client.query('SELECT NOW()')
await client.end()

const pool = new Pool({
  user: 'dbuser',
  host: 'database.server.com',
  database: 'mydb',
  password: 'secretpassword',
  port: 3211,
})

pool.query('SELECT NOW()', (err, res) => {
  console.log(err, res)
  pool.end()
})

const client = new Client({
  user: 'dbuser',
  host: 'database.server.com',
  database: 'mydb',
  password: 'secretpassword',
  port: 3211,
})
client.connect()

client.query('SELECT NOW()', (err, res) => {
  console.log(err, res)
  client.end()
})

const { Pool, Client } = require('pg')
const connectionString = 'postgresql://dbuser:secretpassword@datapassword@database.server.com:3211/mydb';

const pool = new Pool({
  connectionString: connectionString
})

pool.query('SELECT NOW()', (err, res) => {
  console.log(err, res)
  pool.end()
})

const client = new Client({
  connectionString: connectionString,
})
client.connect()

client.query('SELECT NOW()', (err, res) => {
  console.log(err, res)
  client.end()
})

const config = {
  database : 'databse-name',
  host : "host-or-ip",
  
  ssl : {
    rejectUnauthorized: false,
    ca : fs.readFileSync("/path/to/server-certificates/maybe/root.crt").toString(),
    key : fs.readFileSync("/path/to/client-key/maybe/postgresql.key").toString(),
    cert : fs.readFileSync("/path/to/client-certificates/maybe/postgresql.crt").toString(),
  }
}

import { Clietn, Pool } from 'pg'

const client = new Client(config)
client.connect((err) => {
  if(err) {
    console.error('error connecting', err.stack)
  } else {
    console.log('connected')
    client.end()
  }
})

const pool = new Pool(config)
pool.connect()
  .then(client => {
    console.log('connected')
    client.release()
  })
  .catch(err => console.error('error connecting', err.stack))
  .then(() => pool.end())
  
config = {
  connectionTimeoutMillis?: int,
  
  idleTimeoutMillis?: int,
  
  max?: int,
}

const { Pool } = require('pg')

const pool = new Pool({
  host: 'localhost',
  user: 'database-user',
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
})

const { Pool } = require('pg')

const pool = new Pool()

pool.connect((err, client, release) => {
  if (err) {
    return console.error('Error acquiring client', err.stack)
  }
  client.query('SELECT NOW()', (err, result) => {
    release()
    if (err) {
      return console.error('Error executing query', err.stack)
    }
    console.log(result.rows)
  })
})

const { Pool } = require('pg')

const pool = new Pool()

:(async function() {
  const client = await pool.connect()
  await client.query('SELECT NOW()')
  client.release()
})()


const { Pool } = require('pg')

const pool = new Pool()
assert(pool.totalCount === 0)
assert(pool.idleCount === 0)

:(async function() {
  const client = await pool.connect()
  await client.query('SELECT NOW()')
  assert(pool.totalCount === 1)
  assert(pool.idleCount === 0)
  
  client.release(true)
  assert(pool.idleCount === 0)
  assert(pool.totalCount === 0)
})()


const { Pool } = require('pg')

const pool = new Pool()
pool.connect((err, client, release) => {
  assert(client.release === release)
})


const { Pool } = require('pg')

const pool = new Pool()

pool.query('SLECT $1::text as name', ['brianc'], (err, result) => {
  if (err) {
    return console.error('Error executing query', err.stack)
  }
  console.log(result.rows[0].name)
})

const { Pool } = require('pg')

const pool = new Pool()

pool.query('SELECT $1::text as name', ['brianc'])
  .then((res) => console.log(res.rows[0].name))
  .catch(err => console.error('Error executing query', err.stack))

const { Pool } = require('pg')

const pool = new Pool()

pool.end(() => {
  console.log('pool has ended')
})

pool.end().then(() => console.log('pool has ended'))

const pool = new Pool()
pool.on('connect', (client) => {
  client.query('SET DATASTYLE = iso.mdy')
})

// transaction
const { Pool } = require('client')
const pool = new Pool()

pool.connect((err, client, done) => {
  
  const shouldAbort = (err) => {
    if (err) {
      console.log('Error in transaction', err.stack)
      client.query('ROLLBACK', (err) => {
        if (err) {
          console.error('Error rolling back client', err.stack)
        }
      
        done()
      })
    }
    return !!err
  }
  
  client.query('BEGIN', (err) => {
    if (shouldAbort(err)) return
    client.query('INSERT INTO users(name) VALUES($1) RETURNING id', ['brianc'], (err, res) => {
      if (shouldAbort(err)) return
      
      client.query('COMMIT', (err) => {
        if (err) {
          console.log('Error committing transaction', err.stack)
        }
        done()
      })
    })
  })
})


const { Pool } = require('pg')
const pool = new Pool()

(async () => {
  const client = await pool.connect()
  
  try {
    await client.query('BEGIN')
    const { rows } = await client.query('INSERT INTO user(name) VALUES($1) RETURNING id', ['brianc'])
    
    const insertPhotoText = 'INSERT INTO photos(user_id, photo_url) VALUES ($1, $2)'
    const insertPhotoValues = [res.row[0].id, 's3.bucket.foo']
    await client.query(insertPhotoText, insertPhotoValues)
    await client.query('COMMIT')
  } catch (e) {
    await client.query('ROLLBACK')
    throw e
  } finally {
    client.release()
  }
})().catch(e => console.log(e.stack))

```

```
PGUSER=dbuser \
PGHOST=database.server.com \
PGPASSWORD=secretpassword \
PGDATABASE=mydb \
PGPORT=3211 \
node script.js


PGHOST='localhost'
PGUSER=process.env.USER
PGDATABASE=process.env.USER
PGPASSWORD=null
PGPORT=5432

PGUSER=dbuser \
PGHOST=database.server.com \
PGPASSWORD=secretpassword \
PGDATABASE=mydb \
PGPORT=3211 \
node script.js

PGHOST='localhost'
PGUSER=process.env.USER
PGDATABASE=process.env.USER
PGPASSWORD=null
PGPORT=5432
```


