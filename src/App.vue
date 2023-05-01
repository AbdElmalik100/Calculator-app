<template>
  <main :class="`${theme} d-flex align-items-center justify-content-center`">
    <div class="calc container" style="width: 500px;">
      <div class="header d-flex align-items-center justify-content-between">
        <h3>calc</h3>
        <div class="theme-control d-flex align-items-center gap-4">
          <span class="text-uppercase">Theme</span>
          <div :class="`theme-toggle ${defaultChecked === 'one' ? 'one' : defaultChecked === 'two' ? 'two' : 'three'}`">
            <span class="point"></span>
            <label for="theme-one" data-num="1">
              <input id="theme-one" name="theme" type="radio" @click="defaultChecked = 'one'"
                @click.natvie="theme = 'theme-1'">
            </label>
            <label for="theme-two" data-num="2">
              <input id="theme-two" name="theme" type="radio" @click="defaultChecked = 'two'"
                @click.natvie="theme = 'theme-2'">
            </label>
            <label for="theme-three" data-num="3">
              <input id="theme-three" name="theme" type="radio" @click="defaultChecked = 'three'"
                @click.natvie="theme = 'theme-3'">
            </label>
          </div>
        </div>
      </div>
      <div class="screen p-4 px-4 rounded my-3 text-end">
        <!-- <h1 class="result mt-2 mb-0 text-break" style="height: 45px;">{{ parseFloat(calculator || 0).toLocaleString('en-US') }}</h1> -->
        <h1 class="result mt-2 mb-0 text-break" style="height: 45px;">{{ formatNumber || 0 }}</h1>
      </div>
      <div class="key-pad p-4 px-4 rounded">
        <div class="pad row">
          <div class="col-3 mb-3" v-for="(item, index) in keys" :key="index">
            <h2 :data-key="item" @click="calculation(item)"
              class="h-100 pt-2 d-block d-flex align-items-center justify-content-center rounded text-uppercase">
              {{ item }}
            </h2>
          </div>
        </div>
        <div class="row">
          <div class="col-6 ">
            <button class="reset py-1 pt-2 rounded border-0 w-100 text-uppercase" @click="reset">Reset</button>
          </div>
          <div class="col-6">
            <button class="equal py-1 pt-2 rounded border-0 w-100" @click="equalization">=</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<script setup>
import { ref, computed } from 'vue';

const defaultChecked = ref('one')
const keys = [
  '7',
  '8',
  '9',
  'del',
  '4',
  '5',
  '6',
  '+',
  '1',
  '2',
  '3',
  '-',
  '.',
  '0',
  '/',
  'X',
]
const theme = ref('theme-1')

const specails = ['+', 'X', '/', '-', 'del']
const calculator = ref('')
const operand = ref(null)
const firstNum = ref(0)
const lastNum = ref(0)


const formatNumber = computed(() => {
  return String(calculator.value).replace(/\B(?=(\d{3})+(?!\d))/g, ',')
})

let calculation = (key) => {
  if (!specails.includes(key)) {
    calculator.value += key
    if (operand.value) {
      lastNum.value = key
    }
  }
  if (key === 'del') {
    let tempArr = String(calculator.value).split('')
    tempArr.pop()
    calculator.value = tempArr.join('')
  }
  if (['+', '-', 'X', '/'].includes(key)) {
    if (key === 'X') {
      operand.value = '*'
    } else {
      operand.value = key
    }
    firstNum.value = parseFloat(calculator.value)
    calculator.value = ''
  }
}

let equalization = () => {
  if (operand.value) {
    calculator.value = eval(firstNum.value + operand.value + parseFloat(lastNum.value))
    firstNum.value = parseFloat(calculator.value)
  }
}

let reset = () => {
  operand.value = null
  calculator.value = ''
  firstNum.value = ''
  lastNum.value = ''
}

</script>
<style lang="scss">
* {
  box-sizing: border-box;
}

:root {
  // Theme 1

  // #### Backgrounds
  --Verydark-desaturated-blue-main-background: hsl(222, 26%, 31%);
  --Very-dark-desaturated-blue-toggle-background-keypad-background: hsl(223, 31%, 20%);
  --Very-dark-desaturated-blue-screen-background: hsl(224, 36%, 15%);

  // #### Keys
  --Desaturated-dark-blue-key-background: hsl(225, 21%, 49%);
  --Desaturated-dark-blue-key-shadow: hsl(224, 28%, 35%);

  --Red-key-background-toggle: hsl(6, 63%, 50%);
  --Dark-red-key-shadow: hsl(6, 70%, 34%);

  --Light-grayish-orange-key-background: hsl(30, 25%, 89%);
  --Grayish-orange-key-shadow: hsl(28, 16%, 65%);

  // #### Text
  --Very-dark-grayish-blue: hsl(221, 14%, 31%);
  --White: hsl(0, 0%, 100%);


  // ### Theme 2

  // #### Backgrounds
  --Light-gray-main-background: hsl(0, 0%, 90%);
  --Grayish-red-toggle-background-keypad-background: hsl(0, 5%, 81%);
  --Very-light-gray-screen-background: hsl(0, 0%, 93%);

  // #### Keys
  --Dark-moderate-cyan-key-background: hsl(185, 42%, 37%);
  --Very-dark-cyan-key-shadow: hsl(185, 58%, 25%);

  --Orange-key-background-toggle: hsl(25, 98%, 40%);
  --Dark-orange-key-shadow: hsl(25, 99%, 27%);

  --Light-grayish-yellow-key-background: hsl(45, 7%, 89%);
  --Dark-grayish-orange-key-shadow: hsl(35, 11%, 61%);

  // #### Text
  --Very-dark-grayish-yellow: hsl(60, 10%, 19%);
  --White-text: hsl(0, 0%, 100%);


  // ### Theme 3

  // #### Backgrounds
  --Very-dark-violet-main-background: hsl(268, 75%, 9%);
  --Very-dark-violet-toggle-background-keypad-background-screen-background: hsl(268, 71%, 12%);

  // #### Keys
  --Dark-violet-key-background: hsl(281, 89%, 26%);
  --Vivid-magenta-key-shadow: hsl(285, 91%, 52%);

  --Pure-cyan-key-background-toggle: hsl(176, 100%, 44%);
  --Soft-cyan-key-shadow: hsl(177, 92%, 70%);

  --Very-dark-violet-key-background: hsl(268, 47%, 21%);
  --Dark-magenta-key-shadow: hsl(290, 70%, 36%);

  // #### Text
  --Light-yellow: hsl(52, 100%, 62%);
  --Very-dark-blue: hsl(198, 20%, 13%);
  --White-text: hsl(0, 0%, 100%);
}

body {
  font-family: 'League Spartan', sans-serif;
}

ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

main {
  min-height: 115vh;
  transition: 0.2s;

  &.theme-1 {
    // BackGrounds 
    background-color: var(--Verydark-desaturated-blue-main-background);

    .theme-toggle,
    .key-pad {
      background-color: var(--Very-dark-desaturated-blue-toggle-background-keypad-background);
    }

    .screen {
      background-color: var(--Very-dark-desaturated-blue-screen-background);
    }

    // Keys 
    .reset,
    [data-key="del"] {
      background-color: var(--Desaturated-dark-blue-key-background);
      border-color: var(--Desaturated-dark-blue-key-shadow) !important;
    }

    .theme-toggle span,
    .equal {
      background-color: var(--Red-key-background-toggle);
      border-color: var(--Dark-red-key-shadow) !important;
    }

    h2 {
      background-color: var(--Light-grayish-orange-key-background);
      border-color: var(--Grayish-orange-key-shadow);
    }

    // Text Colors
    .reset,
    .equal,
    h1,
    h3,
    .theme-control span,
    label::before,
    [data-key="del"] {
      color: var(--White);
    }

    h2 {
      color: var(--Very-dark-grayish-blue);
    }
  }

  &.theme-2 {
    // BackGrounds 
    background-color: var(--Light-gray-main-background);

    .theme-toggle,
    .key-pad {
      background-color: var(--Grayish-red-toggle-background-keypad-background);
    }

    .screen {
      background-color: var(--Very-light-gray-screen-background);
    }

    // Keys 
    .reset,
    [data-key="del"] {
      background-color: var(--Dark-moderate-cyan-key-background);
      border-color: var(--Very-dark-cyan-key-shadow) !important;
    }

    .theme-toggle span,
    .equal {
      background-color: var(--Orange-key-background-toggle);
      border-color: var(--Dark-orange-key-shadow) !important;
    }

    h2 {
      background-color: var(--Light-grayish-yellow-key-background);
      border-color: var(--Dark-grayish-orange-key-shadow);
    }

    // Text Colors
    h1,
    h3,
    .theme-control span,
    label::before {
      color: var(--Very-dark-grayish-yellow);
    }

    [data-key="del"],
    .reset,
    .equal {
      color: var(--White-text);
    }

  }

  &.theme-3 {
    // BackGrounds 
    background-color: var(--Very-dark-violet-main-background);

    .theme-toggle,
    .key-pad,
    .screen {
      background-color: var(--Very-dark-violet-toggle-background-keypad-background-screen-background);
    }

    // Keys 
    .reset,
    [data-key="del"] {
      background-color: var(--Dark-violet-key-background);
      border-color: var(--Vivid-magenta-key-shadow) !important;
    }

    .theme-toggle span,
    .equal {
      background-color: var(--Pure-cyan-key-background-toggle);
      border-color: var(--Soft-cyan-key-shadow) !important;
    }

    h2 {
      background-color: var(--Very-dark-violet-key-background);
      border-color: var(--Dark-magenta-key-shadow);
    }

    // Text Colors
    h1,
    h3,
    h2,
    .theme-control span,
    label::before {
      color: var(--Light-yellow);
    }

    .equal {
      color: var(--Very-dark-blue);
    }

    [data-key="del"],
    .reset {
      color: var(--White-text);
    }

  }

  .calc {
    .header {
      .theme-control {
        .theme-toggle {
          width: fit-content;
          padding: 3px 8px;
          border-radius: 50px;
          display: flex;
          gap: 10px;
          align-items: center;
          justify-content: center;
          position: relative;

          label {
            width: 20px;
            height: 20px;
            cursor: pointer;
            position: relative;

            &::before {
              content: attr(data-num);
              position: absolute;
              top: -25px;
              left: 50%;
              transform: translateX(-50%);
            }

            input {
              display: none;
            }
          }

          span {
            position: absolute;
            left: 15px;
            transition: 0.2s;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            z-index: 555;
            cursor: pointer;

            &:hover {
              filter: brightness(1.4);
            }
          }

          &.one span {
            left: 11px;
          }

          &.two span {
            left: 41px;
          }

          &.three span {
            left: 71px;
          }
        }
      }
    }

    .key-pad {

      h2,
      button {
        cursor: pointer;
        transition: 0.2s;
        border-bottom-width: 3px !important;
        border-bottom-style: solid !important;
        outline: none;
        transition: 0.2s;
      }

      button,
      h2 {
        &:hover {
          filter: brightness(1.4);
        }
      }

      h2 {
        font-size: 32px;
      }

      [data-key="del"],
      [data-key="+"],
      [data-key="-"],
      [data-key="X"],
      [data-key="/"] {
        font-size: 27px;
      }

      button {
        font-size: 25px;
      }
    }
  }
}
</style>