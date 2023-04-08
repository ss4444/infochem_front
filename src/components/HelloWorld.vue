<template>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
 
 <div class="example-2">
   <div class="form-group">
     <input  @change="onFileSelected" type="file" name="file" id="file" class="input-file">
     <label for="file" class="btn btn-tertiary js-labelFile">
       <i class="icon fa fa-check"></i>
       <span class="js-fileName">Загрузить файл</span>
     </label>
   </div>
   <p>{{ this.benz_92 }}</p>
   <p>{{ this.benz_95 }}</p>
   <p>{{ this.benz_98 }}</p>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      selectedFile: null,
      benz_92: null,
      benz_95: null,
      benz_98: null,
    }
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0]
      // console.log(event)
      // var user = {
      //   user: "Fred"
      // }
      const config = {
        headers: {
          'content-type': 'multipart/form-data'
        }
      }
      const file = new FormData();
      file.append('file', this.selectedFile)
      axios.post('http://10.82.244.70:8000/upload_file', file, config).then(res=>{
        console.log(res.status)
        if (res.status == 200){
        this.benz_92 = "92 бензин" + " " + res.data.benz_92 + "%"
        this.benz_95 = "95 бензин" + " " + res.data.benz_95 + "%"
        this.benz_98 = "98 бензин" + " " + res.data.benz_98 + "%"
        }
        if (res.status == 204) {
          this.benz_92 = ""
          this.benz_95 = "Не верный тип файла"
          this.benz_98 = ""
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.example-2 .btn-tertiary{color:#555;padding:0;line-height:400px;width:800px;margin:auto;display:block;border:2px solid #555}
.example-2 .btn-tertiary:hover,.example-2 .btn-tertiary:focus{color:#888;border-color:#888}
.example-2 .input-file{width:.1px;height:.1px;opacity:0;overflow:hidden;position:absolute;z-index:-1}
.example-2 .input-file + .js-labelFile{overflow:hidden;text-overflow:ellipsis;white-space:nowrap;padding:0 10px;cursor:pointer}
.example-2 .input-file + .js-labelFile .icon:before{content:"\f093"}
.example-2 .input-file + .js-labelFile.has-file .icon:before{content:"\f00c";color:#5AAC7B}
</style>
