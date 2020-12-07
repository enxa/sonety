<script>
  let scrollable = document.querySelector('[data-scroll]')
  
  let winSize = 0
  let docSize = 0
  let docScroll = 0
  
  let previousPosition = 0
  let currentPosition = 0
  let easing = 0.1
  
  let calcWinSize = () => winSize = { width: window.innerWidth, height: window.innerHeight }
  
  let calcDocSize = () => docSize = document.body.style.height = `${scrollable.scrollHeight}px`
  
  let calcDocScroll = () => docScroll = window.pageYOffset || document.documentElement.scrollTop
  
  let easeScroll = () => {
    const lerp = (a, b, n) => (1 - n) * a + n * b
    
    currentPosition = docScroll
    previousPosition = lerp(previousPosition, currentPosition, easing)
    scrollable.style.transform = `translateY(${-1*previousPosition}px)`
    scrollable.style.position = 'fixed'

    requestAnimationFrame(() => easeScroll())
  }

  window.addEventListener('load', () => {
    calcWinSize(),
    calcDocSize(),
    calcDocScroll()
  })
  window.addEventListener('resize', () => {
    calcWinSize(),
    calcDocSize()
  })
  window.addEventListener('scroll', () => {
    calcDocScroll()
  })

  easeScroll()
</script>