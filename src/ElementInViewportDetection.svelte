<script>
  // script.js
  export let detectPosition = (event, node) => {
    let windowPosition = {}
    let documentPosition = {}
    let clientPosition = {}
    let eventPosition = {}

    let scrollPercentage = 0
  
    window.addEventListener('scroll', () => {
      // ElementsPosition
      
      if (clientPosition.top + clientPosition.height >= 0 && 
          clientPosition.bottom - clientPosition.height <= windowPosition.innerHeight) {
        node.classList.remove("out-view")
        node.classList.add("in-view")
      } else {
        node.classList.remove("in-view")
        node.classList.add("out-view")
      }

      scrollPercentage = (documentPosition.scrollTop / (documentPosition.scrollHeight - documentPosition.clientHeight)) * 100
    })
    return scrollPercentage
  }

  // usage
  import { detectPosition } from './detectPosition'
</script>

<section use:detectPosition></section>

