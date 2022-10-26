<template>
   <div class="container">
    <div slot="header">
        <b>Test Project Techbodai</b>
      </div>


      <el-col :span="20" style="text-align: end;">
          <el-form
            :inline="true"
            class="demo-form-inline"
            style="margin-top: 1%"
          >
            <el-form-item label="Seach Info:">
              <el-input
                v-model="usernameKeyword"
                placeholder="Search..."
                clearable
              />
            </el-form-item>
          </el-form>
        </el-col>
      <el-button
        type="success"
        style="float: right; margin-right: 66px; margin-top: 10px;"
        @click="searchData"
      >Search</el-button>


     <el-table
         :data="tableData"
         fit
         highlight-current-row
         style="margin-top: 2%; text-align: center;"
         border
      >
       <el-table-column
         prop="idd"
         label="Idd"
         width="60">
         <template slot-scope="{ row }">
          <span v-if="row.idd">{{ row.idd.root }}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="altSpellings"
         label="altSpellings"
         width="180">
         <template slot-scope="{ row }">
          <span v-if="row.altSpellings">{{ row.altSpellings[1] }}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="translations"
         label="Native Name">
         <template slot-scope="{ row }">
          <span v-if="row.translations.zho">{{ row.translations.zho.official }}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="cca2"
         label="Cca2">
         <template slot-scope="{ row }">
          <span v-if="row.cca2">{{ row.cca2}}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="cca3"
         label="Cca3">
         <template slot-scope="{ row }">
          <span v-if="row.cca3">{{ row.cca3}}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="name"
         label="Country">
         <template slot-scope="{ row }">
          <span v-if="row.name">{{ row.name.official}}</span>
        </template>
       </el-table-column>
       <el-table-column
         prop="flags"
         label="Flags">
         <template slot-scope="{ row }">
          <el-image
          v-if="row.flags"
          style="width: 100px; height: 70px"
          :src="row.flags.png"></el-image>
        </template>
       </el-table-column>
     </el-table>
   </div>
</template>

<script>
  export default {
    data() {
      return {
        tableData: []
      }
    },
    mounted(){
      this.fetchData()
    },
    methods: {
      fetchData() {
        const getData = 'https://restcountries.com/v3.1/all'
        this.$http.get(getData)
        .then((res) => {
          this.tableData = res.data
          console.log(this.tableData)
        })
      }
    }
  }
</script>
<style>
 .container{
  text-align: center;
  margin-top: 3%;
 }
</style>
