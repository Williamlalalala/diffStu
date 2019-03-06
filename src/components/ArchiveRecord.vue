<template>
     <div>
       <div v-if="sign === 1 ">
        <!--建档历史列表-->
        <el-table :data="history_records" height="580">
          <el-table-column v-for="(record,index) in history_table_attitude" :key="index" :prop="record.prop" :label="record.label">
          </el-table-column>
          <el-table-column fixed="right" label="操作" width="190">
            <div slot-scope="scope">
              <el-tooltip class="item" effect="dark" content="更改记录人" placement="top-end">
                <el-button @click="preChClick(scope.row)" size="small" icon="el-icon-document
              "></el-button>
              </el-tooltip>

              <el-tooltip class="item" effect="dark" content="修改记录" placement="top-start">
                <el-button @click="OpClick(scope.row)" icon="el-icon-edit
              " size="small"></el-button>
              </el-tooltip>
            </div>
          </el-table-column>
        </el-table>
        </div>
        <div class="innerOp" v-else-if="sign === 2">
          <i class="el-icon-back" @click="sign = 1"></i>
          姓名：{{mockStuName}}
          <el-tabs v-model="activeName" @tab-click="handleClick">
            <el-tab-pane label="联系简易记录表" name="first">
              联系简易记录表
            </el-tab-pane>
            <el-tab-pane label="面谈记录表" name="second">
              面谈记录表
            </el-tab-pane>
            <el-tab-pane label="家长联系记录表" name="third">家长联系记录表</el-tab-pane>
            <el-tab-pane label="研讨及总结记录" name="fourth">研讨及总结记录</el-tab-pane>
            <el-tab-pane label="学生档案" name="fifth">学生档案</el-tab-pane>
          </el-tabs>
          <el-table :data="recordTable" v-if="table_sign === 1">
            <el-table-column type="expand">
                <template slot-scope="props">
                  <el-form label-position="left" inline class="demo-table-expand">
                    <el-form-item label="访谈内容">
                      <span>{{ props.row.content }}</span>
                    </el-form-item>
                  </el-form>
                </template>
              </el-table-column>
              <el-table-column style="max-height: 94px;overflow: auto" v-for="data in record_table" :prop="data.prop" :label="data.label">
            </el-table-column>
            <el-table-column fixed="right" label="操作" width="160">
              <el-button @click=" preChClick(scope.row)" type="text" size="small">删除记录</el-button>
            </el-table-column>
          </el-table>
          
          <!--此处新增-->
          <el-col v-if="table_sign === 2">

            <div>
              <el-form :inline="true" :model="stuDetailInf" class="demo-form-inline" label-width="120px">
                <el-form-item v-for="(item,index) in input_data" :key="index" :label="item.label">
                  <el-input v-model="stuDetailInf[item.code]" :placeholder="item.label"></el-input>
                </el-form-item>
                <el-form-item label="帮扶类型">
                  <el-input v-model="stuDetailInf.helpType" placeholder="帮扶类型"></el-input>
                </el-form-item>
                <el-form-item label="关注类型">
                  <el-input v-model="stuDetailInf.attentionType" placeholder="关注类型"></el-input>
                </el-form-item>
              </el-form>
            </div>
            <div style="position:reletive;left:10%;">
              <el-input
                width="100%"
                type="textarea"
                :autosize="{ minRows: 3, maxRows: 5}"
                placeholder="请输入内容"
                v-model="textarea">
              </el-input>
            </div>
            <el-button style="margin:10px;" type="success" @click="modifyStuInf()" icon="el-icon-check" circle></el-button>
            <el-button style="margin:10px;" type="danger" @click="deleteStuInf()" icon="el-icon-delete" circle></el-button>
          </el-col>
          <el-button type="primary" @click="changeAddStuStatus" v-if="table_sign === 1" >新增</el-button>
          <el-dialog title="新增记录" :visible.sync="addStuInfFormFlag" >
            <el-form style="background-color:#fcfcfc;padding:10px" ref="addStuInfForm" :model="addStuInfForm" label-width="80px">
            <el-form-item label="学生">
              <el-input v-model="addStuInfForm.studentId"></el-input>
            </el-form-item>
            <el-form-item label="时间">
              <el-col :span="20">
                <el-date-picker type="date" placeholder="选择日期" v-model="addStuInfForm.time" style="width: 100%;"></el-date-picker>
              </el-col>
            </el-form-item>
            <el-form-item label="见证人">
              <el-input v-model="addStuInfForm.witness"></el-input>
            </el-form-item>
            <el-form-item label="记录人">
              <el-input v-model="addStuInfForm.recorder"></el-input>
            </el-form-item>
            <el-form-item label="方式">
              <el-select v-model="addStuInfForm.way" placeholder="请选择访谈方式">
                <el-option label="面谈" value="面谈"></el-option>
                <el-option label="电话" value="电话"></el-option>
                <el-option label="网上聊天" value="网上聊天"></el-option>
                <el-option label="其他" value="其他"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="地点">
              <el-select v-model="addStuInfForm.location" placeholder="请选择访谈地点">
                <el-option label="办公室" value="办公室"></el-option>
                <el-option label="寝室" value="寝室"></el-option>
                <el-option label="教学楼" value="教学楼"></el-option>
                <el-option label="其他" value="其他"></el-option>
              </el-select>
            </el-form-item>
            
            <el-form-item label="内容">
              <el-input type="textarea" v-model="addStuInfForm.content"></el-input>
            </el-form-item>
            <el-form-item label="评论">
              <el-input type="textarea" v-model="addStuInfForm.comment"></el-input>
            </el-form-item>
            <el-form-item style="margin-bottom:10px">
              <el-button type="primary" @click="createNewRecord">立即创建</el-button>
              <el-button @click="addStuInfFormFlag = false">取消</el-button>
            </el-form-item>
          </el-form>
         </el-dialog>
        </div>
        <!--change recorder dialog-->
        <el-dialog title="更改记录人" :visible.sync="dialogChangePeople">
          <el-form :model="changePeopleInf">
            <el-form-item label="更改原因" :label-width="formLabelWidth">
              <el-input v-model="changePeopleInf.reson" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="更改人" :label-width="formLabelWidth">
                <el-input v-model="changePeopleInf.people" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogChangePeople = false">取 消</el-button>
            <el-button type="primary" @click=" ChClick()">确 定</el-button>
          </div>
        </el-dialog>     
      </div>
      
</template>

<script>
export default {
  data() {
    return {
      ip:'http://211.83.111.247:8082',
      sign:1,
      //进入之后的列表显示问题
      table_sign:2,
      activeName: 'fifth',
      mockStuName:"",
      teacherId:"",
      mockStuId:"",
      textarea:"",
      dialogChangePeople:false,
      formLabelWidth: "100px",
      //进入之后的
      //记录列表
      history_records:[],
      recordTable:[],
      formInline: {
        college: "",
        grade: "",
        major: "",
        class: "",
        identity: '',
        name: "",
        studentID: "",
        province: "",
        nation: "",
        sex: ""
      },
      addStuInfFormFlag:false,
      addStuInfForm:{},
      detailTableFlag:0,
      changePeopleInf:{
        reson:"",
        people:""
      },
      input_data:[
        {label:'学号',code:"studentId"},{label:'性别',code:'sex'},
        {label:'姓名',code:'name'},{label:'专业',code:'major'},{label:'年级',code:'grade'},
        {label:'班级',code:'studentClass'},{label:'政治面貌',code:'politicalStatus'},{label:'民族',code:'ethnicGroup'},
        {label:'职务',code:'duty'},{label:'宿舍',code:'dormitory'},{label:'籍贯',code:'birthOrigin'},
        {label:'家庭详细地址',code:'familyAddress'},{label:'联系方式',code:'contactWay'},{label:'家庭电话',code:'familyTelNumber'},
        {label:'父亲电话',code:'fatherTelNumber'},{label:'母亲电话',code:'motherTelNumber'},{label:'家庭状况',code:'familyCondition'},
        {label:'学习状况',code:'studyCondition'},{label:'健康状况',code:'healthCondition'},{label:'生活状况',code:'lifeCondition'},
        {label:'其他情况',code:'otherCondition'},{label:'创建基本情况',code:'bulidingBasis'},{label:'创建记录人',code:'bulidingRecorder'},
        {label:'创建时间',code:'bulidingTime'},{label:'创建人',code:'bulidingPerson'},{label:'创建人职责',code:'bulidingPersonDuty'}
      ],
      record_table: [
        /*{
          recordId: "2016",
          recordName: "陈",
          recordTime: "2018-01-01",
          location: "",
          witness: "",
          recorder: "",
          participant: "",
          way: "",
          content: "",
          comment: "",
        },*/
            { label: '记录号', prop: "recordId", show: true },
            { label: '记录名', prop: "recordName",show:true},
            { label: '记录时间', prop: "recordTime", show: true },
            { label: '地点', prop: "location", show: true },
            { label: '观察者', prop: "witness", show: true },
            { label: '记录者', prop: "recorder", show: true },
            { label: '参与者', prop: "participant", show: true },
            { label: '方式', prop: "way", show: true },
            //{ label: '内容', prop: "content", show: true },
            { label: '评论', prop: "comment", show: true }
      ],
      //记录历史属性
      history_table_attitude: [
        { prop: "historyArchiveId", label: "historyArchiveId" },
        { prop: "studentId", label: "studentId" },
        { prop: "sex", label: "sex" },
        { prop: "name", label: "name" },
        { prop: "major", label: "major" },
        { prop: "grade", label: "grade" },
        { prop: "studentClass", label: "studentClass" },
        { prop: "ethnicGroup", label: "ethnicGroup" },
        { prop: "contactWay", label: "contactWay" },
        //{ prop: "familyTelNumber", label: "familyTelNumber" },
        { prop: "bulidingTime", label: "bulidingTime" },
        { prop: "helpType", label: "helpType" },
        { prop: "attentionType", label: "attentionType" },
        { prop: "destoryingTime", label: "destoryingTime" }
      ],
       stuDetailInf:{
        //   {
        //     "studentId":,
        //     "sex":,
        //     "name":,
        //     "major":,
        //     "grade":,
        //     "studentClass":,
        //     "politicalStatus":,
        //     "ethnicGroup":,
        //     "duty":,
        //     "dormitory":,
        //     "birthOrigin":,
        //     "familyAddress":,
        //     "contactWay":,
        //     "familyTelNumber":,
        //     "fatherTelNumber":,
        //     "motherTelNumber":,
        //     "familyCondition":,
        //     "studyCondition":,
        //     "healthCondition":,
        //     "lifeCondition":,
        //     "otherCondition":,
        //     "bulidingBasis":,
        //     "bulidingRecorder":,
        //     "bulidingTime":,
        //     "bulidingPerson":,
        //     "bulidingPersonDuty":,
        //     "helpType":,
        //     "attentionType":,
        //     "recorderChanges": [
        //         {
        //             "changeTime":,
        //             "recorderNow":,
        //             "changeReason": 
        //         }
        //     ]
        // },
      },
    }
  },
  methods: {
    handleClick(tab, event) {
      var token = sessionStorage.getItem("token");
      var that = this;
      const h = this.$createElement;
      this.detailTableFlag = tab.index;
      if(tab.index == 0){
        that.table_sign = 1;
        $.ajax({
            url: this.ip+"/newhelp/api/records/联系简易记录表/"+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (res) {
              if (res.success) {
                console.log(res);
                that.recordTable = res.data;
              } else {
                that.$notify({
                  title: '联系简易记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '联系简易记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                })
            },
        })
      }
      else if(tab.index == 1){
        that.table_sign = 1;
        $.ajax({
            url: this.ip+"/newhelp/api/records/面谈记录表/"+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (res) {
              if (res.success) {
                console.log(res);
                that.recordTable = res.data;
              } else {
                that.$notify({
                  title: '面谈记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '面谈记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                })
            },
        })
      }else if(tab.index == 2){
        that.table_sign = 1;
        $.ajax({
            url: this.ip+"/newhelp/api/records/家长联系记录表/"+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (res) {
              if (res.success) {
                that.recordTable = res.data;
                console.log(res.data);
              } else {
                that.$notify({
                  title: '家长联系记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '家长联系记录表',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                })
            },
        })
      }else if(tab.index == 3){
        that.table_sign = 1;
        $.ajax({
            url: this.ip+"/newhelp/api/records/研讨及总结记录/"+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (res) {
              if (res.success) {
                console.log(res);
                that.recordTable = res.data;
              } else {
                that.$notify({
                  title: '研讨及总结记录',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '研讨及总结记录',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                })
            },
        })
      }else if(tab.index == 4){
        that.table_sign = 2;
          $.ajax({
            //{studentId}
            url: this.ip+'/newhelp/api/archiveStudent/'+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            data: '',
            success: function (res) {
              if (res.success) {
                that.stuDetailInf = res.data;
                that.recordChanges = that.stuDetailInf.recorderChanges;
              } else {
                that.$notify({
                  title: '个人信息',
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '个人信息',
                  message: h('i', { style: 'color: teal'}, 'API错误')
                })
            },
        })
      }
      console.log(tab, event);
    },
    preChClick:function(row)  {
      this.dialogChangePeople = true;
      this.mockStuId = row.studentId;
    },
    changeAddStuStatus:function(){
      this.addStuInfForm.studentId = this.mockStuId;
      this.addStuInfForm.recorder = this.teacherId;
      this.addStuInfFormFlag = true;
    },
    OpClick:function(row){
      var token = sessionStorage.getItem("token");
      this.mockStuName = row.name;
      this.mockStuId = row.studentId;
      this.sign = 2;
      const h = this.$createElement;
      var that = this;
      //获取这个学生的基本信息
      $.ajax({
        //{studentId}
        url: this.ip+'/newhelp/api/archiveStudent/'+this.mockStuId,
        type: "GET",
        beforeSend: function (request) {
            request.setRequestHeader("Authorization", token);
        },
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        data: "",
        success: function (res) {
          if (res.success) {
            that.stuDetailInf = res.data;
            that.recordChanges = that.stuDetailInf.recorderChanges;
          } else {
            console.log(res);
            that.$notify({
              title: '学生信息',
              message: h('i', { style: 'color: teal'}, '请重新登录')
            });
          }
        },error: function () {
          that.$notify({
              title: '学生信息',
              message: h('i', { style: 'color: teal'}, 'API错误')
            })
        },
      })
    },
    createNewRecord:function(){
      var that = this;
      var token = sessionStorage.getItem('token');
      const h = this.$createElement;
      this.dialogChangePeople = false
      switch(that.detailTableFlag){
        case '0':that.addStuInfForm.recordName = "联系简易记录表";
          break;
        case '1':that.addStuInfForm.recordName = "面谈记录表";
          break;
        case '2':that.addStuInfForm.recordName = "家长联系记录表";
          break;
        case '3':that.addStuInfForm.recordName = "研讨及总结记录";
          break;
        default: break;
      }
      $.ajax({
        url: that.ip+"/newhelp/api/record",
        type: "POST",
        beforeSend: function (request) {
          request.setRequestHeader("Authorization", token);
        },
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        data: JSON.stringify(that.addStuInfForm),
        success: function (res) {
          if (res.success) {
            console.log('成功');
          } else {
            console.log(res);
            that.$notify({
              title: '发送命令成功',
              message: h('i', { style: 'color: teal'}, '变更失败')
            });
          }
        },
        error: function (status) {}
      });
       $.ajax({
            url: this.ip+"/newhelp/api/records/"+that.addStuInfForm.recordName+"/"+this.mockStuId,
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json; charset=utf-8",
            dataType: "json",
            success: function (res) {
              if (res.success) {
                console.log(res);
                that.recordTable = res.data;
              } else {
                that.$notify({
                  title: that.addStuInfForm.recordName,
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: that.addStuInfForm.recordName,
                  message: h('i', { style: 'color: teal'}, '请重新登录')
                })
            },
        })

    },
    modifyStuInf:function(){
      var that =this;
      const h = this.$createElement;
      var token = sessionStorage.getItem('token');
      $.ajax({
        //{newTeacherId}
        url: that.ip+"/newhelp/api/archiveStudent",
        type: "PUT",
        beforeSend: function (request) {
          request.setRequestHeader("Authorization", token);
        },
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        data: JSON.stringify(this.stuDetailInf),
        success: function (res) {
          if (res.success) {
            console.log('成功');
          } else {
            console.log(res);
            that.$notify({
              title: '发送命令成功',
              message: h('i', { style: 'color: teal'}, '变更失败')
            });
          }
        },
        error: function (status) {
          that.$notify({
              title: status,
              message: h('i', { style: 'color: teal'}, '变更失败')
            });
        },
      })
    },
    deleteStuInf:function(){
      var that =this;
      var token = sessionStorage.getItem('token');
      const h = this.$createElement;
      $.ajax({
          url: that.ip+"/newhelp/api/archiveStudent",
          type: "DELETE",
          contentType: "application/json",
          beforeSend: function (request) {
            request.setRequestHeader("Authorization", token);
          },
          dataType: "json",
          data: JSON.stringify({studentId: that.mockStuId,
                teacherId:"test",
                destoryingRecorder:sessionStorage.getItem('userName'),
                destoryingBasis:'',
                destoryingTime:''}),
          success: function (res) {
            if (res.success) {
              console.log('成功');
            } else {
              console.log(res);
              that.$notify({
                title: '发送命令成功',
                message: h('i', { style: 'color: teal'}, '变更失败')
              });
            }
          },
          error: function (status) {
            that.$notify({
                title: status,
                message: h('i', { style: 'color: teal'}, '变更失败')
              });
          },
      })
    },
    ChClick:function(){
      var that = this;
      this.dialogChangePeople = false
      var token = sessionStorage.getItem('token');
      const h = this.$createElement;
      $.ajax({
          //{newTeacherId}
          url: that.ip+"/newhelp/api/change/"+ that.changePeopleInf.people,
          type: "POST",
          beforeSend: function (request) {
            request.setRequestHeader("Authorization", token);
          },
          contentType: "application/json; charset=utf-8",
          dataType: "json",
          data: JSON.stringify({studentId: that.mockStuId,
                changeReason: that.changePeopleInf.reson}),
          success: function (res) {
            if (res.success) {
              console.log('成功');
              that.record_table = res.data.data;
            } else {
              console.log(res);
              that.$notify({
                title: '发送命令成功',
                message: h('i', { style: 'color: teal'}, '变更失败')
              });
            }
          },
          error: function (status) {
            that.$notify({
                title: status,
                message: h('i', { style: 'color: teal'}, '变更失败')
              });
            },
        })
      }
    },
    created() {
      let token = sessionStorage.getItem('token');
      this.teacherId = sessionStorage.getItem('userName');
      var that = this;
      //7、查看历史帮扶学生列表
      $.ajax({
        url: this.ip + "/newhelp/api/historyArchives/" + that.teacherId,
        type: "GET",
        beforeSend: function (request) {
          request.setRequestHeader("Authorization", token);
        },
        contentType: false,
        async:false,
        contentType: false,
        processData: false,
        dataType: "json",
        success: function (data) {
          if (data.success) {
            that.history_records = data.data;
            console.log(data.data);
          }
          else {
            alert(data.message);
          }
        },
        error: function (data) {
          console.log("无法获取历史信息");
        },
      });
      console.log(that.history_records);
    },
    mounted() {
    },
}
</script>

<style scoped>
.search-btn {
  background-color: white;
  font-size: 20px;
  width: 100%;
  height: 50px;
  line-height: 50px;
  margin-bottom: 10px;
}
.innerOp{
  margin: 20px;
}
.el-icon-back{
  width: 30px;
  height: 30px;;
}
</style>