<script>
export default {
  props: {
    buttonData: Object,
  },
  emits: ["buttonClicked"],
  methods: {
    handleClick() {
      this.$emit("buttonClicked", this.buttonData);
    }
  }
}
</script>
<template>
  <button
    :class="buttonData.className"
    @click="handleClick"
  >
    {{ buttonData.text }}
  </button>
</template>

<style scoped>
  button {
    border: none;
    width: 100%;
    background-color: rgb(80, 60, 209);
    font-size: 24px;
    color: rgb(255, 255, 255);
    font-weight: bold;
    cursor: pointer;
    border-radius: 4px;
    outline: none;
    height: 50px;
    font-family: 'Enriqueta', serif;
  }  
  .ac {
  grid-area: ac;
  }

  .c {
  grid-area: c;
  }

  .c {
  grid-area: c;
  }

  .mc {
  grid-area: mc;
  }

  .ms {
  grid-area: ms;
  }

  .mr {
  grid-area: mr;
  }

  .m-plus {
  grid-area: m-plus
  }

  .m-minus {
  grid-area: m-minus;
  }

  .sign {
  grid-area: sign;
  }

  .percent {
  grid-area: percent;
  }

  .square-root {
  grid-area: square-root;
  }

  .divide {
  grid-area: divide;
  }

  .multiply {
  grid-area: multiply;
  }

  .add {
  grid-area: add;
  }

  .subtract {
  grid-area: subtract;
  }

  .equal {
  grid-area: equal;
  }

  .decimal {
  grid-area: decimal;
  }

  .zero {
  grid-area: zero;
  }

  .one {
  grid-area: one;
  }

  .two {
  grid-area: two;
  }

  .three {
  grid-area: three;
  }

  .four {
  grid-area: four;
  }

  .five {
  grid-area: five;
  }

  .six {
  grid-area: six;
  }

  .seven {
  grid-area: seven;
  }

  .eight {
  grid-area: eight;
  }

  .nine {
  grid-area: nine;
  }

</style>
