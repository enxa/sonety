<script>
  //// routes
  const express = require('express')
  const router = express.Router()
  const Issue = require('./issueModel')
  const authorization = require('../auth/authorization')
  const asyncCatch = require('../utils/asyncCatch');
  
  router.route('/api/issues').post(authorization, asyncCatch(async (req, res, next) => {
    const result = await Issue.create({
      name: req.body.name,
      author: req.body.author,
      assigned: req.body.assigned
    })
    res.json({ message: "issue created", result: result })
  }))
  
  router.route('/api/issues').get(authorization, asyncCatch(async (req, res, next) => {
    const result = await Issue.find().populate('author assigned', 'name -_id')
    // .sort({ name: 1 })
    // .select({ name: 1, email: 1 })
    res.json({ message: "issues readed", result: result })
  }))
  
  router.route('/api/issues/:id').get(authorization, asyncCatch(async (req, res, next) => {
    const result = await Issue.findById(req.params.id).populate('author assigned', 'name -_id')
    res.json({ message: "issue readed", result: result })
  }))
  
  router.route('/api/issues/:id').put(authorization, asyncCatch(async (req, res, next) => {
    const result = await Issue.findByIdAndUpdate(req.params.id, {
      name: req.body.name,
      author: req.body.author
    }, {new: true})
    res.json({ message: "issue updated", result: result })
  }))
  
  router.route('/api/issues/:id').delete(authorization, asyncCatch(async (req, res, next) => {
    const result = await Issue.findByIdAndRemove(req.params.id)
    res.json({ message: "issue deleted", result: result })
  }))
  
  router.route('/api/issues').delete(authorization, async (req, res, next) => {
    const result = await Issue.deleteMany()
    res.json({ message: "issue deleted", result: result })
  })
  
  module.exports = router

  //// model
  const mongoose = require('mongoose')

  module.exports = mongoose.model('Issue', new mongoose.Schema({  
    name: {
      type: 'String'
    },
    author: { 
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User'
    },
    assigned: [
      { 
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User'
      }
    ]
  }))
</script>