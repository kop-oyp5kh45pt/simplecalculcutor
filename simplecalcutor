<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple Calculator</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');
  body {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(135deg, #667eea, #764ba2);
    min-height: 100vh;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
  }
  .calculator {
    background: rgba(255, 255, 255, 0.1);
    width: 320px;
    border-radius: 20px;
    box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    padding: 1.5rem;
  }
  .display {
    background: rgba(255,255,255,0.2);
    border-radius: 15px;
    font-size: 2rem;
    padding: 1rem;
    margin-bottom: 1.2rem;
    text-align: right;
    letter-spacing: 0.1em;
    user-select: none;
    min-height: 50px;
    overflow-x: auto;
  }
  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px;
  }
  button {
    background: rgba(255,255,255,0.25);
    border: none;
    border-radius: 15px;
    padding: 1rem 0;
    font-size: 1.3rem;
    font-weight: 700;
    color: #fff;
    cursor: pointer;
    box-shadow: 0 4px 30px rgba(255,255,255,0.1);
    transition: background 0.3s ease, transform 0.1s ease;
    user-select: none;
  }
  button.operator {
    background: #9a7ad8;
  }
  button.operator:hover,
  button.equal:hover {
    background: #764ba2;
  }
  button.equal {
    grid-column: span 2;
    background: #ff6b6b;
  }
  button.equal:active,
  button.operator:active,
  button:active {
    transform: scale(0.95);
  }
  button.clear {
    background: #e06666;
  }
  button.clear:hover {
    background: #ba4b4b;
  }
  button:focus {
    outline: 2px solid #fff;
  }
  @media (max-width: 400px) {
    .calculator {
      width: 90vw;
      padding: 1rem;
    }
  }
</style>
</head>
<body>
  <div class="calculator" role="application" aria-label="Simple Calculator">
    <div class="display" id="display" aria-live="polite" aria-atomic="true" tabindex="0">0</div>
    <div class="buttons">
      <button class="clear" id="clear" aria-label="Clear">C</button>
      <button aria-label="Divide" class="operator" data-value="/">÷</button>
      <button aria-label="Multiply" class="operator" data-value="*">×</button>
      <button aria-label="Backspace" id="backspace">⌫</button>

      <button aria-label="Seven" data-value="7">7</button>
      <button aria-label="Eight" data-value="8">8</button>
      <button aria-label="Nine" data-value="9">9</button>
      <button aria-label="Subtract" class="operator" data-value="-">−</button>

      <button aria-label="Four" data-value="4">4</button>
      <button aria-label="Five" data-value="5">5</button>
      <button aria-label="Six" data-value="6">6</button>
      <button aria-label="Add" class="operator" data-value="+">+</button>

      <button aria-label="One" data-value="1">1</button>
      <button aria-label="Two" data-value="2">2</button>
      <button aria-label="Three" data-value="3">3</button>
      <button aria-label="Equals" class="equal" id="equals">=</button>

      <button aria-label="Zero" data-value="0" style="grid-column: span 2;">0</button>
      <button aria-label="Decimal point" data-value=".">.</button>
    </div>
  </div>

<script>
  (() => {
    const display = document.getElementById('display');
    const buttons = document.querySelectorAll('.buttons button');
    const clearBtn = document.getElementById('clear');
    const equalsBtn = document.getElementById('equals');
    const backspaceBtn = document.getElementById('backspace');

    let currentInput = '0';

    function updateDisplay() {
      display.textContent = currentInput;
      display.focus();
    }

    function appendToInput(char) {
      if (currentInput === '0' && char !== '.') {
        currentInput = char;
      } else {
        currentInput += char;
      }
    }

    function isOperator(char) {
      return ['+', '-', '*', '/'].includes(char);
    }

    function handleButtonClick(e) {
      const value = e.target.getAttribute('data-value');
      if (!value) return;

      const lastChar = currentInput.slice(-1);

      if (isOperator(value)) {
        // Prevent two operators in a row
        if (currentInput.length === 0) return;
        if (isOperator(lastChar)) {
          currentInput = currentInput.slice(0, -1) + value;
        } else {
          currentInput += value;
        }
      } else if (value === '.') {
        // Prevent multiple dots in the current number segment
        let lastOperatorIndex = -1;
        for (let i = currentInput.length - 1; i >=0; i--) {
          if (isOperator(currentInput[i])) {
            lastOperatorIndex = i;
            break;
          }
        }
        let lastNumberSegment = currentInput.slice(lastOperatorIndex + 1);
        if (!lastNumberSegment.includes('.')) {
          currentInput += '.';
        }
      } else {
        appendToInput(value);
      }
      updateDisplay();
    }

    function clearInput() {
      currentInput = '0';
      updateDisplay();
    }

    function backspace() {
      if (currentInput.length > 1) {
        currentInput = currentInput.slice(0, -1);
      } else {
        currentInput = '0';
      }
      updateDisplay();
    }

    function calculate() {
      try {
        // Evaluate expression safely
        // Disallow invalid characters using regex
        if (/[^0-9+\-*/.() ]/.test(currentInput)) {
          alert('Invalid input');
          return;
        }
        // Eval can be dangerous but here with sanitized input and simple calculator it's acceptable
        let result = Function('"use strict";return (' + currentInput + ')')();
        if (typeof result === 'number' && isFinite(result)) {
          currentInput = String(result);
        } else {
          alert('Calculation error');
          currentInput = '0';
        }
      } catch (e) {
        alert('Invalid expression');
        currentInput = '0';
      }
      updateDisplay();
    }

    buttons.forEach(btn => {
      if (!btn.id) {
        btn.addEventListener('click', handleButtonClick);
      }
    });
    clearBtn.addEventListener('click', clearInput);
    equalsBtn.addEventListener('click', calculate);
    backspaceBtn.addEventListener('click', backspace);

    // Keyboard support
    window.addEventListener('keydown', (e) => {
      if ((e.key >= '0' && e.key <= '9') || ['+', '-', '*', '/', '.'].includes(e.key)) {
        e.preventDefault();
        const fakeEvent = { target: { getAttribute: () => e.key }};
        handleButtonClick(fakeEvent);
      } else if (e.key === 'Enter') {
        e.preventDefault();
        calculate();
      } else if (e.key === 'Backspace') {
        e.preventDefault();
        backspace();
      } else if (e.key === 'Escape') {
        e.preventDefault();
        clearInput();
      }
    });

    updateDisplay();
  })();
</script>
</body>
</html>

