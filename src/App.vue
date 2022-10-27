<template>
  <div class="container">
    <div slot="header">
      <b style="font-size: 27px">Project Techbodai</b>
    </div>
    <el-button v-if="sortedbyASC" style="float: left" @click="sortList('cca3')" type="warning"
      >Asc</el-button
    >
    <el-button v-else style="float: left" @click="sortList('cca3')" type="success"
      >Desc</el-button
    >
    <el-col :span="20" style="text-align: end">
      <el-form :inline="true" class="demo-form-inline" style="margin-top: 1%">
        <el-form-item label="Seach Info:">
          <el-input
            placeholder="Search..."
            clearable
            v-model="keywordCountry"
          />
        </el-form-item>
      </el-form>
    </el-col>
    <el-button
      type="success"
      @click="searchData()"
      style="float: right; margin-right: 66px; margin-top: 13px"
      >Search</el-button
    >
    <el-table
      v-loading="loading"
      :data="tableData"
      fit
      highlight-current-row
      style="margin-top: 2%; text-align: center"
      border
    >
      <el-table-column
         label="Num"
         type="index"
         width="60">
       </el-table-column>
      <el-table-column prop="idd" label="Idd" width="60">
        <template slot-scope="{ row }">
          <span v-if="row.idd">{{ row.idd.root }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="altSpellings" label="altSpellings" width="180">
        <template slot-scope="{ row }">
          <span v-if="row.altSpellings">{{ row.altSpellings[1] }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="translations" label="Native Name">
        <template slot-scope="{ row }">
          <span v-if="row.translations.zho">{{
            row.translations.zho.official
          }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="cca2" label="Cca2">
        <template slot-scope="{ row }">
          <span v-if="row.cca2">{{ row.cca2 }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="cca3" label="Cca3">
        <template slot-scope="{ row }">
          <span v-if="row.cca3">{{ row.cca3 }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="name" label="Country">
        <template slot-scope="{ row }">
          <span
            style="cursor: pointer"
            v-if="row.name"
            @click="getInfoCountry(row)"
            >{{ row.name.common }}</span
          >
        </template>
      </el-table-column>
      <el-table-column prop="flags" label="Flags">
        <template slot-scope="{ row }">
          <el-image
            v-if="row.flags"
            style="width: 100px; height: 70px"
            :src="row.flags.png"
          ></el-image>
        </template>
      </el-table-column>
    </el-table>
    <br />
    <jw-pagination
      :pageSize="25"
      :items="exampleItems"
      @changePage="onChangePage"
    ></jw-pagination>
    <el-dialog title="Info of country" :visible.sync="countryDialog">
      <el-dialog
        width="30%"
        title="Inner Dialog"
        :visible.sync="countryCancelDialog"
        append-to-body
      >
      </el-dialog>
      <el-table :data="tableData" height="250" style="width: 100%">
        <el-table-column prop="region" label="Region">
          <template slot-scope="{ row }">
            <span v-if="row.region">{{ row.region }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="subregion" label="Subregion">
          <template slot-scope="{ row }">
            <span v-if="row.subregion">{{ row.subregion }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="languages" label="Language">
          <template slot-scope="{ row }">
            <span v-if="row.languages.eng">{{ row.languages.eng }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="area" label="Area">
          <template slot-scope="{ row }">
            <span v-if="row.area">{{ row.area }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="car" label="Car">
          <template slot-scope="{ row }">
            <span v-if="row.car.signs[0]">{{ row.car.signs[0] }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="timezones" label="TimeZones">
          <template slot-scope="{ row }">
            <span v-if="row.timezones[0]">{{ row.timezones[0] }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="capitalInfo" label="Capital">
          <template slot-scope="{ row }">
            <span v-if="row.capitalInfo.latlng[0]">{{
              row.capitalInfo.latlng[0]
            }}</span>
          </template>
        </el-table-column>
      </el-table>

      <div slot="footer" class="dialog-footer">
        <el-button @click="countryDialog = false">Cancel</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    initialPage: {
      type: Number,
      default: 1,
    },
    pageSize: {
      type: Number,
      default: 25,
    },
    maxPages: {
      type: Number,
      default: 25,
    },
  },
  data() {
    return {
      tableData: [],
      countryDialog: false,
      countryCancelDialog: false,
      keywordCountry: "",
      selectRow: {},
      infoCountry: [],
      exampleItems: [],
      sortedData: [],
      sortedbyASC: true,
      loading: true,
    };
  },
  mounted() {
    this.fetchData();
    this.tableData = this.originalData;
  },
  methods: {
    fetchData() {
      const getData = "https://restcountries.com/v3.1/all";
      this.$http.get(getData).then((res) => {
        this.tableData = res.data;
        this.exampleItems = res.data;
        this.loading = true;
        this.loading = false;
      });
    },
    searchData() {
      const getData = `https://restcountries.com/v3.1/name/${this.keywordCountry}`;
      this.$http
        .get(getData, this.keywordCountry, this.exampleItems)
        .then((res) => {
          this.tableData = res.data;
        });
      console.log(this.tableData);
    },
    getInfoCountry(row) {
      this.countryDialog = !this.countryDialog;
      this.selectRow = row;
    },
    onChangePage(tableData) {
      this.tableData = tableData;
    },
    sortList(sortBy) {
      if (this.sortedbyASC) {
        this.tableData.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
        this.sortedbyASC = false;
      } else {
        this.tableData.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
        this.sortedbyASC = true;
      }
    },
  },
};
</script>
<style>
.container {
  text-align: center;
  margin-top: 3%;
}
.el-table th.el-table__cell > .cell {
  color: green;
  text-align: center;
}
.el-table td.el-table__cell div {
  text-align: center;
}
li.page-item.page-number.active {
  color: red;
}
</style>
