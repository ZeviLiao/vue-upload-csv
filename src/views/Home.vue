<template>
  <div class="home">
    <h1>Uploading a CSV File</h1>

    <!-- <input type="file" id="csvfile"> -->
    <input type="file" id="file" ref="myFiles" class="custom-file-input"
  @change="previewFiles" multiple>

    <b>Header</b>
    <select id="header">
      <option>true</option>
      <option>false</option>
    </select>

    <br>
    <button id="submitbutton" type="button" @click="upload">read.csv</button>
  </div>
</template>

<script>
// @ is an alias to /src

import Papa from 'papaparse'

export default {
  data () {
    return {
      files: [],
      doc: null
    }
  },
  methods: {
    previewFiles () {
      this.files = this.$refs.myFiles.files
    },
    upload () {
      if (this.files.length === 0) return
      const that = this
      const fileToLoad = this.files[0]
      const reader = new FileReader()
      reader.onload = fileLoadedEvent => {
        Papa.parse(fileLoadedEvent.target.result, {
          header: false,
          complete (results) {
            let snJson = results.data.map(x => {
              let [y] = x // header: false
              // let y = x.sn // header: true
              return y
            })
            that.doc = JSON.stringify(snJson, null, 2)
            console.log(that.doc)
          },
          error (errors) {
            console.log('error', errors)
          }
        })
      }
      reader.readAsText(fileToLoad)
    }
  }
}
</script>
