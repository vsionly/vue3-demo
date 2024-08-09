<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <el-upload
      v-model:file-list="fileList"
      class="upload-demo"
      action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
      multiple
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :before-upload="beforeUp"
      :limit="3"
      :on-exceed="handleExceed"
    >
      <el-button type="primary">Click to upload</el-button>
      <template #tip>
        <div class="el-upload__tip">
          jpg/png files with a size less than 500KB.
        </div>
      </template>
    </el-upload>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import * as XLSX from 'xlsx';

export default {
  name: "HomeView",
  components: {
    HelloWorld
  },
  data() {
    return {
      fileList: []
    };
  },
  created() {
    let o = {
      ew: { a: 1, b: 2 },
      w: { a: 3, b: 44 }
    };
    console.log(o.keys, 333);
  },
  methods: {
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleExceed(files, fileList) {
      this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
    },
    async beforeUp(file) {
      const data = await this.readFile(file)
      console.log(data);
      
      return false
    },
    readFile(file){
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = (event) => {
            const data = event.target.result;
            const workbook = XLSX.read(data, { type: 'binary' });
            
            // 获取第一个工作表的名字
            const firstSheetName = workbook.SheetNames[0];
            
            // 将工作表转换为 JSON 格式
            const dataJson =  XLSX.utils.sheet_to_json(workbook.Sheets[firstSheetName], { header: 1 });
            resolve(dataJson)
        };
        reader.onerror = (error) => {
            reject(error);
        };
        reader.readAsArrayBuffer(file);
      })
    }
  }
};
</script>
