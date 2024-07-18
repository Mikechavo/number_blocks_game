<template>
  <div id="app">
    <h1>Number Blocks Game</h1>
    <h2>Hi Satori!</h2>
    <h3>Daddy made this for you! ❤️</h3>
    <div class="calculator">
      <div class="display">{{ equation }}</div>
      <div class="buttons">
        <button @click="appendNumber(num)" v-for="num in numbers" :key="num">{{ num }}</button>
        <button @click="appendOperator(op)" v-for="op in operators" :key="op">{{ op }}</button>
        <button @click="calculate">=</button>
        <button @click="clear">C</button>
      </div>
      <div v-if="result !== null" class="result-row">
        <span>{{ equation }} = {{ result }}</span>
        <img src="./assets/One.jpg" alt="result-image">
      </div>
      <div v-if="result !== null" class="visual-aid">
        <div v-if="result > 100">
          <span>That's a really big number!</span>
        </div>
        <div v-else-if="result < 0">
          <span>Let's practice easier math right now!</span>
        </div>
        <div v-else class="blocks">
          <div v-for="n in result" :key="n" class="block"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      equation: '',
      result: null,
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0],
      operators: ['+', '-'],
    };
  },
  methods: {
    appendNumber(number) {
      // Ensure numbers are appended correctly
      if (this.equation === '' || this.equation.match(/[+-]$/)) {
        this.equation += number;
      } else {
        const lastNumber = this.equation.split(/[+-]/).pop();
        if (lastNumber.length < 3) {
          this.equation += number;
        }
      }
    },
    appendOperator(operator) {
      // Ensure operator is appended correctly
      if (this.equation !== '' && !this.equation.match(/[+-]$/)) {
        this.equation += operator;
      }
    },
    calculate() {
      // Ensure calculation and handle negative results
      if (this.equation.includes('+') || this.equation.includes('-')) {
        try {
          const calculatedResult = eval(this.equation);
          if (calculatedResult < 0) {
            this.result = "Let's practice easier math right now!";
          } else {
            this.result = calculatedResult;
          }
        } catch (error) {
          alert("There was an error calculating the result.");
        }
      }
    },
    clear() {
      this.equation = '';
      this.result = null;
    }
  }
};
</script>

<style>
#app {
  text-align: center;
}
.calculator {
  display: inline-block;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
}
.display {
  margin-bottom: 20px;
  font-size: 24px;
  min-height: 40px;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(3, 60px);
  gap: 10px;
}
.buttons button {
  font-size: 18px;
  padding: 10px;
}
.result-row {
  margin-top: 20px;
  display: flex;
  align-items: center;
}
.result-row img {
  width: 40px;
  height: 40px;
  margin-left: 10px;
}
.visual-aid {
  margin-top: 20px;
}
.blocks {
  display: grid;
  grid-template-columns: repeat(auto-fill, 20px);
  gap: 5px;
}
.block {
  width: 20px;
  height: 20px;
  background-color: #4caf50;
}
</style>
