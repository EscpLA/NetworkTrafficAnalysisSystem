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

    <div class="bottomInfo">
      <div class="modelBox">检测模型：xxxxxx模型</div>
      <div class="buttonBox">
        <el-button type="primary" size="medium" icon="el-icon-search" style="padding: 20px 30px; font-size: 20px;" >开始检测</el-button>
      </div>
    </div>
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
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "FaceBook",
        },
        {
          No: "2",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "Gmail",
        },
        {
          No: "8",
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "FaceBook",
        },
        {
          No: "9",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "ICQ",
        },
        {
          No: "10",
          Source: "10.149.2.95",
          Destination: "121.194.11.72",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14796",
          "Destination Port": "443",
          Label: "Netflix",
        },
        {
          No: "13",
          Source: "13.107.21.239",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "443",
          "Destination Port": "14714",
          Label: "Gmail",
        },
        {
          No: "16",
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "Youtube",
        },
        {
          No: "17",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "ICQ",
        },
        {
          No: "22",
          Source: "106.39.206.82",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "14000",
          "Destination Port": "14803",
          Label: "Netflix",
        },
        {
          No: "23",
          Source: "10.149.2.95",
          Destination: "106.39.206.82",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14803",
          "Destination Port": "14000",
          Label: "AIM-chat",
        },
        {
          No: "25",
          Source: "10.149.2.95",
          Destination: "51.104.15.252",
          Protocol: "TLSv1.2",
          Length: "139",
          "Source Port": "14102",
          "Destination Port": "443",
          Label: "Email",
        },
        {
          No: "26",
          Source: "10.149.2.95",
          Destination: "51.104.15.252",
          Protocol: "TLSv1.2",
          Length: "93",
          "Source Port": "14102",
          "Destination Port": "443",
          Label: "Skype",
        },
        {
          No: "27",
          Source: "10.149.2.95",
          Destination: "51.104.15.252",
          Protocol: "TLSv1.2",
          Length: "893",
          "Source Port": "14102",
          "Destination Port": "443",
          Label: "Vimeo",
        },
        {
          No: "28",
          Source: "121.194.10.213",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "117",
          "Source Port": "443",
          "Destination Port": "14435",
          Label: "Hangout",
        },
        {
          No: "29",
          Source: "10.149.2.95",
          Destination: "121.194.10.213",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14435",
          "Destination Port": "443",
          Label: "SCPdown",
        },
        {
          No: "30",
          Source: "51.104.15.252",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "443",
          "Destination Port": "14102",
          Label: "SFTPdown",
        },
        {
          No: "34",
          Source: "51.104.15.252",
          Destination: "10.149.2.95",
          Protocol: "TCP",
          Length: "60",
          "Source Port": "443",
          "Destination Port": "14102",
          Label: "TorTwitter",
        },
        {
          No: "38",
          Source: "51.104.15.252",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "93",
          "Source Port": "443",
          "Destination Port": "14102",
          Label: "Email",
        },
        {
          No: "39",
          Source: "10.149.2.95",
          Destination: "51.104.15.252",
          Protocol: "TCP",
          Length: "54",
          "Source Port": "14102",
          "Destination Port": "443",
          Label: "Spotify",
        },
        {
          No: "44",
          Source: "51.104.15.252",
          Destination: "10.149.2.95",
          Protocol: "TLSv1.2",
          Length: "153",
          "Source Port": "443",
          "Destination Port": "14102",
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
.container
  background-color #f4f7fc
  padding-bottom: 80px
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
