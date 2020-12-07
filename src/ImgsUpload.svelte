<script>
  let files
  let preview
  let data
  let images
  let fallback
  
  let readImgAsBase64 = () => {
    if (files) {
      if (files[0].size > 10000000) return fallback = 'File size exceeds 10MB'
      const reader = new FileReader()
      reader.readAsDataURL(files[0])
      reader.addEventListener("load", () => preview.style.backgroundImage = `url('${reader.result}')`, false)
    }
  }
  
  let postData = async () => {
    const headers = {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(files[0])
    }
    data = await fetch('/api', headers)
    images = await data.json()
  }

  let getData = async () => {
    data = await fetch('/api')
    images = await data.json()
  }

  // api
  app.post('/api', (req, res) => {
    let data = req.body
    data.timestamp = Date.now()
    db.insert(data)
    res.json(data)
  })
  
  app.get('/api', (req, res) => {
    db.find({}).sort({ _id: -1 }).exec((err, data) => res.json(data))
  })
</script>

<input type="file" bind:files on:change="{readImgAsBase64}">
<div bind:this="{preview}" style="height: 10vh; background: no-repeat left center / contain"></div>
<input type="submit" value="Submit" on:click="{postData}">