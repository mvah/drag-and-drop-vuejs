<style>
  .example-drag label.btn {
    margin-bottom: 0;
    margin-right: 1rem;
  }
  .example-drag .drop-active {
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    position: fixed;
    z-index: 9999;
    opacity: .6;
    text-align: center;
    background: #000;
  }
  .example-drag .drop-active h3 {
    margin: -.5em 0 0;
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    -webkit-transform: translateY(-50%);
    -ms-transform: translateY(-50%);
    transform: translateY(-50%);
    font-size: 40px;
    color: #fff;
    padding: 0;
  }
</style>
<template>
  <div class="example-drag">
    <div class="upload">
      <template v-if="files.length">
          <v-layout row wrap>
            <v-flex d-flex xs12 sm6 md4 > 
              <v-card class="pa-4 ma-2" v-for="(file, index) in files" :key="file.id">
                <v-img :lazy-src="`https://picsum.photos/350/165?random`" aspect-ratio="1" :src="file.blob" height="100" width="200" class="grey darken-4">
                    <template v-slot:placeholder>
                        <v-layout fill-height align-center justify-center ma-0>
                            <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
                        </v-layout>
                    </template>
                </v-img>
                <v-card-text class="pa-0">
                    <span> lorem ipsum lorem ipsum lorem ipsum lorem</span>
                </v-card-text>
              </v-card>
            </v-flex>
            
          </v-layout>
      </template>
      <template v-else>
        <v-card  class="pa-4 ma-2">
          <div class="text-center p-5">
            <label for="file" class="btn btn-lg btn-primary">
              Drop files anywhere to upload
              <br/>or<br/>
              Select Files</label>
          </div>
        </v-card>
      </template>
      <div v-show="$refs.upload && $refs.upload.dropActive" class="drop-active">
    		<h3>Drop files to upload</h3>
      </div>

      <div class="example-btn">
        <file-upload
          class="btn btn-primary"
          post-action="/upload/post"
          :multiple="true"
          :drop="true"
          :drop-directory="true"
          v-model="files"
          put-action="/put.method"
          @input-filter="inputFilter"
          @input-file="inputFile"
          ref="upload">
          <i class="fa fa-plus"></i>
        </file-upload>
        <button type="button" class="btn btn-success" v-if="!$refs.upload || !$refs.upload.active" @click.prevent="$refs.upload.active = true">
          <i class="fa fa-arrow-up" aria-hidden="true"></i>
         
        </button>
        <button type="button" class="btn btn-danger"  v-else @click.prevent="$refs.upload.active = false">
          <i class="fa fa-stop" aria-hidden="true"></i>
          Stop Upload
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue';
import FileUpload from 'vue-upload-component';
export default Vue.extend ({
  components: {
    FileUpload,
  },
  data: () => ({
    files: [],
  }),
  methods: {
    /**
     * Pretreatment
     * @param  Object|undefined   newFile   Read and write
     * @param  Object|undefined   oldFile   Read only
     * @param  Function           prevent   Prevent changing
     * @return undefined
     */
    inputFilter: function (newFile, oldFile, prevent) {
      if (newFile && !oldFile) {
        if (!/\.(jpeg|jpe|jpg|gif|png|webp)$/i.test(newFile.name)) {
          return prevent()
        }
      }
      newFile.blob = ''
      let URL = window.URL || window.webkitURL
      if (URL && URL.createObjectURL) {
        newFile.blob = URL.createObjectURL(newFile.file)
      }
    }
  }
});
</script>