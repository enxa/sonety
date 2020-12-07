<script>
  //// authentication
  const express = require('express')
  const router = express.Router()

  const jwt = require('jsonwebtoken')
  const bcrypt = require('bcryptjs')

  const asyncCatch = require('../utils/asyncCatch');
  const authorization = require('../auth/authorization')
  const User = require('./userModel')

  // sign-up - post new user
  router.route('/api/auth').post(asyncCatch(async (req, res, next) => {
    // check if credentials provided
    if (!req.body.name || !req.body.email || !req.body.password) return res.json({ message: 'Nie podano loginu lub emaila lub hasła', auth: false })
    // check if user exist
    const user = await User.findOne({ email: req.body.email }).select('+password')
    if (user) return res.json({ message: 'Email zarezerwowany', auth: false })
    // hash password and store new user
    const hashed = await bcrypt.hash(req.body.password, 10)
    if (hashed) {
      User.create({
        name: req.body.name,
        email: req.body.email,
        password: hashed
      })
      res.json({ message: 'Poprawnie zarejestrowano', auth: true })
    }
  }))

  // sign-in - get token
  router.route('/api/auth').get(asyncCatch(async (req, res, next) => {
    // check if credentials provided
    if (!req.body.name || !req.body.email || !req.body.password) return res.json({ message: 'Nie podano loginu lub emaila lub hasła', auth: false })
    // check if user exist
    const user = await User.findOne({ email: req.body.email }).select('+password')
    if (!user) return res.json({ message: 'Niepoprawny email', auth: false })
    // compare password
    const compared = await bcrypt.compare(req.body.password, user.password)
    if (!compared) return res.json({ message: 'Niepoprawne hasło', auth: false })
    // if everything is ok send token
    const token = jwt.sign({ _id: user._id, email: user.email }, process.env.JWT_SECRET, { expiresIn: process.env.JWT_EXPIRES_IN })
    res
      .cookie('jwt', token, {
        expires: new Date(Date.now() + 60 * 24 * 60 * 60 * 1000),     // 60 days
        secure: true,
        httpOnly: true
      })
      .header('Authorization', `Bearer ${token}`)
      .header('Set-Cookie', `loggedIn=${token}`)
      .json({ message: 'Poprawnie zalogowano', auth: true, token: token })
  }))

  // me route
  router.route('/api/auth').put(authorization, asyncCatch(async (req, res, next) => {
    // check if user exist
    const user = await User.findById(req.user._id)
    if (!user) return res.json({ message: 'Nie ma takiego użytkownika', auth: false })
    // send user data
    res.json({ user: user, auth: true })
  }))

  module.exports = router

  //// authorization
  const jwt = require('jsonwebtoken')
  const User = require('./userModel')

  module.exports = async (req, res, next) => {
    try {
      // check if bearer has been set
      const bearer = await req.headers.authorization && req.headers.authorization.startsWith('Bearer')
      if (!bearer) return res.json({ message: 'Brak autoryzacji', auth: false })
      // check token
      const token = await req.headers.authorization.split(" ")[1]     // || req.body.token || req.query.token => Bearer
      if (!token) return res.json({ message: 'Brak tokenu', auth: false })
      // verify token
      const decoded = await jwt.verify(token, process.env.JWT_SECRET)
      if (!decoded) return res.json({ message: 'Błąd tokenu', auth: false })
      // set new field for request
      req.user = decoded
      if (req.user.exp < parseInt((Date.now()/1000))) return res.json({ message: 'Token wygasł, zaloguj się ponownie', auth: false })
      // check if user still exist
      const user = await User.findById(req.user._id)
      if (!user) return res.json({ message: 'Użytkownik z tym tokenem już nie istnieje', auth: false })

      next()
    }
    catch (error) {
      return res.json({ message: 'Błąd autoryzacji', auth: false })
    }
  }

  //// model
  const mongoose = require('mongoose')

  module.exports = mongoose.model('User', new mongoose.Schema({  
    name: {
      type: 'String',
      trim: true
    },
    email: {
      type: 'String',
      required: true,
      trim: true,
      unique: true,
      match: /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/
    },
    password: {
      type: 'String',
      required: true,
      trim: true,
      minlength: 3,
      select: false
    }
  }))
</script>