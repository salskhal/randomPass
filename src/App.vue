<script setup>
import { computed, ref } from "@vue/reactivity";
import Copy from "./assets/icons/copy.svg";
import Arrow from "./assets/icons/arrow-right.svg";

const range = ref(10);

const isNum = ref(true)
const isCap = ref(true)
const isLow = ref(true)
const isSym = ref(true)

const all = ref("");
const error = ref("")
const pass = ref("")


function getRandomLower() {
	return String.fromCharCode(Math.floor(Math.random() * 26) + 97);
}

function getRandomUpper() {
	return String.fromCharCode(Math.floor(Math.random() * 26) + 65);
}

function getRandomNumber() {
	return +String.fromCharCode(Math.floor(Math.random() * 10) + 48);
}

function getRandomSymbol() {
	const symbols = '!@#$%^&*(){}[]=<>/,.'
	return symbols[Math.floor(Math.random() * symbols.length)];
}



const randomFunc = {
	lower: getRandomLower,
	upper: getRandomUpper,
	number: getRandomNumber,
	symbol: getRandomSymbol
}

const generate = () => {
  const length = range.value
	const hasLower = isLow.value
	const hasUpper = isCap.value
	const hasNumber = isNum.value
	const hasSymbol = isSym.value

  pass.value = generatePassword(hasLower, hasNumber,hasSymbol,hasUpper,length)
};

const generatePassword = (lower, number, symbol, upper, length) =>{
  let generated = ""
  const typesCount = lower + upper + number + symbol;
  const typesArr = [{lower}, {upper}, {number}, {symbol}].filter(item => Object.values(item)[0]);
  if(typesCount === 0) {
		return '';
	}
  for(let i=0; i<length; i+=typesCount) {
		typesArr.forEach(type => {
			const funcName = Object.keys(type)[0];
			generated += randomFunc[funcName]();
		});
	}

  const finalPassword = generated.slice(0, length);
	
	return finalPassword;
}
</script>

<template>
  <main class="min-h-screen flex items-center justify-center bg-slate-900 body">
    <div class="w-[80%] md:w-[50%] lg:w-[40%] flex flex-col gap-5">
      <h1 class="text-center text-gray-300 text-xl">Password Generator</h1>
      <div class="flex items-center bg-gray-700 p-4 justify-between">
        <h4 class="font font-semibold text-xl text-gray-400">{{ pass }}</h4>
        <Copy />
      </div>
      <form class="bg-gray-700 p-4" @submit.prevent="">
        <div class="flex items-center justify-between mb-3">
          <h2 class="text-xl text-white">Character Length</h2>
          <p class="text-3xl text-green-600 font-semibold">{{ range }}</p>
        </div>
        <input
          type="range"
          name=""
          id=""
          v-model="range"
          min="4"
          max="20"
          class="w-full h-2 bg-slate-900 rounded-lg appearance-none cursor-pointer range-lg slider"
        />
        <div class="flex flex-col gap-4 text-white my-4">
          <div class="flex items-center">
            <input type="checkbox" name="" id="" v-model="isCap"/>
            <p class="ml-5 text-lg">Include Uppercase Letters</p>
          </div>
          <div class="flex items-center">
            <input type="checkbox" name="" id="" v-model="isLow"/>
            <p class="ml-5 text-lg">Include Lowercase Letters</p>
          </div>
          <div class="flex items-center">
            <input type="checkbox" name="" id="" v-model="isNum"/>
            <p class="ml-5 text-lg">Include numbers</p>
          </div>
          <div class="flex items-center">
            <input type="checkbox" name="" id="" v-model="isSym"/>
            <p class="ml-5 text-lg">Include Symbols</p>
          </div>
        </div>
        <span>{{error}}</span>
        <button
          class="flex items-center justify-center bg-green-600 p-4 w-full text-white mt-10"
          @click="generate"
        >
          <p class="mr-4">GENERATE</p>
          <Arrow />
        </button>
      </form>
    </div>
  </main>
</template>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Source+Code+Pro:wght@400;500;600;700&display=swap');

  .body{
    font-family: "Source Code Pro", sans-serif;
  }
.slider::-moz-range-thumb {
  /* background: #04aa6d; */
  cursor: pointer;
  --tw-bg-opacity: 1;
  background-color: rgb(22 163 74 / var(--tw-bg-opacity));
}
</style>
