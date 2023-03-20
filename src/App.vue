<script>
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import Button from './components/Button.vue'
import Display from './components/Display.vue'
import { calculatorButtons } from './data/calculator-bonus-03-button-data.js'

export default {
  components: {
    Header,
    Footer,
    Button,
    Display,
  },
  data() {
    return {
      calculatorButtons,
      memory: 0,
      operandA: "0",
      operator: "",
      operandB: "",
      MAX_DISPLAY_LENGTH: 12, // not including -, exp
    }
  },
  computed: {
    displayString() {
      let displayStr;
      if (this.operandB) {
        displayStr = this.operandB;
      } else if (this.operator) {
        return this.operator;
      } else {
        displayStr = this.operandA;
      }
      // use sci notation if display won't fit
      if (displayStr.length > this.MAX_DISPLAY_LENGTH) {
        const value = parseFloat(displayStr);
        displayStr = value.toExponential(this.MAX_DISPLAY_LENGTH - 1);
      } 
      return displayStr;
    }
  },
  methods: {
    setOperandA(value) {
      this.operandA = value;
    },
    setOperator(value) {
      this.operator = value;
    },
    setOperandB(value) {
      this.operandB = value;
    },
    setMemory(value) {
      this.memory = value;
    },
    updateOperandA(value) {
      value = value.toString();
      if (value !== "." || !this.operandA.includes(".")) {
        (this.operandA === "0" && value !== ".") ? this.setOperandA(value) :
          this.setOperandA(this.operandA + value);
      }
    },
    updateOperandB(value) {
      value = value.toString();
      if (value !== "." || !this.operandB.includes(".")) {
        this.setOperandB(this.operandB + value);
      }
    },
    updateOperator(value) {
      if (!this.operator) {
        this.setOperator(value);
      }
    },
    toggleSignOperandA() {
      if (this.operandA !== "0") {
        this.operandA.startsWith("-") ? this.setOperandA(this.operandA.slice(1)) :
          this.setOperandA("-" + this.operandA);
      }
    },
    toggleSignOperandB() {
      if (this.operandB && this.operandB !== "0") {
        this.operandB.startsWith("-") ? this.setOperandB(this.operandB.slice(1)) :
          this.setOperandB("-" + this.operandB);
      }
    },
    percent(value) {
      const result = parseFloat(value) / 100.0;
      return result.toString();
    },
    squareRoot(value) {
      const result = Math.sqrt(parseFloat(value));
      if(isNaN(result)) {
        throw Error("Error");
      }
      return result.toString();
    },
    evaluateExpression() {
      const a = parseFloat(this.operandA);
      const b = this.operandB ? parseFloat(this.operandB) : 0.0;
      let result;
      switch (this.operator) {
        case "+":
          result = a + b;
          break;
        case "-":
          result = a - b;
          break;
        case "*":
          result = a * b;
          break;
        case "/":
          result = a / b;
          break;
        default: // no operator
          result = a;
          break;
      }
      // throw error for div by 0 or sqrt of negative
      if (isNaN(result) || !isFinite(result)) {
        throw Error("Error");
      }
      return result.toString();
    },
    resetDisplay() {
      this.setOperandA("0");
      this.setOperator("");
      this.setOperandB("");
    },
    handleClearButtonClicked(value) {
      // all clear
      (value === "All Clear") ? this.resetDisplay() :
      // clear
        this.operandB ? this.setOperandB("") :
          this.operator ? this.setOperator("") :
            this.setOperandA("0");
    },
    handleMemoryButtonClicked(value) {
      const activeOperandFlt = this.operandB ? parseFloat(this.operandB) : 
        parseFloat(this.operandA);
      switch (value) {
        case "Memory Save":
          this.setMemory(activeOperandFlt);
          break;
        case "Memory Clear":
          this.setMemory(0);
          break;
        case "Memory Recall":
          this.operandB ? this.setOperandB(this.memory.toString()) :
            this.setOperandA(this.memory.toString());
          break;
        case "Memory Addition":
          this.memory += activeOperandFlt;
          break;
        case "Memory Subtraction":
          this.memory -= activeOperandFlt
          break;
        default:
          break;
      }
    },
    handleButtonClicked(buttonData) {
      switch(buttonData.type) {
        case "number":
          this.operator ? this.updateOperandB(buttonData.value) :
            this.updateOperandA(buttonData.value);
          break;
        case "operator":
          this.updateOperator(buttonData.value);
          break;
        case "enter":
          try {
            const result = this.evaluateExpression();
            this.resetDisplay();
            this.setOperandA(result);
          } catch (error) {
            this.resetDisplay();
            this.setOperandA(error.message);
          }
          break;
        case "clear":
          this.handleClearButtonClicked(buttonData.value);
          break;
        case "memory":
          this.handleMemoryButtonClicked(buttonData.value);
          break;
        case "sign":
          this.operator ? this.toggleSignOperandB() :
            this.toggleSignOperandA();
          break;
        case "percent":
          this.operandB ? this.setOperandB(this.percent(this.operandB)) :
            this.setOperandA(this.percent(this.operandA));
          break;
        case "sqrt":
          try {
            this.operandB ? this.setOperandB(this.squareRoot(this.operandB)) :
              this.setOperandA(this.squareRoot(this.operandA));
          } catch (error) {
            this.resetDisplay();
            this.setOperandA(error.message);
          }
          break;
        default:
          break;
      }
    },
  }
}
</script>

<template>
  <div class="wrapper">
    <Header :title="'Calculator App'"></Header>
    <main>
      <Display :text="displayString"></Display>
      <div class="button-area">
        <Button
          v-for="(button, index) in calculatorButtons"
          :key="index"
          :buttonData="button"
          @buttonClicked="(buttonData) => handleButtonClicked(buttonData)"
        ></Button>
      </div>
    </main>
    <Footer :author="'Adam Rodrigues'"></Footer>
  </div>
</template>

<style scoped>
  .wrapper {
    box-sizing: border-box;
    text-align: center;
    background-color: #282c34;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    color: white;
    margin: 0;
  }

  main {
    padding: 10px;
    width: 340px;
    height: 540px;
    border-radius: 10px;
    border: solid grey 2px;
    background-color: #485461;
    background-image: linear-gradient(315deg, #485461 0%, #28313b 74%);
  }

  .button-area {
    padding: 10px;
    width: 100%;
    height: calc(100% - 120px);
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-areas: 
    "ac       ac        c            mc"
    "ms       mr        m-plus       m-minus"
    "sign     percent   square-root  divide"
    "seven    eight     nine         multiply"
    "four     five      six          subtract"
    "one      two       three        add"
    "decimal  zero      equal        equal";
    grid-gap: 10px;
  }


</style>
