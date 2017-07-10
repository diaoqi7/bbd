<template>
  <div>
    
    <el-table :data="tableData" stripe style="width: 70%;margin:auto auto;text-align:center" align="center">
      <el-table-column prop="id" label="ID" width="150" align="center"> </el-table-column>
      <el-table-column prop="region" label="位置" width="100" align="center"> </el-table-column>
      <el-table-column prop="describ" label="描述" width="200" align="center"> </el-table-column>
      <el-table-column prop="isOnline" label="状态" width="130" align="center">
      <template scope="scope">
        <el-tag
          :type="scope.row.isOnline === '未上线' ? 'primary' : 'success'"
          close-transition>{{scope.row.isOnline}}</el-tag>
      </template>
    </el-table-column>
      <el-table-column prop="adtime" label="时间" width="250" align="center"> </el-table-column>
      <el-table-column label="操作" align="center">
        <template scope="scope">
          <el-button @click="codeClick(scope.$index)" type="text" size="small">代码</el-button>
          <el-button @click="fixClick(scope.$index)" type="text" size="small">编辑</el-button>
          <el-button @click="open2(scope.$index)" type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="width: 70%;margin:20px auto;">
      <el-button @click="addClick" type="info">添加一条广告</el-button>
    </div>


    <!--编辑表单-->
    <transition name="el-zoom-in-top">
    <div id="fixdiv" v-if="fixdiv" class="transition-box fixdiv">
      
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="广告ID">
          <el-input v-model="tableData[i].id" readonly="true" style="background-color: #fff"></el-input>
        </el-form-item>
        <el-form-item label="广告位置">
          <el-select v-model="tableData[i].region" placeholder="请选择活动区域" style="width:100%">
            <el-option label="位置一" value="1"></el-option>
            <el-option label="位置二" value="2"></el-option>
            <el-option label="位置三" value="3"></el-option>
            <el-option label="位置四" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="广告描述">
          <el-input v-model="tableData[i].describ"></el-input>
        </el-form-item>
        <el-form-item label="广告状态">
          <el-select v-model="tableData[i].isOnline" placeholder="请选择是否上线" style="width:100%">
            <el-option label="已上线" value="已上线"></el-option>
            <el-option label="未上线" value="未上线"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="活动时间">
          <el-col :span="11">
            <el-date-picker type="date" @change="fixdate1func(i)" placeholder="选择日期" v-model="tableData[i].date1" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
          </el-col>
          <el-col class="line" :span="2" style="text-align: center">-</el-col>
          <el-col :span="11">
            <el-date-picker type="date" @change="fixdate2func(i)" placeholder="选择日期" v-model="tableData[i].date2" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="广告代码">
          <el-input type="textarea" v-model="tableData[i].code"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="fixoff(i)">保存</el-button>
        </el-form-item>
      </el-form>
    </div>
    </transition>
    <!--/编辑表单-->


    <!--添加表单-->
    <transition name="el-zoom-in-top">
    <div id="adddiv" v-if="adddiv" class="transition-box fixdiv">
      
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="广告位置">
          <el-select v-model="region" placeholder="请选择活动区域" style="width:100%">
            <el-option label="位置一" value="1"></el-option>
            <el-option label="位置二" value="2"></el-option>
            <el-option label="位置三" value="3"></el-option>
            <el-option label="位置四" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="广告描述">
          <el-input v-model="describ"></el-input>
        </el-form-item>
        <el-form-item label="广告状态">
          <el-select v-model="isOnline" placeholder="请选择是否上线" style="width:100%">
            <el-option label="已上线" value="已上线"></el-option>
            <el-option label="未上线" value="未上线"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="活动时间">
          <el-col :span="11">
            <el-date-picker type="date" @change="date1func" placeholder="选择日期" v-model="date1" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
          </el-col>
          <el-col class="line" :span="2" style="text-align: center">-</el-col>
          <el-col :span="11">
            <el-date-picker type="date" @change="date2func" placeholder="选择日期" v-model="date2" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="广告代码">
          <el-input type="textarea" v-model="code"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="addoff">保存</el-button>
        </el-form-item>
      </el-form>
    </div>
    </transition>
    <!--/添加表单-->



    <!--代码模块-->
    <transition name="el-zoom-in-top">
    <div id="codediv" v-if="codediv" class="transition-box fixdiv">
      
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="广告代码">
          <el-input type="textarea" readonly="readonly" v-model="tableData[i].code"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="codeoff">关闭</el-button>
        </el-form-item>
      </el-form>
    </div>
    </transition>
    <!--/代码模块-->



  </div>
</template>

<script>
  export default {
    data() {
      return {
        i : 0,
        fixdiv : false,
        adddiv : false,
        codediv : false,
        id: '',
        region: '',
        describ: '',
        isOnline: '',
        adtime: '',
        date1: '',
        date2: '',
        code: '',
        tableData: [],
      }
    },
    mounted: function() {    //ready
      this.tableData = [{
          id: '1231321321321',
          region: '1',
          describ: '主页大图2',
          isOnline: '已上线',
          adtime: '2017.06.21 - 2017.07.10',
          date1: '2017-06-21',
          date1str: '2017-06-21',
          date2: '2017-07-10',
          date2str: '2017-07-10',
          code: '1111111111'
        }, {
          id: '1231321321322',
          region: '2',
          describ: '主页大图3',
          isOnline: '未上线',
          adtime: '2017.06.21 - 2017.07.10',
          date1: '2017-06-21',
          date1str: '2017-06-21',
          date2: '2017-07-10',
          date2str: '2017-07-10',
          code: '22222222'
        }, {
          id: '1231321321323',
          region: '3',
          describ: '主页大图4',
          isOnline: '已上线',
          adtime: '2017.06.21 - 2017.07.10',
          date1: '2017-06-21',
          date1str: '2017-06-21',
          date2: '2017-07-10',
          date2str: '2017-07-10',
          code: '3333333333'
        }, {
          id: '1231321321324',
          region: '4',
          describ: '主页大图5',
          isOnline: '未上线',
          adtime: '2017.06.21 - 2017.07.10',
          date1: '2017-06-21',
          date1str: '2017-06-21',
          date2: '2017-07-10',
          date2str: '2017-07-10',
          code: '4444444444'
        }]
    },
    methods: {
      handleClick() {

      },
      handlePreview(file) {
        console.log(file);
      },
      onSubmit() {
        console.log('submit!');
      },
      fixClick(x) {
        var that = this;
        this.i = x;
        this.fixdiv = true;
      },
      fixoff(x) {
        var that = this;
        this.fixdiv = false;
        
      },
      addoff(){
        var that = this;
        this.adddiv = false;
        this.adtime = this.date1 + " - " + this.date2;
        this.tableData.push({
          id: (new Date()).valueOf(),
          region: that.region,
          describ: that.describ,
          isOnline: that.isOnline,
          date1str: that.date1.format("yyyy.MM.dd"),
          date2str: that.date2.format("yyyy.MM.dd"),
          adtime: that.date1.format("yyyy.MM.dd") + " - " + that.date2.format("yyyy.MM.dd"),
          code: that.code,
          date1: that.date1,
          date2: that.date2,
        });
        this.id = "";
        this.region = "";
        this.date1 = "";
        this.date2 = "";
        this.describ = "";
        this.isOnline = "";
        this.adtime = "";
        this.code = "";
        
      },
      addClick(){
        this.adddiv = true;
      },
      codeoff(){
        this.codediv = false;
      },
      codeClick(x){
        this.i = x;
        this.codediv = true;
      },
      open2(x) {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.tableData.splice(x,1);
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      fixdate1func(x){
        var that = this
        var a = this.tableData[x].date1.format("yyyy.MM.dd");
        this.tableData[x].date1str = a;
        //alert(that.tableData[x].date1str)
        this.tableData[x].adtime = this.tableData[x].date1str + " - " + this.tableData[x].date2str;
        
      },
      fixdate2func(x){
        var that = this
        var a = this.tableData[x].date2.format("yyyy.MM.dd");
        this.tableData[x].date2str = a;
        this.tableData[x].adtime = this.tableData[x].date1str + " - " + this.tableData[x].date2str;
        
      },
      filterTag(value, row) {
        return row.region === value;
      },
      filterTag2(value, row) {
        return row.isOnline === value;
      }
    }
  }
</script>
<style>
  .fixdiv{
    width:380px;
    margin:20px auto;
    border: 1px solid #d1dbe5;
    box-shadow:0 2px 4px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
    padding:20px;
    position: absolute;
    top: 100px;
    left: 35%;
    background-color: white;
    z-index: 5;
  }
</style>