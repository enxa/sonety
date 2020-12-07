<script>
  let mousePosition = { x: 0, y: 0 }

  let handleMousePosition = e => {
    mousePosition.x = e.x
    mousePosition.y = e.y
  }
</script>

<style>
  section { width: 100%; height: 100%; }
</style>

<section on:mousemove="{handleMousePosition}"></section>