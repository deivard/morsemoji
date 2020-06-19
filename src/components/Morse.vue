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
      <button @click="autoDecode" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded mx-2 mt-1" :disabled="!enableDecodeButton" :class="{ disabledButton: !enableDecodeButton }" >Auto decode</button>
      <button @click="clearText" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded mx-2">Clear</button>
    </div>
    <div>
      <p v-show="this.decodeAttempt" class="text-sm w-3/4 mx-auto"><i>Psst... If text didn't decode properly, try decoding again!</i></p>
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
        // const regex = /(?:[\u00A9\u00AE\u203C\u2049\u2122\u2139\u2194-\u2199\u21A9-\u21AA\u231A-\u231B\u2328\u23CF\u23E9-\u23F3\u23F8-\u23FA\u24C2\u25AA-\u25AB\u25B6\u25C0\u25FB-\u25FE\u2600-\u2604\u260E\u2611\u2614-\u2615\u2618\u261D\u2620\u2622-\u2623\u2626\u262A\u262E-\u262F\u2638-\u263A\u2640\u2642\u2648-\u2653\u2660\u2663\u2665-\u2666\u2668\u267B\u267F\u2692-\u2697\u2699\u269B-\u269C\u26A0-\u26A1\u26AA-\u26AB\u26B0-\u26B1\u26BD-\u26BE\u26C4-\u26C5\u26C8\u26CE-\u26CF\u26D1\u26D3-\u26D4\u26E9-\u26EA\u26F0-\u26F5\u26F7-\u26FA\u26FD\u2702\u2705\u2708-\u270D\u270F\u2712\u2714\u2716\u271D\u2721\u2728\u2733-\u2734\u2744\u2747\u274C\u274E\u2753-\u2755\u2757\u2763-\u2764\u2795-\u2797\u27A1\u27B0\u27BF\u2934-\u2935\u2B05-\u2B07\u2B1B-\u2B1C\u2B50\u2B55\u3030\u303D\u3297\u3299]|(?:\uD83C[\uDC04\uDCCF\uDD70-\uDD71\uDD7E-\uDD7F\uDD8E\uDD91-\uDD9A\uDDE6-\uDDFF\uDE01-\uDE02\uDE1A\uDE2F\uDE32-\uDE3A\uDE50-\uDE51\uDF00-\uDF21\uDF24-\uDF93\uDF96-\uDF97\uDF99-\uDF9B\uDF9E-\uDFF0\uDFF3-\uDFF5\uDFF7-\uDFFF]|\uD83D[\uDC00-\uDCFD\uDCFF-\uDD3D\uDD49-\uDD4E\uDD50-\uDD67\uDD6F-\uDD70\uDD73-\uDD7A\uDD87\uDD8A-\uDD8D\uDD90\uDD95-\uDD96\uDDA4-\uDDA5\uDDA8\uDDB1-\uDDB2\uDDBC\uDDC2-\uDDC4\uDDD1-\uDDD3\uDDDC-\uDDDE\uDDE1\uDDE3\uDDE8\uDDEF\uDDF3\uDDFA-\uDE4F\uDE80-\uDEC5\uDECB-\uDED2\uDEE0-\uDEE5\uDEE9\uDEEB-\uDEEC\uDEF0\uDEF3-\uDEF6]|\uD83E[\uDD10-\uDD1E\uDD20-\uDD27\uDD30\uDD33-\uDD3A\uDD3C-\uDD3E\uDD40-\uDD45\uDD47-\uDD4B\uDD50-\uDD5E\uDD80-\uDD91\uDDC0]))/g  
        // var emojis = [...encrypted.matchAll(regex)]
        // If the string isnt encrypted with emojis
        // if (emojis.length < 2){
        //   short = encrypted[0]
        //   for (var i in encrypted){
        //     if (encrypted[i] != short && encrypted[i] != " "){
        //       long = encrypted[i]
        //       break
        //     }
        //   }
        // }
        // else {
        //   short = emojis[0][0]
        //   for (var i in emojis){
        //     if (emojis[i][0] != short) {
        //       long = emojis[i][0]
        //       break
        //     }
        //   }
        // }
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
  opacity: 50%;
  /* cursor:not-allowed; */
  background-color: lightgray !important;
  
}
.disabledButton:hover{

}

</style>