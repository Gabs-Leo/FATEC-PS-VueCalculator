<script setup lang="ts">
  import { ref } from 'vue'
  defineProps<{ msg: string }>()

  enum Operation {
    PLUS, MINUS, DIVIDE, MULTIPLY, NONE
  }

  const outputValue = ref<string>("");
  const lastNumber = ref<string>("");
  const lastOperation = ref<Operation>(Operation.NONE);
  const endOfOperation = ref<boolean>(false);

  const addNumber = (num:number) => {
    if(outputValue.value === lastNumber.value || endOfOperation.value){
      outputValue.value = "";
    }
    endOfOperation.value = false
    outputValue.value = `${outputValue.value}${num}`;
  }

  const addNumberToOperation = (operation:Operation) => {
    outputValue.value = outputValue.value.replace(",", ".");
    if(lastOperation.value === Operation.NONE) {
      endOfOperation.value = true;
      lastOperation.value = operation;
      lastNumber.value = outputValue.value;
      outputValue.value = outputValue.value.replace(".", ",");
      return;
    }
    
    switch(lastOperation.value){
      case Operation.PLUS:
        outputValue.value = String(Number(lastNumber.value) + Number(outputValue.value));
        lastOperation.value = Operation.PLUS;
        break;
      case Operation.MINUS:
        if(lastNumber.value === ""){
          lastNumber.value = String(Number(outputValue.value)*2);
        }
        outputValue.value = String(Number(lastNumber.value) - Number(outputValue.value));
        lastOperation.value = Operation.MINUS;
        break;
      case Operation.MULTIPLY:
        if(lastNumber.value === ""){
          lastNumber.value = "1";
        }
        console.log(lastNumber.value);
        let result = Number(lastNumber.value) * Number(outputValue.value);
        outputValue.value = String(result);
        
        lastOperation.value = Operation.MULTIPLY;
        break;
      case Operation.DIVIDE:
        if(lastNumber.value === ""){
          lastNumber.value = String(Number(outputValue.value)**2);
        }
        outputValue.value = String((Number(lastNumber.value) / Number(outputValue.value)));
        lastOperation.value = Operation.DIVIDE;
        break;
    }
    outputValue.value = outputValue.value.replace(".", ",");
    lastOperation.value = operation;
    lastNumber.value = outputValue.value;
  }

  const addPoint = () => {
    if(!outputValue.value.includes(","))
      outputValue.value += ",";
  }

  const processOutput = () => {
    addNumberToOperation(lastOperation.value)
    lastNumber.value = "";
    endOfOperation.value = true
    lastOperation.value = Operation.NONE;
  }

  const clearField = () => {
    outputValue.value = "";
    lastNumber.value = "";
    lastOperation.value = Operation.NONE
  }
  
</script>

<template>
  <div class="col-md-4 m-3 d-flex justify-content-center w-100">
    <table class="table table-bordered">
      <tbody>
        <tr>
          <td class="output align-middle" colspan="4">
            {{ outputValue || 0 }}
          </td>
        </tr>
        <tr>
          <td @click="clearField()" class="align-middle" colspan="3">
            C
          </td>
          <td @click="addNumberToOperation(Operation.PLUS)" class="align-middle" colspan="1"><i class="fa-solid fa-plus"></i></td>
        </tr>
        <tr>
          <td @click="addNumber(7)" class="align-middle">7</td>
          <td @click="addNumber(8)" class="align-middle">8</td>
          <td @click="addNumber(9)" class="align-middle">9</td>
          <td @click="addNumberToOperation(Operation.DIVIDE)" class="align-middle"><i class="fa-solid fa-divide"></i></td>
        </tr>
        <tr>
          <td @click="addNumber(4)" class="align-middle">4</td>
          <td @click="addNumber(5)" class="align-middle">5</td>
          <td @click="addNumber(6)" class="align-middle">6</td>
          <td @click="addNumberToOperation(Operation.MULTIPLY)" class="align-middle"><i class="fa-solid fa-xmark"></i></td>
        </tr>
        <tr>
          <td @click="addNumber(1)" class="align-middle">1</td>
          <td @click="addNumber(2)" class="align-middle">2</td>
          <td @click="addNumber(3)" class="align-middle">3</td>
          <td @click="addNumberToOperation(Operation.MINUS)" class="align-middle"><i class="fa-solid fa-minus"></i></td>
        </tr>
        <tr>
          <td @click="addPoint()"class="align-middle">,</td>
          <td @click="addNumber(0)" class="align-middle">0</td>
          <td @click="processOutput()" colspan="2" class="equals align-middle"><i class="fa-solid fa-equals"></i></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
*{
  font-size: 20px;
  
}
table td{
  background-color: #3b3b3b;
  color: white;
  border: 2px solid #202020;
  border-radius: 8px;
  width: 100px;
  height: 80px;
  vertical-align: center;
}
table td:hover{
  cursor: pointer;
}
table td:active{
  background-color: #282828
}

.output {
  font-size: 30px;
  text-align: end;
  font-weight: bold;
}
.output, table{
  background-color: #202020 !important;
}
.equals{
  background-color: #f783b2;
  color: #202020;
}
.equals:active{
  background-color: #cb6e94
}
</style>
