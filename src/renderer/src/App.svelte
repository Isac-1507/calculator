<script lang="ts">
  import { onMount } from 'svelte'
  let display_number = ''
  // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  const select = (num) => () => (display_number += num)
  // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  const clear = () => (display_number = '')

  let operand
  let result
  let operator
  let operators = ['+', '-', '*', '/', 'x']

  function handleKeydown(event: KeyboardEvent): void {
    const key = event.key
    if (!isNaN(Number(key))) {
      select(key.toString())()
    } else if (operators.includes(key)) {
      operation(key)
    } else if (key === 'Enter') {
      equals()
    } else if (key === 'Escape') {
      clear()
    }
  }
  function operation(sign): void {
    operand = Number(display_number)
    operator = sign
    display_number = ''
  }

  function equals(): void {
    // eslint-disable-next-line @typescript-eslint/ban-ts-comment
    // @ts-ignore
    display_number = Number(display_number)
    if (operator === '+') {
      result = operand + display_number
    } else if (operator === '-') {
      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      // @ts-ignore
      result = operand - display_number
    } else if (operator === '*' || operator === 'x') {
      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      // @ts-ignore
      result = operand * display_number
    } else if (operator === '/') {
      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      // @ts-ignore
      result = operand / display_number
    }
    display_number = result.toString()
  }

  onMount(() => {
    window.addEventListener('keydown', handleKeydown)
    return (): void => {
      window.removeEventListener('keydown', handleKeydown)
    }
  })
</script>

<body style="background-color: #323232;">
  <div class="container">
    <div class="display">
      {display_number.length < 23 ? display_number : display_number.substring(0, 23)}
    </div>
    <div class="buttons">
      <button on:click={select(7)}>7</button>
      <button on:click={select(8)}>8</button>
      <button on:click={select(9)}>9</button>
      <button class="operator" on:click={() => operation(operators[0])}>+</button>
      <button on:click={select(4)}>4</button>
      <button on:click={select(5)}>5</button>
      <button on:click={select(6)}>6</button>
      <button class="operator" on:click={() => operation(operators[1])}>-</button>
      <button on:click={select(1)}>1</button>
      <button on:click={select(2)}>2</button>
      <button on:click={select(3)}>3</button>
      <button class="operator" on:click={() => operation(operators[2])}>*</button>
      <button on:click={select(0)}>0</button>
      <button on:click={select('.')}>.</button>
      <button on:click={clear} class="clear">C</button>
      <button class="operator" on:click={() => operation(operators[3])}>/</button>
      <button class="equals" on:click={equals}>=</button>
    </div>
  </div>
</body>

<style>
  .container {
    display: inline-block;
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    background: #323232;
  }

  .display {
    font-size: 24px;
    border: 1px solid #ccc;
    height: 24px;
    padding: 10px;
    margin-bottom: 10px;
    width: 300px;
    text-align: right;
    border-radius: 5px;
    font-family: Arial, Helvetica, sans-serif;
    color: #ccc;
  }
  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 75px);
    gap: 5px;
  }
  button {
    padding: 10px;
    font-size: 18px;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
  }
  .operator {
    background: #878787;
    border: 1px solid #000;
  }
  .equals {
    background: #e7a932;
  }
  .clear {
    border: 0px solid #000;
    background: red;
  }
  .operator:hover {
    background: #adadad;
  }
  .clear:hover {
    background: #ffc0cb;
  }
  .equals:hover {
    background: #ffd700;
  }
</style>
