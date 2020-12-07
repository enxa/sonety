<script>
  let radioOption = 0;
  let checkboxOption = [];

  let options = [
    'option1',
    'option2',
    'option3'
  ]
</script>

<style>
  label:before {
    content: "";
    display: inline-block;
    width: 20px;
    height: 20px;
    margin: 5px 10px 5px 0px;
    border: 1px solid #000;
    transform: translateY(50%);
    cursor: pointer;
    transition: 1s all
  }
  input {
    display: none
  }
  input:checked + label:before {
    background: #000
  }
</style>

{#each options as option, i}
  <input id="checkboxOption{i}" type=checkbox value={option} bind:group={checkboxOption}>
  <label for="checkboxOption{i}">{option}</label>
{/each}

{#each options as option, i}
  <input id="radioOption{i}" type=radio value={option} bind:group={radioOption}>
  <label for="radioOption{i}">{option}</label>
{/each}   