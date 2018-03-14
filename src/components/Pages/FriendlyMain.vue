<template>
  <section class="container">
    <div class="columns">
      <div class="column is-half is-offset-one-quarter">
        <h1>
          Please enter a list of words, or drag and drop a text file to check for <b>Friendly Words!</b>
        </h1>
        <div class="upload-box">
          <textarea class="textarea" ref="wordTextArea" placeholder="Each word should be on a new line" v-model="textAreaWords" @blur="unfocusTextArea()"></textarea>
          <input id="fileUpload" class="input-file" type="file" @change="readWords($event.target.files)" @click="focusTextArea($event)" accept=".txt" />
        </div>
      </div>
    </div>
    <div>
      <p class="has-text-danger">
        {{errors[0]}}
      </p>
      <button v-if="inputWords.length !== 1"class="button is-success is-medium" @click="parseWords">Parse {{inputWords.length}} Words</button>
      <button v-else class="button is-success is-medium" @click="parseWords">Parse {{inputWords.length}} Word</button>
    </div>
    <hr />
    <div class="columns has-text-left">
      <div class="column is-half">
        <p>
          Two words are considered “friendly” if there exists a one to one mapping of letters between the two.
        </p>
        <p>
          GAGA and BOBO are friendly because mapping G<->B and A<->O would make them the same.
        </p>
        <p>
          This site will take a list of words and output the number of <b>Friendly Words</b> that are found.
        </p>
      </div>
      <div class="column is-half">

      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'FriendlyMain',
  data() {
    return {
      inputWords: [],
      textAreaWords: '',
      errors: [],
      totalWords: 0,
      friendlyCount: 0,
    };
  },
  methods: {
    readWords(wordFile) {
      this.resetErrors();
      this.textAreaWords = [];

      // Verify that a file was uploaded
      if (!wordFile.length) {
        return;
      }

      const file = wordFile[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        // Read all lines from the file and put them in a local variable
        const lines = e.target.result.split(/\r\n|\n/);
        this.inputWords = lines;
        this.textAreaWords = `${this.inputWords.length} word(s) to parse from text file`;
      };
      reader.readAsText(file);
    },
    resetErrors() {
      this.errors = [];
    },
    focusTextArea(e) {
      this.resetErrors();
      // Prevent the file browser from opening
      e.preventDefault();
      // Focus on the TextArea
      this.$refs.wordTextArea.focus();
      // Move the file input field behind the text area while it is focused
      document.getElementById('fileUpload').style.zIndex = -1;
    },
    unfocusTextArea() {
      // Make sure there is text in the textarea, and that it is not the file upload string
      if (this.textAreaWords !== '' && !this.textAreaWords.match(/words to parse from.*/)) {
        // Remove any empty lines, space in between letters and any non-letter characters
        this.textAreaWords = this.textAreaWords.replace(/(\s*$| *)[^a-zA-Z\s]*/gm, '');
        this.inputWords = this.textAreaWords.split(/\r\n|\n/);
      } else if (this.inputWords.length > 0) { // No words to parse in the textarea, check if the text file count should be used
        this.textAreaWords = `${this.inputWords.length} word(s) to parse from text file`;
      }

      // Reset the file input field's z position
      document.getElementById('fileUpload').style.zIndex = 1;
    },
    parseWords() {
      // Verify that there are words to parse
      if (this.inputWords.length === 0) {
        this.errors.push('No words to parse');
      } else if (this.inputWords.length === 1) {
        this.errors.push('There must be at least two words to find friends');
      } else { // Parse the words for friendly words
        this.totalWords = this.inputWords.length;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .upload-box {
    position: relative;
  }

  .input-file {
    opacity: 0; /* invisible but it's there! */
    top: 0;
    left: 0;
    width: 100%;
    height:100%;
    position: absolute;
    cursor: text;
    z-index: 1;
  }

  h1 {
    font-size: 1.5em;
    margin: 20px 0;
  }

  textarea {
    resize: none;
  }

  p {
    font-size: 1.2em;
    margin: 10px 0;
  }

</style>
