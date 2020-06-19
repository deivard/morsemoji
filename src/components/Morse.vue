<template>
  <div class="flex-col justify-center max-w-md">
    <h5>Morse code settings</h5>
    <div class="justify-center p-3">
      <label for="short">Short</label><input ref="short" @input="setShort" type="text" id="short" placeholder="🙂" class="w-2/12 mx-2 px-2 rounded" :value="this.shortChar">
      <label for="long">Long</label><input ref="long" @input="setLong" type="text" placeholder="🙃" id="long" class="w-2/12 mx-2 px-2 rounded" :value="this.longChar" >
    </div>
    <h5>Text to encode</h5>
    <textarea name="in" id="in" cols="30" rows="3" @input="encode" ref="input" class="w-3/4 mb-2 rounded px-1" placeholder="Type the text you want to encode here..." style="resize: none;"></textarea>
    <h5>Encoded text</h5>
    <textarea name="out" id="out" cols="30" rows="5" @input="decode" ref="output" class="w-3/4 rounded px-1" placeholder="Paste encoded text here..." style="resize: none;"></textarea>
    <div>
    </div>
    <div class="m-3">
      <button @click="copyText" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded mx-2">Copy</button>
      <button @click="autoDecode" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded mx-2 mt-1" :class="{ disabledButton: !enableDecodeButton }" >Auto decode</button>
      <button @click="clearText" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded mx-2">Clear</button>
    </div>
    <div>
      <p v-show="this.decodeAttempt" class="text-sm w-3/4 mx-auto"><i>Psst... If the text didn't decode properly, try decoding again!</i></p>
    </div>
  </div>  
</template>

<script>
import Vue from "vue"
import GraphemeSplitter from "grapheme-splitter"
Object.defineProperty(Vue.prototype, '$GraphemeSplitter', { value: GraphemeSplitter });
export default {
  name: "Morse",
  
  data(){
    return {
      inputText: "",
      outputText: "",
      decodeAttempt: 0,
      showDecodeHint: false,
      shortChar: "🙂",
      longChar: "🙃",
      morseCode: {
        "0": "-----",
        "1": ".----",
        "2": "..---",
        "3": "...--",
        "4": "....-",
        "5": ".....",
        "6": "-....",
        "7": "--...",
        "8": "---..",
        "9": "----.",
        "a": ".-",
        "b": "-...",
        "c": "-.-.",
        "d": "-..",
        "e": ".",
        "f": "..-.",
        "g": "--.",
        "h": "....",
        "i": "..",
        "j": ".---",
        "k": "-.-",
        "l": ".-..",
        "m": "--",
        "n": "-.",
        "o": "---",
        "p": ".--.",
        "q": "--.-",
        "r": ".-.",
        "s": "...",
        "t": "-",
        "u": "..-",
        "v": "...-",
        "w": ".--",
        "x": "-..-",
        "y": "-.--",
        "z": "--..",
        "å": ".--.-",
        "ä": ".-.-",
        "ö": "---.",
        ".": ".-.-.-",
        ",": "--..--",
        "?": "..--..",
        "!": "-.-.--",
        "-": "-....-",
        "/": "-..-.",
        "@": ".--.-.",
        "(": "-.--.",
        ")": "-.--.-",
        " ": "  "
      },
      codeMorse: {
        "-----":"0", 
        ".----":"1", 
        "..---":"2", 
        "...--":"3", 
        "....-":"4", 
        ".....":"5", 
        "-....":"6", 
        "--...":"7", 
        "---..":"8", 
        "----.":"9", 
        ".-":"a", 
        "-...":"b", 
        "-.-.":"c", 
        "-..":"d", 
        ".":"e", 
        "..-.":"f", 
        "--.":"g", 
        "....":"h", 
        "..":"i", 
        ".---":"j", 
        "-.-":"k", 
        ".-..":"l", 
        "--":"m", 
        "-.":"n", 
        "---":"o", 
        ".--.":"p", 
        "--.-":"q", 
        ".-.":"r", 
        "...":"s", 
        "-":"t", 
        "..-":"u", 
        "...-":"v", 
        ".--":"w", 
        "-..-":"x", 
        "-.--":"y", 
        "--..":"z", 
        ".--.-": "å", 
        ".-.-": "ä",
        "---.": "ö", 
        ".-.-.-":".", 
        "--..--":",", 
        "..--..":"?", 
        "-.-.--":"!", 
        "-....-":"-", 
        "-..-.":"/", 
        ".--.-.":"@", 
        "-.--.":"(", 
        "-.--.-":")", 
        "/": " "
      },
    }
  },
  computed: {
    enableDecodeButton: function(){
      return this.outputText.length > 0
    }
  },
  methods: {
    setShort: function() {
      this.shortChar = this.$refs.short.value
      this.decodeAttempt = 0
      if (this.longChar != "" && this.shortChar != "") {
        this.encode()
      }
      // console.log(this.shortChar)
    },
    setLong: function() {
      this.longChar = this.$refs.long.value
      this.decodeAttempt = 0
      if (this.longChar != "" && this.shortChar != "") {
        this.encode()
      }
      // console.log(this.longChar)
    },

    encode: function() {
      var inputText = this.$refs.input.value
      this.inputText = inputText
      var outputText = ""
      for (let i in inputText) {
        let char = inputText.charAt(i).toLowerCase()
        var morse = this.morseCode[char]
        if (typeof morse === 'undefined'){
          morse = ""
        }
        else {
          morse = morse.replace(/\./g, this.shortChar)
          morse = morse.replace(/-/g, this.longChar)
        }
        outputText +=  (morse === "  " || outputText.slice(-2) === "  " || outputText.length == 0 ? "" : " ") + morse
      }
      this.$refs.output.value = outputText
      this.outputText = outputText
      // console.log(inputText)
    },

    decode: function() {
      var decodeText = this.$refs.output.value
      this.outputText = decodeText
      decodeText = decodeText.split(/[ ]{2}/g).join(" / ").split(" ")
      var decoded = ""
      for (var s in decodeText) {
        var morse = decodeText[s]
        morse = morse.split(this.shortChar).join(".")
        morse = morse.split(this.longChar).join("-")
        var char = this.codeMorse[morse]
        if (typeof char === 'undefined'){
          char = ""
        }
        else {
        }
        decoded += char
      }
      this.$refs.input.value = decoded
      this.inputText = decoded
    },

    autoDecode: function() {
      if(this.decodeAttempt == 0){
        var short = ""
        var long = ""
        var encrypted = this.$refs.output.value
        var splitter = new GraphemeSplitter()
        var split = splitter.splitGraphemes(encrypted)
        short = split[0]
        for(var i in split){
          if (split[i] != short && split[i] != " ") {
            long = split[i]
            break
          }
        }
    
        this.shortChar = short
        this.longChar = long
      }
      else {
        var temp = this.shortChar
        this.shortChar = this.longChar
        this.longChar = temp
      }
      if (typeof this.shortChar === "undefined") {
        this.shortChar = "."
      }
      if (typeof this.longChar === "undefined") {
        this.longChar = "-"
      }

      this.decodeAttempt = (this.decodeAttempt + 1) % 2
      this.decode()
    },

    copyText: function() {
      var copyText = this.$refs.output
      copyText.select()
      copyText.setSelectionRange(0, 99999);
      document.execCommand("copy");
      this.decodeAttempt = 0
    },

    clearText: function() {
      this.$refs.input.value = this.$refs.output.value = this.inputText = this.outputText = ""
      this.decodeAttempt = 0
    }
  }
}
</script>

<style>

.disabledButton {
  opacity: 0.5;
  /* cursor:not-allowed; */
  background-color: lightgray !important;
  pointer-events: none;
}

</style>