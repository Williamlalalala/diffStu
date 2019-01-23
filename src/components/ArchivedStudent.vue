<template>
  <div>
      <div v-if="sign === 1 ">
        <!--查看总体的记录-->
        <el-table :data="beforStuInfTable">
          <el-table-column v-for="data in archivedStudentTable" :prop="data.prop" :label="data.label">
          </el-table-column>
          <el-table-column fixed="right" label="操作" width="160">
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
              <el-table-column v-for="data in record_table" :prop="data.prop" :label="data.label">
              </el-table-column>
              <el-table-column fixed="right" label="操作" width="160">
                <el-button @click=" preChClick(scope.row)" type="text" size="small">更改記錄人</el-button>
              </el-table-column>
        </el-table>

        <!--此处新增-->
        <el-col :span="18" v-if="table_sign === 2">

          <div class="sub-title" >
            <div><p>学号:</p><el-input  v-model="stuDetailInf.studentId"/></div>
            <div><p>性别:</p><el-input  v-model="stuDetailInf.sex"/></div>
            <div><p>姓名:</p><el-input  v-model="stuDetailInf.name"/></div>
            <div><p>专业:</p><el-input  v-model="stuDetailInf.major"/></div>
            <div><p>年级:</p><el-input  v-model="stuDetailInf.grade"/></div>
            <div><p>班级:</p><el-input  v-model="stuDetailInf.studentClass"/></div>
            <div><p>政治面貌:</p><el-input  v-model="stuDetailInf.politicalStatus"/></div>
            <div><p>民族:</p><el-input  v-model="stuDetailInf.ethnicGroup"/></div>
            <div><p>职责:</p><el-input  v-model="stuDetailInf.duty"/></div>
            <div><p>寝室:</p><el-input  v-model="stuDetailInf.dormitory"/></div>
            <div><p>出生日期:</p><el-input  v-model="stuDetailInf.birthOrigin"/></div>
            <div><p>家庭地址:</p><el-input  v-model="stuDetailInf.familyAddress"/></div>
            <div><p>联系方式:</p><el-input  v-model="stuDetailInf.contactWay"/></div>
            <div><p>家庭电话:</p><el-input  v-model="stuDetailInf.familyTelNumber"/></div>
            <div><p>父亲电话:</p><el-input  v-model="stuDetailInf.fatherTelNumber"/></div>
            <div><p>母亲电话:</p><el-input  v-model="stuDetailInf.motherTelNumber"/></div>
            <div><p>家庭状况:</p><el-input  v-model="stuDetailInf.familyCondition"/></div>
            <div><p>学习情况:</p><el-input  v-model="stuDetailInf.studyCondition"/></div>
            <div><p>健康情况:</p><el-input  v-model="stuDetailInf.healthCondition"/></div>
            <div><p>生活情况:</p><el-input  v-model="stuDetailInf.lifeCondition"/></div>
            <div><p>其他情况:</p><el-input  v-model="stuDetailInf.otherCondition"/></div>
            <div><p>创建基本情况:</p><el-input  v-model="stuDetailInf.bulidingBasis"/></div>
            <div><p>创建记录人:</p><el-input  v-model="stuDetailInf.bulidingRecorder"/></div>
            <div><p>创建时间:</p><el-input  v-model="stuDetailInf.bulidingTime"/></div>
            <div><p>创建人:</p><el-input  v-model="stuDetailInf.bulidingPerson"/></div>
            <div><p>创建人职责:</p><el-input  v-model="stuDetailInf.bulidingPersonDuty"/></div>
            <div><p>帮扶类型:</p><el-input  v-model="stuDetailInf.helpType"/></div>
            <div><p>关注类型:</p><el-input  v-model="stuDetailInf.attentionType"/></div>
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
        <el-button type="primary" @click="changeAddStuStatus" v-if="table_sign === 1" icon="el-icon-caret-bottom">新增</el-button>
        <div style="background-color:#fcfcfc;margin:10px">
          <el-form style="background-color:#fcfcfc;padding:10px" v-if="addStuInfFormFlag === 1 && table_sign === 1" ref="addStuInfForm" :model="addStuInfForm" label-width="80px">
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
              <el-button @click="addStuInfFormFlag = 0">取消</el-button>
            </el-form-item>
          </el-form>
        </div>
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
      mockStuId:"",
      textarea:"",
      dialogChangePeople:false,
      formLabelWidth: '120px',
      teacherId:"",
      addStuInfFormFlag:0,
      addStuInfForm:{},
      detailTableFlag:0,
      //进入之后的
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
      changePeopleInf:{
        reson:"",
        people:""
      },
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
            { label: '内容', prop: "content", show: true },
            { label: '评论', prop: "comment", show: true }
      ],
      allMajor: [
        "信息工程", "软件技术", "嵌入式系统", "网络安全工程", "大型主机", "信息与获取", "数字动漫", "留学生", "工业产品计算机", "数字信息处理", "互联网+", "互联网安全", "国际班", "英文授课留学生"
      ],
      allIdentities: [
        "中共党员", "共青团员", "预备党员", "积极分子", "群众"
      ],
      allProvinces: ["四川省", "河北省", "山西省", "吉林省", "辽宁省", "黑龙江省", "陕西省", "甘肃省", "青海省", "山东省",
        "福建省", "浙江省", "河南省", "湖北省", "湖南省", "江西省", "江苏省", "安徽省", "广东省", "海南省", "贵州省", "云南省",
        "北京", "上海", "重庆", "天津", "内蒙古", "新疆", "宁夏", "广西", "西藏", "台湾", "香港", "澳门"
      ],
      allNations: ["汉族", "壮族", "满族", "回族", "苗族", "维吾尔族", "土家族", "彝族", "蒙古族", "藏族", "布依族", "侗族",
        "瑶族", "朝鲜族", "白族", "哈尼族", "哈萨克族", "黎族", "傣族", "畲族", "傈僳族", "仡佬族", "东乡族", "高山族", "拉祜族",
        "水族", "佤族", "纳西族", "羌族", "土族", "仫佬族", "锡伯族", "柯尔克孜族", "达斡尔族", "景颇族", "毛南族", "撒拉族",
        "布朗族", "塔吉克族", "阿昌族", "普米族", "鄂温克族", "怒族", "京族", "基诺族", "德昂族", "保安族", "俄罗斯族", "裕固族",
        "乌孜别克族", "门巴族", "鄂伦春族", "独龙族", "塔塔尔族", "赫哲族", "珞巴族"
      ],
      //建档学生列表
      archivedStudentTable:[
            { label: '学号', prop: "studentId", show: true },
            { label: '性别', prop: "sex",show:true},
            { label: '姓名', prop: "name", show: true },
            { label: '专业', prop: "major", show: true },
            { label: '年级', prop: "grade", show: true },
            { label: '政治面貌', prop: "politicalStatus", show: true },
            { label: '班级', prop: "studentClass", show: true },
            { label: '民族', prop: "ethnicGroup", show: true },
            { label: '联系方式', prop: "contactWay", show: true },
            { label: '家庭电话', prop: "familyTelNumber", show: true },
            { label: '创建时间', prop: "bulidingTime", show: true },
            { label: '帮扶方式', prop: "helpType", show: true },
            { label: '关注类型', prop: "attentionType", show: true },
            { label: '上次记录时间', prop: "lastRecordTime", show: true },
            { label: '高亮', prop: "highlight", show: true },
      ],

      //建档学生详细信息
      stuDetailInf:{
        /*
        {
          "studentId":,
          "sex":,
          "name":,
          "major":,
          "grade":,
          "studentClass":,
          "politicalStatus":,
          "ethnicGroup":,
          "duty":,
          "dormitory":,
          "birthOrigin":,
          "familyAddress":,
          "contactWay":,
          "familyTelNumber":,
          "fatherTelNumber":,
          "motherTelNumber":,
          "familyCondition":,
          "studyCondition":,
          "healthCondition":,
          "lifeCondition":,
          "otherCondition":,
          "bulidingBasis":,
          "bulidingRecorder":,
          "bulidingTime":,
          "bulidingPerson":,
          "bulidingPersonDuty":,
          "helpType":,
          "attentionType":,
          "recorderChanges": [
              {
                  "changeTime":,
                  "recorderNow":,
                  "changeReason": 
              }
          ]
      },
        */ 
      },




      //记录更改
      /** [
              {
                  "changeTime":,
                  "recorderNow":,
                  "changeReason": 
              }
          ] */
      recordChanges:[],
      //帮扶学生列表
      beforStuInfTable: [
        /*{
        historyArchiveId: "",
        studentId: "2016",
        sex: "男",
        name: "陈",
        major: "",
        grade: "大三",
        studentClass: "0302",
        ethnicGroup: "",
        contactWay: "电话",
        familyTelNumber: "123456",
        bulidingTime: "2018-01-01",
        helpType: "经济困难",
        attentionType: "一般关注",
        lastRecordTime: "2017-12-01"
      }*/
      ],
      //困难学生横轴属性
      straitened_stu_table: [
        { prop: "name", label: "姓名" },
        { prop: "studentId", label: "学号" }, { prop: "grade", label: "年级" }, { prop: "studentClass", label: "班级" },
        { prop: "sex", label: "性别" }, { prop: "nation", label: "民族" }, { prop: "contactWay", label: "联系方式" },
        { prop: "familyTelNumber", label: "家庭电话" }, { prop: "bulidingBasis", label: "建档类型" }, { prop: "bulidingTime", label: "建档日期" }, { prop: "attentionType", label: "关注状态" }, { prop: "destoryingTime", label: "除档时间" }
      ],
    }
  },
  created:function() {
    var that = this;
    const h = this.$createElement; 
    var token = sessionStorage.getItem("token");
    this.teacherId = sessionStorage.getItem('userName');
    $.ajax({
        //{recordName}/{studentId}
        url: this.ip+"/newhelp/api/archiveStudents/"+that.teacherId,
        type: "GET",
        beforeSend: function (request) {
            request.setRequestHeader("Authorization", token);
          },
        contentType: "application/json; charset=utf-8",
        dataType: "json",
        data: {},
        success: function (res) {
          if (res.success) {
            console.log(res.data);
            that.beforStuInfTable = res.data;
            console.log(that.beforStuInfTable);
          } else {
            that.$notify({
              title: '标题名称',
              message: h('i', { style: 'color: teal'}, '数据获取错误')
            });
          }
        },
        error: function () {
          that.$notify({
              title: '标题名称',
              message: h('i', { style: 'color: teal'}, '数据获取错误')
            })
        },
    })
  },
  methods: {
    
    /*//8、搜索历史帮扶学生列表
    findHistoryTable: function () {
      this.postData(this.ip+'/newhelp/api/historyArchives/', this.beforStuInfTable, "GET")
        .then(data => {
          console.log(data.data);
          this.data.beforStuInfTable = data.data;
        }) // JSON from `response.json()` call
        .catch(error => console.error(error));
    },*/
    handleClick(tab, event) {
      var token = sessionStorage.getItem("token");
      this.detailTableFlag = tab.index;
      const h = this.$createElement;
      var that = this;
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
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '联系简易记录表',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
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
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '面谈记录表',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
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
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '家长联系记录表',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
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
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '研讨及总结记录',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
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
            data: that.toBeSentKeywords,
            success: function (res) {
              if (res.success) {
                that.stuDetailInf = res.data;
                that.recordChanges = that.stuDetailInf.recorderChanges;
              } else {
                that.$notify({
                  title: '标题名称',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: '标题名称',
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                })
            },
        })
      }
        console.log(tab, event);
    },
    changeAddStuStatus:function(){
      this.addStuInfForm.studentId = this.mockStuId;
      this.addStuInfForm.recorder = this.teacherId;
      if(this.addStuInfFormFlag == 1){
        this.addStuInfFormFlag = 0;
      }else{
        this.addStuInfFormFlag = 1;
      }
      this.addStuInfFormFlag = 1;
    },
    OpClick:function(row){
      var token = sessionStorage.getItem("token");
      console.log(row);
      this.mockStuName = row.name;
      this.mockStuId = row.studentId;
      this.sign = 2;
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
        data: that.toBeSentKeywords,
        success: function (res) {
          if (res.success) {
            that.stuDetailInf = res.data;
            that.recordChanges = that.stuDetailInf.recorderChanges;
          } else {
            that.$notify({
              title: '标题名称',
              message: h('i', { style: 'color: teal'}, '数据获取错误')
            });
          }
        },
        error: function () {
          that.$notify({
              title: '标题名称',
              message: h('i', { style: 'color: teal'}, '数据获取错误')
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
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
                });
              }
            },
            error: function () {
              that.$notify({
                  title: that.addStuInfForm.recordName,
                  message: h('i', { style: 'color: teal'}, '数据获取错误')
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
              that.$notify({
                title: '保存成功',
                message: h('i', { style: 'color: green'}, res.message)
              });
            } else {
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
      const h = this.$createElement;
      var token = sessionStorage.getItem('token');
      $.ajax({
          //{newTeacherId}
          url: that.ip+"/newhelp/api/archiveStudent",
          type: "DELETE",
          beforeSend: function (request) {
            request.setRequestHeader("Authorization", token);
          },
          contentType: "application/json",
          dataType: "json",
          data: JSON.stringify({studentId: that.mockStuId,
                destoryingRecorder:sessionStorage.getItem('userName'),
                destoryingBasis:'',
                destoryingTime:''}),
          success: function (res) {
            if (res.success) {
              that.$notify({
                title: '删除成功',
                message: h('i', { style: 'color: teal'}, '已删除')
              });
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
    preChClick:function(row)  {
      this.dialogChangePeople = true;
      this.mockStuId = row.studentId;
    },
    
    ChClick:function(){
      var that = this;
      this.dialogChangePeople = false
      const h = this.$createElement;
      var token = sessionStorage.getItem('token');
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
  
}
</script>


<style>
.search-btn {
  background-color: white;
  font-size: 20px;
  width: 100%;
  height: 50px;
  line-height: 50px;
  margin-bottom: 10px;
}
.el-form-item {
  width: 50%;
  position: relative;
  left: 25%;
  display: flex;
  justify-content: flex-start;
}
.innerOp{
  margin: 20px;
}
.el-icon-back{
  width: 30px;
  height: 30px;;
}
.sub-title{
  width: 100%;
  position: relative;
}
.sub-title div{
  position:relative;
  width:30%;
  float:left;
  left: 10%;
}
.sub-title label{
  position: relative;
  font-size: 12px;
  float: left;
  margin:15px;
  height: 50px;
  width:200px
}
.sub-title input{
  float: right;
  background: transparent;
  border: 1px solid #dcdfe6;;
  padding-left: 5px;
  position: relative;
  width: 135%;
}
</style>
