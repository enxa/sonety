<script>
  window.addEventListener('DOMContentLoaded', () => document.body.innerHTML += loader)
  window.addEventListener('load', () => document.querySelector('#loader').style.display = "none")     
  const loader = `<section id="loader"> <svg width="100%" height="100%" viewBox="0 0 100 100"></svg>`
</script>