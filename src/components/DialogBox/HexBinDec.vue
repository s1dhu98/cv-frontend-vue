<template>
    <v-dialog
        v-model="SimulatorState.dialogBox.hex_bin_dec_converter_dialog"
        :persistent="false"
    >
        <v-card class="messageBoxContent">
            <v-card-text>
                <p class="dialogHeader">Hex-Bin-Dec Converter</p>
                <v-btn
                    size="x-small"
                    icon
                    class="dialogClose"
                    @click="
                        SimulatorState.dialogBox.hex_bin_dec_converter_dialog = false
                    "
                >
                    <v-icon>mdi-close</v-icon>
                </v-btn>
                <div
                    v-for="(value, index) in Object.entries(inputArr)"
                    id="bitconverterprompt"
                    :key="value[0]"
                    title="Dec-Bin-Hex-Converter"
                >
                    <label>{{ value[1].label }}</label>
                    <br />
                    <input
                        :id="value[0]"
                        type="text"
                        :value="value[1].val"
                        :label="value[1].label"
                        name="text1"
                        @keyup="(payload) => converter(payload)"
                    />
                    <br /><br />
                </div>
            </v-card-text>
            <v-card-actions>
                <v-btn class="messageBtn" block @click="setBaseValues(0)">
                    Reset
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
  <v-dialog
      v-model="SimulatorState.dialogBox.hex_bin_dec_converter_dialog"
      :persistent="false"
  >
      <v-card class="messageBoxContent">
          <v-card-text>
              <p class="dialogHeader">Hex-Bin-Dec Converter</p>
              <v-btn
                  size="x-small"
                  icon
                  class="dialogClose"
                  @click="
                      SimulatorState.dialogBox.hex_bin_dec_converter_dialog = false
                  "
              >
                  <v-icon>mdi-close</v-icon>
              </v-btn>
              <div
              v-for="(value, index) in Object.entries(inputArr)"
                  id="bitconverterprompt"
                  :key="value[0]"
                  title="Dec-Bin-Hex-Converter"
              >
                  <label>{{ value[1].label }}</label>
                  <br />
                  <input
                      :id="value[0]"
                      type="text"
                      :value="value[1].val"
                      :label="value[1].label"
                      name="text1"
                      @keyup="(payload) => converter(payload)"
                  />
                  <br /><br />
              </div>
          </v-card-text>
          <v-card-actions>
              <v-btn class="messageBtn" block @click="setBaseValues(0)">
                  Reset
              </v-btn>
          </v-card-actions>
      </v-card>
  </v-dialog>
</template>

<script lang="ts" setup>
import { useState } from '#/store/SimulatorStore/state'
const SimulatorState = useState()
import { onMounted, ref } from '@vue/runtime-core'

const inputArr = ref({
    decimalInput: {
        val: '16',
        label: 'Decimal value',
    },
    binaryInput: {
        val: '0b10000',
        label: 'Binary value',
    },
    bcdInput: {
        val: '00010110',
        label: 'Binary-coded decimal value',
    },
    octalInput: {
        val: '020',
        label: 'Octal value',
    },
    hexInput: {
        val: '0x10',
        label: 'Hexadecimal value',
    },
})

onMounted(() => {
    SimulatorState.dialogBox.hex_bin_dec_converter_dialog = false
})

function converter(e: KeyboardEvent) {
    const target = <HTMLInputElement>e.target!
    let value = target.value

    // Regular expressions for validating input
    const regexBinary = /[^01]/g
    const regexOctal = /[^0-7]/g
    const regexDecimal = /[^0-9]/g
    const regexHex = /[^0-9A-Fa-fx]/g

    switch (target.id) {
        case 'decimalInput':
            value = value.replace(regexDecimal, '')
            decimalConverter(value)
            break
        case 'binaryInput':
            value = value.replace(regexBinary, '')
            binaryConverter(value)
            break
        case 'bcdInput':
            value = value.replace(regexBinary, '')
            bcdConverter(value)
            break
        case 'octalInput':
            value = value.replace(regexOctal, '')
            octalConverter(value)
            break
        case 'hexInput':
            value = value.replace(regexHex, '')
            // Handle '0x' prefix like binary
            let hexVal = value
            if (hexVal.slice(0, 2).toLowerCase() === '0x') {
                hexVal = hexVal.slice(2)
            }
            hexConverter(hexVal)
            break
    }

    // Update the input field with the cleaned value
    target.value = value
}

function convertToBCD(value: number) {
    let digits = value.toString().split('')
    let bcdOfDigits = digits.map(function (digit) {
        return parseInt(digit).toString(2).padStart(4, '0')
    })
    return bcdOfDigits.join('')
}

function setBaseValues(x: number) {
      </v-dialog>
    setBaseValues(x)
}

function hexConverter(input: string) {
    let x = parseInt(input, 16)
    setBaseValues(x)
}
</script>

<style scoped>
#bitconverterprompt {
    color: var(--text-lite);
}

#bitconverterprompt input {
    color: var(--text-lite);
}

#bitconverterprompt label {
    color: var(--text-lite) !important;
}

#bitconverterprompt {
    text-align: center;
    font: inherit;
    border: none;
    margin-top: 5px;
    padding: 0;
}

#bitconverterprompt input {
    background: transparent;
    border: none;
    outline: none;
    text-align: center;
    font: inherit;
}

#bitconverterprompt input:focus {
    border: none;
}
</style>
