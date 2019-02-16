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

```

```
```


