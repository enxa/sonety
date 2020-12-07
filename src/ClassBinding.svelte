<script>
  let inview = false
</script>

<style>
  h1 {
    display: inline-block;
    position: relative;
    overflow: hidden;
    color: transparent;
  }
  h1::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #000;
    transform: translateX(-101%);
  }
  .inview {
    animation: animateReveal 1s forwards;
  }
  .inview::after {
    animation: animateRevealAfter 1s forwards;
  }
  
  @keyframes animateReveal {
    0% { color: transparent }
    50% { color: transparent }
    51% { color: #000 }
    100% { color: #000 }
  }
  
  @keyframes animateRevealAfter {
    0% { transform: translateX(-101%) }
    50% { transform: translateX(0%) }
    51% { transform: translateX(1%) }
    100% { transform: translateX(101%) }
  }
</style>

<input type=checkbox bind:checked={inview}>

<h1 class:inview>Some random text</h1>