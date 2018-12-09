<template>
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
            <div><p>studentId:</p><el-input  v-model="stuDetailInf.studentId"/></div>
            <div><p>sex:</p><el-input  v-model="stuDetailInf.sex"/></div>
            <div><p>name:</p><el-input  v-model="stuDetailInf.name"/></div>
            <div><p>major:</p><el-input  v-model="stuDetailInf.major"/></div>
            <div><p>grade:</p><el-input  v-model="stuDetailInf.grade"/></div>
            <div><p>studentClass:</p><el-input  v-model="stuDetailInf.studentClass"/></div>
            <div><p>politicalStatus:</p><el-input  v-model="stuDetailInf.politicalStatus"/></div>
            <div><p>ethnicGroup:</p><el-input  v-model="stuDetailInf.ethnicGroup"/></div>
            <div><p>duty:</p><el-input  v-model="stuDetailInf.duty"/></div>
            <div><p>dormitory:</p><el-input  v-model="stuDetailInf.dormitory"/></div>
            <div><p>birthOrigin:</p><el-input  v-model="stuDetailInf.birthOrigin"/></div>
            <div><p>familyAddress:</p><el-input  v-model="stuDetailInf.familyAddress"/></div>
            <div><p>contactWay:</p><el-input  v-model="stuDetailInf.contactWay"/></div>
            <div><p>familyTelNumber:</p><el-input  v-model="stuDetailInf.familyTelNumber"/></div>
            <div><p>fatherTelNumber:</p><el-input  v-model="stuDetailInf.fatherTelNumber"/></div>
            <div><p>motherTelNumber:</p><el-input  v-model="stuDetailInf.motherTelNumber"/></div>
            <div><p>familyCondition:</p><el-input  v-model="stuDetailInf.familyCondition"/></div>
            <div><p>studyCondition:</p><el-input  v-model="stuDetailInf.studyCondition"/></div>
            <div><p>healthCondition:</p><el-input  v-model="stuDetailInf.healthCondition"/></div>
            <div><p>lifeCondition:</p><el-input  v-model="stuDetailInf.lifeCondition"/></div>
            <div><p>otherCondition:</p><el-input  v-model="stuDetailInf.otherCondition"/></div>
            <div><p>bulidingBasis:</p><el-input  v-model="stuDetailInf.bulidingBasis"/></div>
            <div><p>bulidingRecorder:</p><el-input  v-model="stuDetailInf.bulidingRecorder"/></div>
            <div><p>bulidingTime:</p><el-input  v-model="stuDetailInf.bulidingTime"/></div>
            <div><p>bulidingPerson:</p><el-input  v-model="stuDetailInf.bulidingPerson"/></div>
            <div><p>bulidingPersonDuty:</p><el-input  v-model="stuDetailInf.bulidingPersonDuty"/></div>
            <div><p>helpType:</p><el-input  v-model="stuDetailInf.helpType"/></div>
            <div><p>attentionType:</p><el-input  v-model="stuDetailInf.attentionType"/></div>
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
</template>
<script>
export default {
    name:"detail",
    data(){
        return{

        }
    },
    props:['mockStuId','mockStuName'],
    methods:{
        handleClick(tab, event) {
            var token = sessionStorage.getItem("token");
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
        deleteStuInf:function(){
            var that =this;
            const h = this.$createElement;
            $.ajax({
                //{newTeacherId}
                url: that.ip+"/newhelp/api/archiveStudent",
                type: "DELETE",
                // beforeSend: function (request) {
                //   request.setRequestHeader("Authorization", token);
                // },
                contentType: "application/json; charset=utf-8",
                dataType: "json",
                data: JSON.stringify({studentId: that.mockStuId,
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
            const h = this.$createElement;
            $.ajax({
                //{newTeacherId}
                url: that.ip+"/newhelp/api/change/"+ that.changePeopleInf.people,
                type: "POST",
                // beforeSend: function (request) {
                //   request.setRequestHeader("Authorization", token);
                // },
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
            /* this.$prompt('请输入联系人', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                ///[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/
                inputPattern: /[a-y]?/,
                inputErrorMessage: '联系人格式不正确'
                }).then(
                ({ value }) => {
                    this.$message({
                        type: 'success',
                        message: '更改完联系人是: ' + value
                    });
                    that.changePeople = value;
                    that.$prompt('请输入原因', '提示', {
                        confirmButtonText: '确定',
                        cancelButtonText: '取消',
                        ///[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/
                        inputPattern: /[a-y]?/,
                        inputErrorMessage: '原因格式不正确'
                    }).then(({ value }) => {
                        this.$message({
                        type: 'success',
                        message: '你的原因是: ' + value
                        });
                        var token = sessionStorage.getItem("token");
                        console.log(token);
                        $.ajax({
                            //{newTeacherId}
                            url: that.ip+"/newhelp/api/change/"+ that.changePeople,
                            type: "POST",
                            beforeSend: function (request) {
                            request.setRequestHeader("Authorization", token);
                            },
                            contentType: "application/json; charset=utf-8",
                            dataType: "json",
                            data: JSON.stringify({studentId: that.mockStuId,
                                changeReason: value}),
                            success: function (res) {
                            if (res.success) {
                                console.log('成功');
                                that.record_table = res.data.data;
                            } else {
                                console.log(res);
                                that.$notify({
                                title: '标题名称',
                                message: h('i', { style: 'color: teal'}, '变更失败')
                                });
                            }
                            },
                            error: function () {
                            console.log('失败');
                            },
                        })
                        
                    })/*.catch(() => {
                        this.$message({
                        type: 'info',
                        message: '取消输入1'
                        });       
                    })
                    
                }
                ).catch(() => {
                this.$message({
                    type: 'info',
                    message: '取消输入2'
                });       
                });
            */
            
            }
    }
}
</script>
