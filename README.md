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
```


