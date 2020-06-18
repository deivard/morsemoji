<template>
  <div class="flex-col justify-center max-w-md">
    <h5>Morse code settings</h5>
    <div class="justify-center mb-2 p-3">

      <label for="short">Short</label><input ref="short" @input="setShort" type="text" id="short" placeholder="🙂" class="w-2/12 mx-2 px-2 rounded">
      <label for="long">Long</label><input ref="long" @input="setLong" type="text" placeholder="🙃" id="long" class="w-2/12 mx-2 px-2 rounded" >
    </div>
    
    <h5>Text to encode</h5>
    <textarea name="in" id="in" cols="30" rows="3" @input="encode" ref="input" class="w-3/4 mb-2 rounded" style="resize: none;"></textarea>
    <h5>Encoded text</h5>
    <textarea name="out" id="out" cols="30" rows="5" @input="decode" ref="output" class="w-3/4 rounded" style="resize: none;"></textarea>
    <div class="m-3">
      <button @click="copyText" class="bg-yellow-400 hover:bg-yellow-500 text-gray-800 font-bold py-2 px-4 rounded">Copy text</button>
    </div>
  </div>  
</template>

<script>
export default {
  name: "Morsemoji",
  
  data(){
    return {
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
  methods: {
    setShort: function() {
      this.shortChar = this.$refs.short.value
      console.log(this.shortChar)
    },
    setLong: function() {
      this.longChar = this.$refs.long.value
      console.log(this.longChar)
    },

    encode: function() {
      var inputText = this.$refs.input.value
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
        outputText += " " + morse
      }
      this.$refs.output.value = outputText
      // console.log(inputText)
    },

    decode: function() {
      var decodeText = this.$refs.output.value //.split(/(?<! ) /g)
      decodeText = decodeText.split(/  /g).join(" / ").split(" ")
      var decoded = ""
      for (var s in decodeText) {
        var morse = decodeText[s]
        console.log(morse)
        morse = morse.split(this.shortChar).join(".")
        morse = morse.split(this.longChar).join("-")
        // morse = morse.split("/").join(" ")
        var char = this.codeMorse[morse]
        if (typeof char === 'undefined'){
          char = ""
        }
        else {
        }
        decoded += char
      }
      this.$refs.input.value = decoded
    },

    copyText: function() {
      var copyText = this.$refs.output
      copyText.select()
      copyText.setSelectionRange(0, 99999);

      document.execCommand("copy");
    }

  }

}
</script>

<style>

</style>