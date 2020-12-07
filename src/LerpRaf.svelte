<script>
  const lerp = (position, targetPosition) => {
    position.x += (targetPosition.x - position.x) * 0.1
    position.y += (targetPosition.y - position.y) * 0.1
    requestAnimationFrame(lerp)
  }
  lerp()
</script>