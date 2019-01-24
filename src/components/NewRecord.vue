<template>
<div>
  <el-form>
      <el-form-item label="学号">
        <el-autocomplete
          popper-class="my-autocomplete"
          v-model="autocompleteId"
          :fetch-suggestions="querySearchId"
          placeholder="请输入要检索的学号"
          @select="handleSelect">
          <i
            class="el-icon-edit el-input__icon"
            slot="suffix"
            @click="handleIconClick">
          </i>
          <template  slot-scope="{ item }">
            <div class="name">{{ item.name }}</div>
            <span class="stu">{{ item.studentId }}</span>
          </template>
        </el-autocomplete>
      </el-form-item>
      <el-form-item label="姓名">
        <el-autocomplete
          popper-class="my-autocomplete"
          v-model="autocompleteName"
          :fetch-suggestions="querySearchName"
          placeholder="请输入要检索的姓名"
          @select="handleSelect">
          <i
            class="el-icon-edit el-input__icon"
            slot="suffix"
            @click="handleIconClick">
          </i>
          <template  slot-scope="{ item }">
            <div class="name">{{ item.name }}</div>
            <span class="stu">{{ item.studentId }}</span>
          </template>
        </el-autocomplete>
      </el-form-item>
  </el-form>
  

  <!-- <div class="dynamicClass" :model="dynamic_data"> -->
        <!--新建建档-->
        <!-- <div><p>学号:</p><el-input  placeholder="studentId" v-model="dynamic_data.studentId"/></div>
        <div><p>教师号:</p><el-input  placeholder="teacherId" v-model="dynamic_data.teacherId"/></div>
        <div><p>性别:</p><el-input  placeholder="sex" v-model="dynamic_data.sex"/></div>
        <div><p>姓名:</p><el-input  placeholder="name" v-model="dynamic_data.name"/></div>
        <div><p>专业:</p><el-input  placeholder="major" v-model="dynamic_data.major"/></div>
        <div><p>年级:</p><el-input  placeholder="grade" v-model="dynamic_data.grade"/></div>
        <div><p>班级:</p><el-input  placeholder="studentClass" v-model="dynamic_data.studentClass"/></div>
        <div><p>政治面貌:</p><el-input  placeholder="politicalStatus" v-model="dynamic_data.politicalStatus"/></div>
        <div><p>民族:</p><el-input  placeholder="ethnicGroup" v-model="dynamic_data.ethnicGroup"/></div>
        <div><p>职责:</p><el-input  placeholder="duty" v-model="dynamic_data.duty"/></div>
        <div><p>寝室:</p><el-input  placeholder="dormitory" v-model="dynamic_data.dormitory"/></div>
        <div><p>出生日期:</p><el-input  placeholder="birthOrigin" v-model="dynamic_data.birthOrigin"/></div>
        <div><p>家庭地址:</p><el-input  placeholder="familyAddress" v-model="dynamic_data.familyAddress"/></div>
        <div><p>联系方式:</p><el-input  placeholder="contactWay" v-model="dynamic_data.contactWay"/></div>
        <div><p>家庭电话:</p><el-input  placeholder="familyTelNumber" v-model="dynamic_data.familyTelNumber"/></div>
        <div><p>父亲电话:</p><el-input  placeholder="fatherTelNumber" v-model="dynamic_data.fatherTelNumber"/></div>
        <div><p>母亲电话:</p><el-input  placeholder="motherTelNumber" v-model="dynamic_data.motherTelNumber"/></div>
        <div><p>家庭状况:</p><el-input  placeholder="familyCondition" v-model="dynamic_data.familyCondition"/></div>
        <div><p>学习情况:</p><el-input  placeholder="studyCondition" v-model="dynamic_data.studyCondition"/></div>
        <div><p>健康情况:</p><el-input  placeholder="healthCondition" v-model="dynamic_data.healthCondition"/></div>
        <div><p>生活情况:</p><el-input  placeholder="lifeCondition" v-model="dynamic_data.lifeCondition"/></div>
        <div><p>其他情况:</p><el-input  placeholder="otherCondition" v-model="dynamic_data.otherCondition"/></div>
        <div><p>创建基本情况:</p><el-input  placeholder="bulidingBasis" v-model="dynamic_data.bulidingBasis"/></div>
        <div><p>创建记录人:</p><el-input  placeholder="bulidingRecorder" v-model="dynamic_data.bulidingRecorder"/></div>
        <div><p>创建时间:</p><el-input  placeholder="bulidingTime" v-model="dynamic_data.bulidingTime"/></div>
        <div><p>创建人:</p><el-input  placeholder="bulidingPerson" v-model="dynamic_data.bulidingPerson"/></div>
        <div><p>创建人职责:</p><el-input  placeholder="bulidingPersonDuty" v-model="dynamic_data.bulidingPersonDuty"/></div>
        <el-select v-model="dynamic_data.helpType" placeholder="帮扶类型" style="margin-top:50px;margin-bottom:200px;">
          <el-option v-for="data in holeHelpType" :label="data.helpTypeName" :value="data.helpTypeName"></el-option>
        </el-select>
        <el-select v-model="dynamic_data.attentionType"  placeholder="关注类型" style="margin-top:50px;margin-bottom:200px;">
          <el-option v-for="data in holeAttentionType" :label="data.attentionTypeName" :value="data.attentionTypeName"></el-option>
        </el-select>
  </div>
  <el-button class="submitButton" type="success" @click="submit()" icon="el-icon-check" ></el-button>-->
<el-form :inline="true" :model="dynamic_data" class="demo-form-inline" label-width="135px">
  <el-form-item v-for="(item,index) in input_data" :key="index" :label="item.label">
    <el-input v-model="dynamic_data[item.code]" :placeholder="item.label"></el-input>
  </el-form-item>
  <el-form-item label="帮扶类型">
    <el-select v-model="dynamic_data.helpType" placeholder="帮扶类型">
      <el-option v-for="data in holeHelpType" :label="data.helpTypeName" :value="data.helpTypeName"></el-option>
    </el-select>
  </el-form-item>
  <el-form-item label="关注类型">
    <el-select v-model="dynamic_data.attentionType" placeholder="关注类型">
      <el-option v-for="data in holeAttentionType" :label="data.attentionTypeName" :value="data.attentionTypeName"></el-option>
    </el-select>
  </el-form-item>
  <el-form-item>
    <el-button class="submitButton" type="success" @click="submit()">创建</el-button>
  </el-form-item>
</el-form>
</div>
</template>

<script>
export default {
  data(){
    return{
      ip:'http://211.83.111.247:8082',
      input_data:[
        {label:'学号',code:"studentId"},{label:'教师帐号',code:"teacherId"},{label:'性别',code:'sex'},
        {label:'姓名',code:'name'},{label:'专业',code:'major'},{label:'年级',code:'grade'},
        {label:'班级',code:'studentClass'},{label:'政治面貌',code:'politicalStatus'},{label:'民族',code:'ethnicGroup'},
        {label:'职务',code:'duty'},{label:'宿舍',code:'dormitory'},{label:'籍贯',code:'birthOrigin'},
        {label:'家庭详细地址',code:'familyAddress'},{label:'联系方式',code:'contactWay'},{label:'家庭电话',code:'familyTelNumber'},
        {label:'父亲电话',code:'fatherTelNumber'},{label:'母亲电话',code:'motherTelNumber'},{label:'家庭状况',code:'familyCondition'},
        {label:'学习状况',code:'studyCondition'},{label:'健康状况',code:'healthCondition'},{label:'生活状况',code:'lifeCondition'},
        {label:'其他情况',code:'otherCondition'},{label:'创建基本情况',code:'bulidingBasis'},{label:'创建记录人',code:'bulidingRecorder'},
        {label:'创建时间',code:'bulidingTime'},{label:'创建人',code:'bulidingPerson'},{label:'创建人职责',code:'bulidingPersonDuty'}
      ],
      textarea_data:[
        {placeholder:"",row:""}
      ],
      //总的帮扶类型
      holeHelpType:[],
      holeAttentionType:[],
      dynamic_data:{
            studentId:"",
            teacherId:"",
            sex:"",
            name:"",
            major:"",
            grade:"",
            studentClass:"",
            politicalStatus:"",
            ethnicGroup:"",
            duty:"",
            dormitory:"",
            birthOrigin:"",
            familyAddress:"",
            familyTelNumber:"",
            contactWay:"",
            familyTelNumber:"",
            fatherTelNumber:"",
            motherTelNumber:"",
            familyCondition:"",
            studyCondition:"",
            healthCondition:"",
            lifeCondition:"",
            otherCondition:"",
            bulidingBasis:"",
            bulidingRecorder:"",
            bulidingTime:"",
            bulidingPerson:"",
            bulidingPersonDuty:"",
            helpType:"",
            attentionType:""
      },
      students:[],
      autocompleteId:"",
      autocompleteName:"",
      new_dynamic_data:{},
      //暂时存放查询学生条件
      temp_querystr:{
                        studentId:"",
                        name:"",
                        grade:"",
                        studentClass:"",
                        sex:"",
                        duty:"",
                        dormitory:"",
                        contactWay:"",
                        idCardNumber:"",
                        qqNumber:"",
                        email:"",
                        birthday:"",
                        height:"",
                        major:"",
                        politicalStatus:"",
                        ethnicGroup:"",
                        birthOrigin:"",
                        collegeEntranceExamScore:"",
                        collegeEntranceExamEnglishScore:"",
                        entranceExamEnglishScore:"",
                        hometownRailwayStation:"",
                        province:"",
                        city:"",
                        familyAddress:"",
                        familyTelNumber:"",
                        postcode:"",
                        specialty:"",
                        dutyInHighSchool:"",
                        awardInHighSchool:"",
                        isHadTechnologyCompetitionAward:"",
                        fatherName:"",
                        fatherWorkUnit:"",
                        fatherWorkUnitAddress:"",
                        fatherDuty:"",
                        fatherPostcode:"",
                        fatherTelNumber:"",
                        motherName:"",
                        motherWorkUnit:"",
                        motherWorkUnitAddress:"",
                        motherDuty:"",
                        motherPostcode:"",
                        motherTelNumber:"",
      }
    }
  },
  methods:{
          querySearchId(queryString, cb) {
            var that = this;
            var token = sessionStorage.getItem("token");
            const h = this.$createElement; 
            if (queryString.length>0) {
              // that.temp_querystr.grade = queryString.substring(0, 4);
              // that.temp_querystr.studentClass = queryString.substring(6, 10);
              // console.log(that.temp_querystr);
              that.temp_querystr.studentId = queryString;
              $.ajax({
                url: this.ip+"/newhelp/api/baseStudents/byIdName",
                type: "POST",
                beforeSend: function (request) {
                    request.setRequestHeader("Authorization", token);
                },
                contentType: "application/json",
                dataType: "json",
                data: JSON.stringify(that.temp_querystr),
                success: function (res) {
                  if (res.success) {
                    that.students = res.data;
                    var results = that.students;//queryString ? this.students.filter(this.createFilter(queryString)) : this.students;
                    // 调用 callback 返回建议列表的数据
                    cb(results);
                  }
                },
                error: function (el) {
                  console.log(el);
                  console.log('数据获取错误');
                },
              })
            }
          },
          querySearchName(queryString, cb) {
            var that = this;
            var token = sessionStorage.getItem("token");
            const h = this.$createElement; 
            if (queryString.length>0) {
              // that.temp_querystr.grade = queryString.substring(0, 4);
              // that.temp_querystr.studentClass = queryString.substring(6, 10);
              // console.log(that.temp_querystr);
              that.temp_querystr.name = queryString;
              $.ajax({
                url: this.ip+"/newhelp/api/baseStudents/byIdName",
                type: "POST",
                beforeSend: function (request) {
                    request.setRequestHeader("Authorization", token);
                },
                contentType: "application/json",
                dataType: "json",
                data: JSON.stringify(that.temp_querystr),
                success: function (res) {
                  if (res.success) {
                    that.students = res.data;
                    var results = that.students;//queryString ? this.students.filter(this.createFilter(queryString)) : this.students;
                    // 调用 callback 返回建议列表的数据
                    cb(results);
                  }
                },
                error: function (el) {
                  console.log(el);
                  console.log('数据获取错误');
                },
              })
            }
          },
          createFilter(queryString) {

            return (student) => {
              return (student.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
            };
          },
          loadAll() {
            return ;
          },
          handleSelect(item) {
            var that = this,
                token = sessionStorage.getItem("token"),
                i,
                j;
            const h = this.$createElement; 
            that.dynamic_data = item;
            that.dynamic_data.teacherId = sessionStorage.getItem('userName');
            $.ajax({
                url: this.ip+"/newhelp/api/baseStudent/archive/"+item.studentId,
                type: "GET",
                beforeSend: function (request) {
                    request.setRequestHeader("Authorization", token);
                },
                contentType: "application/json",
                dataType: "json",
                data: {},
                success: function (res) {
                  if (res.success) {
                    that.new_dynamic_data = res.data;
                  } else {
                    that.$notify({
                      title: '标题名称',
                      message: h('i', { style: 'color: teal'}, '数据获取错误')
                    });
                  }
                },
                error: function (el) {
                  console.log(el);
                  console.log('数据获取错误');
                },
              })

              for (i in that.dynamic_data) {
                if (that.dynamic_data[i]==="") {
                  for(j in that.new_dynamic_data){
                    if(i == j){
                      that.dynamic_data[i] = that.new_dynamic_data[j];
                      break;
                    }
                    else {
                      continue;
                    }
                  }
                }
                else {
                }
                
              }
          },
          handleIconClick(ev) {
          },
          submit(){
            var that = this;
            var token = sessionStorage.getItem("token");
            const h = this.$createElement; 
            $.ajax({
                  //{recordName}/{studentId}
                  url: this.ip+"/newhelp/api/archiveStudent",
                  type: "POST",
                  beforeSend: function (request) {
                      request.setRequestHeader("Authorization", token);
                  },
                  contentType: "application/json",
                  dataType: "json",
                  data: JSON.stringify(that.dynamic_data),
                  success: function (res) {
                    if (res.success) {
                      that.$notify({
                        title: '新增成功',
                        message: h('i', { style: 'color: green'}, res.message)
                      });
                    } else {
                      that.$notify({
                        title: '添加失败',
                        message: h('i', { style: 'color: teal'}, res.message)
                      });
                    }
                  },
                  error: function () {
                    that.$notify({
                        title: 'API报错',
                        message: h('i', { style: 'color: teal'}, '数据获取错误')
                      })
                  },
                })
          }
  },
  created() {
      var that = this;
      var token = sessionStorage.getItem("token");
      const h = this.$createElement; 
      $.ajax({
            //{recordName}/{studentId}
            url: this.ip+"/newhelp/api/helpTypes",
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json",
            dataType: "json",
            data: {},
            success: function (res) {
              if (res.success) {
                that.holeHelpType = res.data; 
              } 
            },
            error: function () {
            },
          })
      $.ajax({
            //{recordName}/{studentId}
            url: this.ip+"/newhelp/api/attentionTypes",
            type: "GET",
            beforeSend: function (request) {
                request.setRequestHeader("Authorization", token);
            },
            contentType: "application/json",
            dataType: "json",
            data: {},
            success: function (res) {
              if (res.success) {
                that.holeAttentionType = res.data; 
              } 
            },
            error: function () {
            },
          })
  },
  mounted() {
        this.students = this.loadAll();
  }
}
</script>


<style scoped>
.my-autocomplete {
  
}
.my-autocomplete li {
    line-height: normal;
    padding: 7px;
}
.my-autocomplete .name {
      text-overflow: ellipsis;
      overflow: hidden;
    }
.my-autocomplete  .stu {
      font-size: 12px;
      color: #b4b4b4;
    }

.my-autocomplete .highlighted .addr {
      color: #ddd;
}
.submitButton{
  width: 100%;

}
.el-input__inner{
  display: inline-block;
  margin: 5px;
  padding: 10px;
  width: 200px;
}
.dynamicClass{
  width: 100%;
  position: relative;
}
.dynamicClass div{
  position:relative;
  width:30%;
  float:left;
  left: 10%;
}
.dynamicClass label{
  position: relative;
  font-size: 12px;
  float: left;
  margin:15px;
  height: 50px;
  width:200px
}
.dynamicClass input{
  float: right;
  background: transparent;
  border: 1px solid #dcdfe6;;
  padding-left: 5px;
  position: relative;
  width: 135%;
}
</style>
