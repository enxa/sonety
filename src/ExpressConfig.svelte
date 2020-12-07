<script>
  const env = require('dotenv').config()
  const path = require('path')
  const mongoose = require('mongoose')
  const express = require('express')
  const helmet = require('helmet')
  const cors = require('cors')
  
  const app = express()
  
  //  db connection
  mongoose.connect(process.env.DB_CONNECTION, { useNewUrlParser: true, useFindAndModify: false, useCreateIndex: true })
  .then(con => console.log(`DB is running at port: ${con.connections[0].port}`))
  .catch(error => console.error(`DB connection error: ${error}`))
  
  // module.exports = app
  app.listen(process.env.PORT || 3000, () => console.log(`Server is listening on port: ${process.env.PORT || 3000}`))
  
  // view engine
  app.set('view engine', 'pug')
  app.set('views', path.join(__dirname, 'views'))
  
  // static files
  app.use(express.static(path.join(__dirname, 'public')))
  app.use('/', express.static(path.join(__dirname, 'public')))
  
  // body parsing
  app.use(express.json())
  app.use(express.urlencoded({ extended: true }))
  
  // headers
  app.use(helmet())
  app.use(cors())
  
  // routes
  app.use(require('./api/auth/authentication'))
  app.use(require('./api/issues/issue'))
  
  // middleware errors handle
  app.all('*', (req, res, next) => res.status(404).json({ error: true, message: 'Not found!' }))          // btw:.sendFile(path.join(__dirname, 'views', '404.pug'))
  app.use((err, req, res, next) => res.status(500).json({ error: true, message: 'Something failed!', err: err }))
  
  // global errors handle
  process.on('uncaughtException', (err, origin) => console.log(`Uncaught exception fuckup: ${err}`))
  process.on('unhandledRejection', (reason, promise) => console.log(`Unhandled rejection fuckup: ${reason}`))

  //.env file:
  DB_CONNECTION=''
  JWT_SECRET='secret'
  JWT_EXPIRES_IN='60d'
</script>