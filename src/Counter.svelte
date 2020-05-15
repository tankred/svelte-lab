<script>
  let counter = 0;  // counter variable
  $: doubled = counter * 2;
  const handleClick = () => { //Function to add 1 to our variable
    counter += 1;
  };
</script>
<style>
  button{
    border:0px;
    background: #3380ef;
    color:white;
  }
</style>
<div class=”counterComponent”>
  <button on:click={handleClick}>plus 1</button>
  <section>My couter is: {counter}</section>
  <p>{counter} doubled is {doubled}</p>
</div>
