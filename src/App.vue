<template>
  <div id="app">
    <div class="container">
        <!-- Header Section -->
        <header id="title">
            <h1>Neno Converter</h1>
        </header>
        <!-- Main Content Section-->
        <main id="main">
        <!-- Number System Conversion -->
            <section id="number-system">
                <h2>Number System Converter</h2>
                <div class="box">
                    <div class="input-group">
                        <label>Decimal</label>
                        <input type="text" v-model="numbers.decimal">  
                    </div>
                    <div class="input-group">
                        <label>Binary</label>
                        <input type="text" v-model="numbers.binary">  
                    </div>
                    <div class="input-group">
                        <label>Octyal</label>
                        <input type="text" v-model="numbers.octal"> 
                    </div>
                    <div class="input-group">
                        <label>Hexadecimal</label>
                        <input type="text" v-model="numbers.hexadecimal">
                    </div>
                    <div class="reset-btn-container">
                      <p v-if="invalidNumber" style="color: red;">Invalid Input</p>
                        <button @click="resetNumber">Reset</button>
                    </div>
                    <div class="one-rem-space"></div>
                </div>
            </section>
        <!-- Text System Conversion -->
            <section id="text-conversion">
                <h2>Text Conversion</h2>
                <div class="box">
                    <div>
                        <div class="input-group">
                            <label>Enter Your Text Below</label>
                            <textarea name="text-field" v-model="lines.text" cols="30" rows="10"></textarea>
                        </div>
                        <div class="half-rem-space"></div>
                        <div class="input-group">
                            <label>Enter Your Binary Below</label>
                            <textarea name="binary-field" v-model="lines.binary" cols="30" rows="10"></textarea>
                        </div>
                        <div class="reset-btn-container">
                          <p v-if="invalidNumber" style="color: red;">Invalid Input</p>
                            <button @click="resetLines">Reset</button>
                        </div>
                        <div class="one-rem-space"></div>
                    </div>
                </div>
            </section>
        </main>
        
    </div>
  </div>
  
</template>

<script>
require('@/assets/css/reset.css');
require('@/assets/css/style.css');

function isBinary(text){
  for( let t of text){
    if(t !== '0' && t !== '1') return false;
  }
  return true
}

function isOctal(text){
  for( let t of text ){
    if( t < '0' || t > 7) return false;
  }
  return true
}

function isHex(text){
  for(let t of text){
    if(
      (t < '0' || t > '9')&&
      (t < 'A' || t > 'F')&&
      (t < 'a' || t > 'f')
    ){
      return false;
    }
  }
  return true;
}


function textToBinary(text){
  const charCodeArray = [];
  for( let i in text){
    charCodeArray.push(text.charCodeAt(i));
  }
  const binaryArray =  charCodeArray.map((char) => {
    return char.toString(2);
  });

  return binaryArray;
}


function binaryToText(codeArray){
  let text = '';
  for(let code of codeArray){
    const char = String.fromCharCode(parseInt(code, 2));
    text = text.concat(char);
  }
  return text;
}


export default {
  name: 'App',

  data() {
    return{
        numbers: {
        decimal:0,
        binary:0,
        octal: 0,
        hexadecimal: 0,
      },
      lines: {
        text: '',
        binary: ''
      },

      invalidNumber: false,
      invalidLine: false
    };
  },
  methods: {
    resetNumber(){
      this.numbers = {
        decimal:0,
        binary:0,
        octal: 0,
        hexadecimal: 0,
      }
      this.invalidNumber = false;
    },
    resetLines(){
      this.lines = {
        text: '',
        binary: '',
      }
      this.invalidNumber = false;
    }
  },
  watch: {
    'numbers.decimal': function(value){
      this.numbers.decimal = parseInt(value) || 0;
      this.numbers.binary = value.toString(2);
      this.numbers.octal = value.toString(8);
      this.numbers.hexadecimal = value.toString(16);
    },
    'numbers.binary': function(value){
      let decimal = parseInt(value, 2);

      if(!isBinary(value)){
        this.invalidNumber = true;
      }else{
        this.invalidNumber = false;
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = value || 0;
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexadecimal = decimal.toString(16);
    },
    'numbers.octal': function(value){
      let decimal = parseInt(`0${value}`, 8) || 0;

      if(!isOctal(value)){
        this.invalidNumber = true;
      }else{
        this.invalidNumber = false
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2) || 0;
      this.numbers.octal = value || 0;
      this.numbers.hexadecimal = decimal.toString(16);
    },
    'numbers.hexadecimal': function(value){
      let decimal = parseInt(`0x${value}`, 16) || 0;

      if(!isHex(value)){
        this.invalidNumber = true;
      }else{
        this.invalidNumber = false
      }
      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2) || 0;
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexadecimal = value || 0;
    },

    'lines.text': function(value){
      if(value.length === 0){
        this.lines.binary = ''
      }else{
        const codeArray = textToBinary(value);
        this.lines.binary = codeArray.join(' ');
      }
    },
    'lines.binary': function(value){
      if(value.length === 0){
        this.lines.text = '';
      }else{
        const codeArray = value.split(' ');
        const text = binaryToText(codeArray);
        this.lines.text = text;
      }
    }

  },
};
</script>
