<script>
  // store
  import { writable } from "svelte/store"

  export const elements = writable([
    { "symbol": "Zn" },
    { "symbol": "He" },
    { "symbol": "Li" }
  ])

  export const elementsStore = {
    subscribe:
      elements.subscribe,
    addElement: 
      newElement => elements.update(el => el = [...el, newElement]),
    removeElement: 
      elementToRemove => elements.update(el => el = el.filter(e => e.symbol != elementToRemove))
  }

  // component
  import { elementsStore } from '../store.js'

  let addElement = e => elementsStore.addElement(newElement)
  let removeElement = e => elementsStore.removeElement(e.target.innerText)
</script>

<button on:click="{addElement}">Add</button>
<button on:click="{removeElement}">Remove</button>

{#each $elements as element}
  {element}
{/each}