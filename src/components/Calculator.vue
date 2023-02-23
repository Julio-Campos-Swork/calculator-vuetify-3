<script setup>
import { ref } from "@vue/reactivity";
import { evaluate, round } from "mathjs";
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
  { key: 7, label: "7", color: "blue" },
  { key: 8, label: "8", color: "blue" },
  { key: 9, label: "9", color: "blue" },
  { key: "*", label: "×", color: "indigo lighten-4" },
  { key: 4, label: "4", color: "blue" },
  { key: 5, label: "5", color: "blue" },
  { key: 6, label: "6", color: "blue" },
  { key: "-", label: "-", color: "indigo lighten-4" },
  { key: 1, label: "1", color: "blue" },
  { key: 2, label: "2", color: "blue" },
  { key: 3, label: "3", color: "blue" },
  { key: "+", label: "+", color: "indigo lighten-4" },
  { key: "%", label: "±", color: "indigo lighten-4" },
  { key: 0, label: "0", color: "blue" },
  { key: ".", label: ".", color: "indigo lighten-4" },
  { key: "=", label: "=", color: "indigo lighten-4" },
]);


const arrayCalculos = ref([]);
    const operadorClick = ref(true);
    const actual = ref("");
    const resultado = ref("");
    const acumulador = ref("");

// const isNumber = (btn) => !isNaN(btn);

    const btnAccion = (valor) => {
      if(valor === "="){
        btnResultado();
      }else if(valor === "ce"){
        btnReiniciar();
      }else{

        if (!isNaN(valor)) {
          if (operadorClick.value) {
            actual.value = "";
            operadorClick.value = false;
          }
          actual.value = `${actual.value}${valor}`;
        } else {
          ejecutarOperacion(valor);
        }
      }

    };

    const ejecutarOperacion = (valor) => {
      if (valor === "%") {
        if (actual.value !== "") {
          actual.value = `${parseFloat(actual.value) / 100}`;
        }
        return;
      }

      if (valor === ".") {
        console.log(actual.value);
        if (actual.value.indexOf(".") === -1) {
          if (operadorClick.value) {
            actual.value = "";
            operadorClick.value = false;
          }
          actual.value = `${actual.value}${valor}`;
        }
        return;
      }

      agregarOperador(valor);
    };

    const agregarOperador = (operador) => {
      // console.log(operador);
      if (!operadorClick.value) {
        acumulador.value += `${actual.value} ${operador} `;
        actual.value = "";
        operadorClick.value = true;
      }
    };

    const btnReiniciar = () => {
      actual.value = "";
      resultado.value = "";
      acumulador.value = "";
      operadorClick.value = false;
    };

    // https://dev.to/spukas/everything-wrong-with-javascript-eval-35on
    const parse = (str) => {
      return Function(`'use strict'; return (${str})`)();
    };

    const btnResultado = () => {
      if (!operadorClick.value) {
        resultado.value = evaluate(acumulador.value + actual.value);
        resultado.value = round(resultado.value, 3);
        arrayCalculos.value.push(
          `${acumulador.value} ${actual.value} = ${resultado.value}`
        );
      } else {
        resultado.value = "Error!";
      }
    };




</script>
<template>
  <v-card elevation="24" hover width="250" height="500">
    <v-card-title> Calculator</v-card-title>

        <v-text-field variant="plain" single-line density="compact" class="d-flex align-end mb-1 justify-end text-no-wrap" > {{ resultado }} </v-text-field>
        <v-text-field variant="plain" density="compact" class="d-flex align-end mb-1 justify-end" > {{ acumulador + actual }} </v-text-field>
    <v-card-text>
      <v-container grid-list-xs pa-1>

        <v-row justify="start" no-gutters>
          <v-col cols="3" v-for="data in calcValues" :key="data.key">
            <v-btn
              class="ml-1 mt-1 mr-1 mt-1"
              :color="data.color"
              @click="btnAccion(data.key)"
              block
              size="large"
            >
              <v-icon v-if="data.icon" dark>{{ data.icon }}</v-icon>
              <template v-else>{{ data.label }}</template>
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
      <!-- <v-card-text>
        {{ arrayCalculos }}
      </v-card-text> -->
    </v-card-text>
  </v-card>
</template>

<style></style>
