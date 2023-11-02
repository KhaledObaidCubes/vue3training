<template>
  <div class="calculator-background">
    <div class="conatiner display-container">
      <p class="last-equation">
        {{ calculationHistory[calculationHistory.length - 1] }}
      </p>
    </div>
    <div class="conatiner display-container">
      <h3 class="display-area">{{ result }}</h3>
    </div>

    <RowBtns
      v-for="row in rows"
      :key="row"
      :buttons="row"
      @action="calculate($event)"
    />
  </div>
</template>

<script setup>
import RowBtns from "./assets/components/calculator-component/row-buttons.vue";

let row0 = [
  {
    value: "C",
    type: "operand",
    disabled: false,
  },
  {
    value: "⇦",
    type: "operand",
    disabled: false,
  },
  {
    value: "%",
    type: "operand",
    disabled: false,
  },
  {
    value: "÷",
    type: "operand",
    disabled: false,
  },
];
let row1 = [
  {
    value: "7",
    type: "num",
    disabled: false,
  },
  {
    value: "8",
    type: "num",
    disabled: false,
  },
  {
    value: "9",
    type: "num",
    disabled: false,
  },
  {
    value: "x",
    type: "operand",
    disabled: false,
  },
];
let row2 = [
  {
    value: "4",
    type: "num",
    disabled: false,
  },
  {
    value: "5",
    type: "num",
    disabled: false,
  },
  {
    value: "6",
    type: "num",
    disabled: false,
  },
  {
    value: "-",
    type: "operand",
    disabled: false,
  },
];
let row3 = [
  {
    value: "1",
    type: "num",
    disabled: false,
  },
  {
    value: "2",
    type: "num",
    disabled: false,
  },
  {
    value: "3",
    type: "num",
    disabled: false,
  },
  {
    value: "+",
    type: "operand",
    disabled: false,
  },
];
let row4 = [
  {
    value: "±",
    type: "num",
    disabled: true,
  },
  {
    value: "0",
    type: "num",
    disabled: false,
  },
  {
    value: ".",
    type: "num",
    disabled: true,
  },
  {
    value: "=",
    type: "equal-btn operand",
    disabled: false,
  },
];

//arrya of calculator rows of buttons
let rows = [row0, row1, row2, row3, row4];

//this function will be called on every button clciked as a start point
//and it will return the buttons type wither it is number(true) or an operand(false)
//since the 'parseInt' method for the num 0 or the string "0" will allways return false
//it was handled once it was entered as an input
const isInputANum = (input) => (input == "0" ? true : !!parseInt(input));

//in this functionif operand exist it will return true(add the input to the right side),
//otherwise false(Add the input to left side)
const getRightSide = (operand) => !!operand.length;

//this function will return (true) if the right-hand side of the equation filled (need to calculate the result and empty the right-hand side)
//else return (false) if its empty and continue add input to the right-hand side
const isRHSFilled = (RHS) => !!String(RHS).length;

//this finction will return the result (calculation result based on the operand)
const getResult = (LHS, RHS, OPRND) => {
  switch (OPRND) {
    case "+":
      return parseInt(LHS) + parseInt(RHS);
    case "-":
      return parseInt(LHS) - parseInt(RHS);
    case "÷":
      return parseInt(LHS) / parseInt(RHS);
    case "x":
      return parseInt(LHS) * parseInt(RHS);
    case "%":
      return parseInt(LHS) % parseInt(RHS);
    case "C":
      return "reset";
    case "=":
      return parseInt(LHS);
    default:
      return "INVALID OPERATION";
  }
};

//this function will remove the "0" from where its prefexed on the left-hand side
const leftZeroSlicer = (LHS, INPT) => {
  if (LHS == "0") LHS = LHS.slice(1);
  return LHS + INPT;
};

//this function will do the back space functionallity will delete the last input from the entier equation
const backspace = (LHS, RHS, OPRND) => {
  if (String(RHS).length) {
    RHS = String(RHS).slice(0, -1);
  } else if (OPRND != "") {
    OPRND = "";
  } else if (String(LHS).length == 1) {
    //String(LHS).slice(0, -1) == ""
    // ? (LHS = "0")
    // : (LHS = String(LHS).slice(0, -1));
    LHS = "0";
  } else {
    LHS = String(LHS).slice(0, -1);
  }
  return [LHS, RHS, OPRND];
};

//this function will log all equations history that was stored
const historyLog = (logs) => {
  var msg = "";

  for (let itm in logs.value) {
    msg = msg.concat(logs.value[itm] + "//");
  }
  console.log(msg);
};

import { ref } from "vue";
const calculationHistory = ref(["0"]);
const result = ref("0");
let leftHandSide = "0";
let rightHandSide = "";
let operand = "";

const calculate = (input) => {
  //the first two if statements are handle the special C and ⇦ chars
  if (input == "C") {
    //reset all variables and wait for the new input( C special charachter)
    leftHandSide = "0";
    rightHandSide = "";
    operand = "";
    result.value = "0";
  } else if (input == "⇦") {
    //remove last input from the equation (⇦ special charachter)
    [leftHandSide, rightHandSide, operand] = backspace(
      leftHandSide,
      rightHandSide,
      operand
    );
  } else if (isInputANum(input)) {
    //chose the side where need to concat the input
    getRightSide(operand)
      ? (rightHandSide = rightHandSide + input)
      : (leftHandSide = leftZeroSlicer(leftHandSide, input));
  } else {
    //if all equation sides are completeed calculate and get the result
    //store equation in history and then empty the right hand side
    if (isRHSFilled(rightHandSide)) {
      calculationHistory.value.push(
        `${leftHandSide} ${operand} ${rightHandSide}`
      );
      leftHandSide = getResult(leftHandSide, rightHandSide, operand);
      rightHandSide = "";
    }
    //if the operand was = don't assigne it to operand variable
    input != "=" ? (operand = input) : (operand = "");
  }
  //if the operand was = update the display value
  operand == "="
    ? (result.value = `${leftHandSide}`)
    : (result.value = `${leftHandSide} ${operand} ${rightHandSide}`);
  historyLog(calculationHistory);
};
</script>

<style scoped>
.calculator-background {
  background-color: #1a1d24;
  height: 450px;
  width: 240px;
  border: none;
  border-radius: 20px;
  display: flexbox;
}
.display-area {
  width: 280px;
  text-align: right;
  color: rgb(239, 233, 233);
  margin: 30px 10px 0 0;
  padding: 0 0 10px 0;
  font-size: 34px;
}
.display-container {
  font-family: Arial, Helvetica, sans-serif;
  padding: 30px 0 0 0;
  display: flex;
}
.conatiner {
  width: 100%;
}
.last-equation {
  width: 100%;
  color: rgb(239, 233, 233);
  text-align: right;
  height: 20px;
  margin-right: 10px;
}
</style>
