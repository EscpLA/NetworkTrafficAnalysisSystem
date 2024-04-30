<template>
  <div class="container">
    <el-input
      style="width: 350px"
      v-model="filename"
      autosize
      prefix-icon="el-icon-document"
      placeholder="请输入文件名（默认excel-list）"
    ></el-input>
    <el-button
      style="margin: 0 0 20px 20px"
      type="primary"
      icon="document"
      @click="handleDownload"
      :loading="downloadLoading"
      >export excel</el-button
    >
    <el-table
      :data="myTableData"
      style="width: 100%"
      height="450"
      size="medium"
      stripe
      border
    >
      <el-table-column fixed prop="No" label="序号" width="80" align="center">
      </el-table-column>
      <el-table-column prop="Source" label="Source IP" width="175"> </el-table-column>
      <el-table-column prop="Destination" label="Destination IP" width="175">
      </el-table-column>
      <el-table-column prop="Protocol" label="Protocol" width="175"> </el-table-column>
      <el-table-column prop="Length" label="Length" width="175"> </el-table-column>
      <el-table-column prop="Source Port" label="Source Port" width="175">
      </el-table-column>
      <el-table-column prop="Destination Port" label="Destination Port" width="175">
      </el-table-column>
      <el-table-column prop="Label" label="Label" width="175"> </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { getTable } from "@/api/excel";
export default {
  data() {
    return {
      isDev: null,
      downloadLoading: false,
      tableDataLoading: true,
      tableData: [],
      filename: "",
      autoWidth: true,
      refresh: true,
      percentageNum: 100,
      timer: 0,
      updateTime: "",

      myTableData: [
        {
          No: "1",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "Youtube",
        },
        {
          No: "2",
          Source: "10.149.2.95",
          Destination: "101.227.134.243",
          Protocol: "TLSv1.2",
          Length: "90",
          "Source Port": "14157",
          "Destination Port": "443",
          Label: "Hangout",
        },
        {
          No: "3",
          Source: "101.227.134.243",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "86",
          "Source Port": "443",
          "Destination Port": "14157",
          Label: "Email",
        },
        {
          No: "4",
          Source: "10.149.2.95",
          Destination: "101.227.134.243",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14157",
          "Destination Port": "443",
          Label: "Gmail",
        },
        {
          No: "5",
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "Email",
        },
        {
          No: "6",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "Netflix",
        },
        {
          No: "7",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "93",
          "Source Port": "443",
          "Destination Port": "14589",
          Label: "Youtube",
        },
        {
          No: "8",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "78",
          "Source Port": "443",
          "Destination Port": "14589",
          Label: "Vimeo",
        },
        {
          No: "9",
          Source: "10.149.2.95",
          Destination: "117.147.183.170",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14589",
          "Destination Port": "443",
          Label: "Gmail",
        },
        {
          No: "10",
          Source: "10.149.2.95",
          Destination: "117.147.183.170",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14589",
          "Destination Port": "443",
          Label: "Skype",
        },
        {
          No: "11",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "443",
          "Destination Port": "14589",
          Label: "SFTPdown",
        },
        {
          No: "12",
          Source: "10.149.2.95",
          Destination: "120.253.255.33",
          Protocol: "TLSv1.2",
          Length: "55",
          "Source Port": "14588",
          "Destination Port": "443",
          Label: "ICQ",
        },
        {
          No: "13",
          Source: "120.253.255.33",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "66",
          "Source Port": "443",
          "Destination Port": "14588",
          Label: "AIM-chat",
        },
        {
          No: "14",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "93",
          "Source Port": "443",
          "Destination Port": "14632",
          Label: "SCPdown",
        },
        {
          No: "15",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "78",
          "Source Port": "443",
          "Destination Port": "14632",
          Label: "Viopbuster",
        },
        {
          No: "16",
          Source: "10.149.2.95",
          Destination: "117.147.183.170",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14632",
          "Destination Port": "443",
          Label: "TorTwitter",
        },
        {
          No: "17",
          Source: "10.149.2.95",
          Destination: "117.147.183.170",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14632",
          "Destination Port": "443",
          Label: "FaceBook",
        },
        {
          No: "18",
          Source: "117.147.183.170",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "443",
          "Destination Port": "14632",
          Label: "FaceBook",
        },
        {
          No: "19",
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "Youtube",
        },
        {
          No: "20",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "Gmail",
        },
      ],
    };
  },
  methods: {
    _getTable() {
      getTable().then((resp) => {
        if (this.isDev) {
          let movieData = resp.data.real.data.detail;
          this.updateTime = resp.data.real.data.total.message;
          this.formatMovieData(movieData);
          this.tableData = movieData;
          this.tableDataLoading = false;
        } else {
          resp.data = JSON.parse(resp.data);
          let movieData = resp.data.real.data.detail;
          this.updateTime = resp.data.real.data.total.message;
          this.formatMovieData(movieData);
          this.tableData = movieData;
          this.tableDataLoading = false;
        }
      });
    },
    updateData() {
      this.refresh = false;
      this.percentageNum = 0;
      this.timer = window.setInterval(() => {
        if (this.percentageNum < 100) {
          this.percentageNum += 1;
        } else {
          this.percentageNum = 100;
          this.refresh = true;
          window.clearInterval(this.timer);
        }
      }, 100);
      this.tableDataLoading = true;
      this._getTable();
    },
    handleDownload() {
      this.downloadLoading = true;
      import("@/vendor/Export2Excel").then((excel) => {
        const tHeader = [
          "影片",
          "上映天数",
          "累计票房",
          "实时票房",
          "票房占比",
          "排片占比",
          "上座率",
          "排座占比",
          "场次",
          "人次",
          "场均人次",
          "场均收入",
          "平均票价",
        ];
        const filterVal = [
          "影片",
          "上映天数",
          "累计票房",
          "实时票房",
          "票房占比",
          "排片占比",
          "上座率",
          "排座占比",
          "场次",
          "人次",
          "场均人次",
          "场均收入",
          "平均票价",
        ];
        const list = this.tableData;
        const data = this.formatJson(filterVal, list);
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: this.filename || "excel-list",
          autoWidth: this.autoWidth,
        });
        this.downloadLoading = false;
      });
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map((v) =>
        filterVal.map((j) => {
          if (j === "影片") {
            return v["movieName"];
          } else {
            return v[j];
          }
        })
      );
    },
    formatMovieData(srcData) {
      srcData.forEach((item) => {
        Object.values(item.attribute).forEach((v, i, t) => {
          // console.log(v)
          item[v.attrName] = v.attrValue;
        });
      });
    },
  },
  created() {
    this.isDev = process.env.NODE_ENV === "development";
    this.tableDataLoading = false;
    this._getTable();
  },
};
</script>

<style scoped lang="stylus">
.fresh
  float right
  width 350px
  text-align right
  padding-right 20px

.bottomInfo
  height 100px
  padding 20px
  font-size 20px
  display flex
  justify-content space-around
  align-items center
</style>
