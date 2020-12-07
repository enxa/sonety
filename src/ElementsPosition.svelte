<script>
  // window
  const { innerWidth, innerHeight, outerWidth, outerHeight } = window
  const { scrollX, scrollY, pageXOffset, pageYOffset } = window
  const { screenX, screenY, screenTop, screenLeft } = window

  // document
  const { offsetWidth, offsetHeight, offsetTop, offsetLeft } = document.body || document.documentElement
  const { clientWidth, clientHeight, clientTop, clientLeft } = document.body || document.documentElement
  const { scrollWidth, scrollHeight, scrollTop, scrollLeft } = document.body || document.documentElement

  // node
  const { x, y, width, height } = node.getBoundingClientRect()
  const { top, right, bottom, left } = node.getBoundingClientRect()

  // event
  const { screenX, screenY, clientX, clientY } = event
  const { pageX, pageY, x, y } = event
  const { offsetX, offsetY, movementX, movementY } = event
</script>