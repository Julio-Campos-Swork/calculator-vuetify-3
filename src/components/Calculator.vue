<script setup>
import { ref } from "@vue/reactivity";

const calcValues = ref([
  { key: "ce", label: "CE", color: "indigo lighten-4" },
  { key: "c", label: "C", color: "indigo lighten-4" },
  {
    key: "back",
    label: "BACK",
    icon: "mdi-backspace",
    color: "indigo lighten-4",
  },
  { key: "/", label: "÷", color: "indigo lighten-4" },
  { key: "7", label: "7", color: "blue" },
  { key: "8", label: "8", color: "blue" },
  { key: "9", label: "9", color: "blue" },
  { key: "*", label: "×", color: "indigo lighten-4" },
  { key: "4", label: "4", color: "blue" },
  { key: "5", label: "5", color: "blue" },
  { key: "6", label: "6", color: "blue" },
  { key: "-", label: "-", color: "indigo lighten-4" },
  { key: "1", label: "1", color: "blue" },
  { key: "2", label: "2", color: "blue" },
  { key: "3", label: "3", color: "blue" },
  { key: "+", label: "+", color: "indigo lighten-4" },
  { key: "±", label: "±", color: "indigo lighten-4" },
  { key: "0", label: "0", color: "blue" },
  { key: ".", label: ".", color: "indigo lighten-4" },
  { key: "=", label: "=", color: "indigo lighten-4" },
]);

const result = ref(0);
const prevResult = ref(0);
const valInput = ref(0);
const flagPlus = ref("");
const flagMinus = ref("");
const flagDivision = ref("");
const flagMultiply = ref("");
const flagEqual = ref("");
const flagPlusMinus = ref("");

const plusOP = () => {
  if (prevResult.value == 0) {
    prevResult.value = parseInt(valInput.value);
    valInput.value = 0;


  } else {
    prevResult.value += parseInt(valInput.value);
    valInput.value = 0;


  }
};
const minusOP = () => {
  if (prevResult.value == 0) {
    prevResult.value = parseInt(valInput.value);
    valInput.value = 0;
    console.log("chain",chainResult.value)


  } else {
    prevResult.value += parseInt(valInput.value);
    valInput.value = 0;

  }
};
const divisionOP = () => {
  if (prevResult.value == 0) {
    prevResult.value = parseInt(valInput.value);
    valInput.value = 0;
  } else {
    prevResult.value /= parseInt(valInput.value);
    valInput.value = 0;
  }
};
const multiplyOP = () => {
  if (prevResult.value == 0) {
    prevResult.value = parseInt(valInput.value);
    valInput.value = 0;
  } else {
    prevResult.value *= parseInt(valInput.value);
    valInput.value = 0;
  }
};
const equalOp = () => {
  if (flagPlus.value == "+") {
    result.value = prevResult.value + parseInt(valInput.value);
    valInput.value = 0;
    prevResult.value = 0;
    flagPlus.value = "";
    console.log("el resultado de la suma es: ", result.value);
  } else if (flagMinus.value == "-") {
    result.value = prevResult.value - parseInt(valInput.value);
    valInput.value = 0;
    prevResult.value = 0;
    flagMinus.value = "";
    console.log("el resultado de la resta es: ", result.value);
  } else if (flagMultiply.value == "*") {
    result.value = prevResult.value * parseInt(valInput.value);
    valInput.value = 0;
    prevResult.value = 0;
    flagMultiply.value = "";
    console.log("el resultado de la multiplicacion es: ", result.value);
  } else if (flagDivision.value == "/") {
    result.value = prevResult.value / parseInt(valInput.value);
    valInput.value = 0;
    prevResult.value = 0;
    flagDivision.value = "";
    console.log("el resultado de la division es: ", result.value);
  }
};

const deleteCalc = () => {
  result.value = 0;
  chainResult.value = 0;
  valInput.value = 0;
  prevResult.value = 0;
  flagPlus.value = "";
  flagMinus.value = "";
  flagDivision.value = "";
  flagMultiply.value = "";
  flagEqual.value = "";
  flagPlusMinus.value = "";
};

const deleteOneCalc = () => {};

const backButton = () => {};
const InputKey = (keyPressed) => {
  switch (keyPressed) {
    case "+":
      flagPlus.value = "+";
      plusOP();
      break;
    case "-":
      flagMinus.value = "-";
      minusOP();
      break;
    case "*":
      flagMultiply.value = "*";
      multiplyOP();
      break;
    case "/":
      flagDivision.value = "/";
      divisionOP();
      break;
    case "back":
      backButton();
      break;
    case "ce":
      deleteOneCalc();
      break;
    case "c":
      deleteCalc();
      break;
    case "=":
      equalOp();
      break;
    default:
      console.log(keyPressed);
      valInput.value += keyPressed;
      break;
  }
};
</script>
<template>
  <v-card elevation="24" hover width="250" height="400">
    <v-card-title> Calculator</v-card-title>

    <v-card-text>
      <v-container grid-list-xs pa-1>
        <v-text-field :model-value="result" label="Escribe valores"> </v-text-field>
        <v-row justify="start" no-gutters>
          <v-col cols="3" v-for="data in calcValues" :key="data.key">
            <v-btn
              class="ml-1 mt-1 mr-1 mt-1"
              :color="data.color"
              @click="InputKey(data.key)"
              block
              size="large"
            >
              <v-icon v-if="data.icon" dark>{{ data.icon }}</v-icon>
              <template v-else>{{ data.label }}</template>
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<style></style>
