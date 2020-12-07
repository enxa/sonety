<script>
  const toObserve = document.querySelectorAll('.animate');

  observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => 
      entry.isIntersecting === true || entry.intersectionRatio > 0 ? 
      entry.target.classList.add('visible') : 
      entry.target.classList.remove('visible'))
  })

  toObserve.forEach(image => {
      observer.observe(image)
  })
</script>