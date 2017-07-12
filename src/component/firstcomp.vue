<template>
  <div>
    
    <el-table :data="tableData" stripe style="width: 75%;margin:auto auto;text-align:center" align="center">
      <el-table-column prop="id" label="ID" width="150" align="center"> </el-table-column>
      <el-table-column prop="region" label="位置" width="70" align="center"> </el-table-column>
      <el-table-column prop="describ" label="描述" width="200" align="center"> </el-table-column>
      <el-table-column prop="isOnline" label="状态" width="100" align="center">
      <template scope="scope">
        <el-tag
          :type="scope.row.isOnline === '未上线' ? 'primary' : 'success'"
          close-transition>{{scope.row.isOnline}}</el-tag>
      </template>
    </el-table-column>
      <el-table-column prop="date1str" label="开始时间" width="110" align="center"> </el-table-column>
      <el-table-column prop="date2str" label="结束时间" width="110" align="center"> </el-table-column>
      <el-table-column label="操作" align="center">
        <template scope="scope">
          <el-button @click="codeClick(scope.$index)" type="warning" size="mini"><i class="el-icon-document el-icon--left"></i>代码</el-button>
          <el-button @click="fixClick(scope.$index)" type="info" size="mini"><i class="el-icon-edit el-icon--left"></i>编辑</el-button>
          <el-button @click="open2(scope.$index)" type="danger" size="mini"><i class="el-icon-delete el-icon--left"></i>删除</el-button>
          <el-button @click="turnIsOline(scope.$index)" :type="scope.row.isOnline === '未上线' ? 'primary' : 'success'" size="mini">切换状态</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="width: 70%;margin:20px auto;">
      <el-button @click="addClick" type="info"><i class="el-icon-plus el-icon--left"></i>添加一条广告</el-button>
    </div>


    <!--编辑表单-->
    <transition name="el-zoom-in-top">
    <div id="fixdiv" v-if="fixdiv" class="transition-box fixdiv">
      
      <el-form :rules="rules" ref="fixform" :model="tableData[i]" label-width="80px">
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
            <el-form-item>
              <el-date-picker type="date" placeholder="选择日期" v-model="tableData[i].date1" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-col>
          <el-col class="line" :span="2" style="text-align: center">-</el-col>
          <el-col :span="11">
            <el-form-item>
              <el-date-picker type="date" placeholder="选择日期" v-model="tableData[i].date2" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item label="广告代码">
          <el-input type="textarea" v-model="tableData[i].code"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="fixoff(i,'fixform')">保存</el-button>
        </el-form-item>
      </el-form>
    </div>
    </transition>
    <!--/编辑表单-->


    <!--添加表单-->
    <transition name="el-zoom-in-top">
    <div id="adddiv" v-if="adddiv" class="transition-box fixdiv">
      
      <el-form :rules="rules" ref="ruleForm" :model="form" label-width="80px">
        <el-form-item label="广告位置" prop="region">
          <el-select v-model="form.region" placeholder="请选择活动区域" style="width:100%">
            <el-option label="位置一" value="1"></el-option>
            <el-option label="位置二" value="2"></el-option>
            <el-option label="位置三" value="3"></el-option>
            <el-option label="位置四" value="4"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="广告描述">
          <el-input v-model="form.describ"></el-input>
        </el-form-item>
        <el-form-item label="广告状态" prop="isOnline">
          <el-select v-model="form.isOnline" placeholder="请选择是否上线" style="width:100%">
            <el-option label="已上线" value="已上线"></el-option>
            <el-option label="未上线" value="未上线"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="活动时间">
          <el-col :span="11">
            <el-form-item>
              <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-col>
          <el-col class="line" :span="2" style="text-align: center">-</el-col>
          <el-col :span="11">
            <el-form-item>
              <el-date-picker type="date" placeholder="选择日期" v-model="form.date2" format="yyyy-MM-dd" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-col>
        </el-form-item>
        <el-form-item label="广告模式">
          <el-radio-group v-model="form.resource">
            <el-radio label="图片"></el-radio>
            <el-radio label="代码"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="广告代码">
          <el-input :rows="4" type="textarea" v-model="form.code"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="addoff('ruleForm')">保存</el-button>
          <el-button @click="addcannel">关闭</el-button>
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
          <el-input :rows="8" type="textarea" readonly="readonly" v-model="tableData[i].code"></el-input>
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
        form : {
          id: '',
          region: '',
          describ: '',
          isOnline: '',
          date1: '',
          date2: '',
          date1str: '',
          date2str: '',
          resource: '',
          code: '',
        },
        tableData: [],
        rules: {
          
          region: [
            { required: true, message: '请选择活动区域1', trigger: 'change' }
          ],
          isOnline: [
            { required: true, message: '请选择活动区域1', trigger: 'change' }
          ],
          date1: [
            { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
          ],
          date2: [
            { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
          ]
        }
      }
    },
    mounted: function() {    //ready
      this.tableData = [{
          id: '1231321321321',
          region: '1',
          describ: '主页大图2',
          isOnline: '已上线',
          date1: '2017-07-10',
          date2: '2017-07-10',
          date1str: '2017-07-10',
          date2str: '2017-07-10',
          resource: '图片',
          code: '1111111111'
        }, {
          id: '1231321321322',
          region: '2',
          describ: '主页大图3',
          isOnline: '未上线',
          date1: '2017-07-10',
          date2: '2017-07-10',
          date1str: '2017-07-10',
          date2str: '2017-07-10',
          resource: '图片',
          code: '22222222'
        }, {
          id: '1231321321323',
          region: '3',
          describ: '主页大图4',
          isOnline: '已上线',
          date1: '2017-07-10',
          date2: '2017-07-10',
          date1str: '2017-07-10',
          date2str: '2017-07-10',
          resource: '图片',
          code: '3333333333'
        }, {
          id: '1499662971866',
          region: '4',
          describ: '主页大图5',
          isOnline: '未上线',
          date1: '2017-07-10',
          date2: '2017-07-10',
          date1str: '2017-07-10',
          date2str: '2017-07-10',
          resource: '图片',
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
      addcannel(){
        this.adddiv = false;
        this.form.id = "";
        this.form.region = "";
        this.form.date1 = "";
        this.form.date2 = "";
        this.form.date1str = "";
        this.form.date2str = "";
        this.form.describ = "";
        this.form.isOnline = "";
        this.form.resource = '',
        this.form.code = "";
      },
      fixClick(x) {
        var that = this;
        this.i = x;
        this.fixdiv = true;
      },
      fixoff(x,formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var that = this;
            this.fixdiv = false;
            that.tableData[x].date1str = ''
            that.tableData[x].date2str = ''
            if(that.tableData[x].date1 !== ''){that.tableData[x].date1str = that.tableData[x].date1.format("yyyy-MM-dd")};
            if(that.tableData[x].date2 !== ''){that.tableData[x].date2str = that.tableData[x].date2.format("yyyy-MM-dd")};
        } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      addoff(formName){
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var that = this;
            this.adddiv = false;
            if(that.form.date1 !== ''){that.form.date1str = that.form.date1.format("yyyy-MM-dd")};
            if(that.form.date2 !== ''){that.form.date2str = that.form.date2.format("yyyy-MM-dd")};
            this.tableData.push({
              id: (new Date()).valueOf(),
              region: that.form.region,
              describ: that.form.describ,
              isOnline: that.form.isOnline,
              code: that.form.code,
              date1: that.form.date1,
              date2: that.form.date2,
              date1str: that.form.date1str,
              date2str: that.form.date2str,
              resource: that.form.resource
            });
            this.form.id = "";
            this.form.region = "";
            this.form.date1 = "";
            this.form.date2 = "";
            this.form.date1str = "";
            this.form.date2str = "";
            this.form.describ = "";
            this.form.isOnline = "";
            this.form.code = "";
            this.form.resource = "";
          } else {
            console.log('error submit!!');
            return false;
          }
        });
        
        
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
      turnIsOline(x){
        if(this.tableData[x].isOnline == "已上线"){this.tableData[x].isOnline = "未上线"}else{this.tableData[x].isOnline = "已上线"};
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