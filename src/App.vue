<template>
  <div id="app">
    <div class="head">
      <h1><u><strong>Number Blocks Game</strong></u></h1>
      <h2>Hi Satori!</h2>
      <h3>Daddy made this for you! ❤️</h3>
    </div>
    <br>
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
        <img v-if="result >= 0 && result <= 100" src="@/assets/One.jpg" alt="result-image">
      </div>
      <div v-if="result !== null" class="visual-aid">
        <div v-if="result > 100">
          <span>That's a really big number!</span>
          <img src="@/assets/bluey.jpeg" alt="big-number-image">
        </div>
        <div v-else-if="result < 0">
          <span>Let's practice easier math right now!</span>
          <img :src="negativeResultImage" alt="negative-result-image">
        </div>
        <div v-else-if="result > 0" class="blocks">
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
      negativeResultImage: require('@/assets/four.jpg') // Correctly require image path
    };
  },
  methods: {
    appendNumber(number) {
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
      if (this.equation !== '' && !this.equation.match(/[+-]$/)) {
        this.equation += operator;
      }
    },
    calculate() {
      if (this.equation.includes('+') || this.equation.includes('-')) {
        try {
          const calculatedResult = eval(this.equation);
          if (calculatedResult < 0) {
            this.result = "Let's practice easier math right now!";
            this.negativeResultImage = require('@/assets/four.jpg'); // Correctly require image path
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
html, body {
  height: 100%;
  margin: 0;
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-image: url('@/assets/background.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
  min-height: 100vh;
}

.head {
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.5);
}

.calculator {
  display: inline-block;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.8);
  max-width: 80%;
}

.display {
  margin-bottom: 20px;
  font-size: 28px;
  min-height: 40px;
  background-color: #fff;
  padding: 10px;
  border-radius: 5px;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(3, 80px);
  gap: 20px;
}

.buttons button {
  font-size: 38px;
  padding: 10px;
  border-radius: 5px;
}

.result-row {
  margin-top: 20px;
  display: flex;
  align-items: center;
  font-size: 28px;
  background-color: rgba(255, 255, 255, 0.8);
  padding: 10px;
  border-radius: 5px;
}

.result-row img {
  width: 80px;
  height: 80px;
  margin-left: 10px;
}

.visual-aid {
  margin-top: 20px;
  font-size: 28px;
}

.visual-aid img {
  margin-top: 10px;
  width: 200px;
  height: 200px;
}

.blocks {
  display: grid;
  grid-template-columns: repeat(auto-fill, 40px);
  gap: 5px;
}

.block {
  width: 40px;
  height: 40px;
  background-color: #4caf50;
}
</style>
