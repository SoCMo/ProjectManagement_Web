<template>
  <div class="wrapper">
    <ButtonGroup class="operation">
      <Button type="success" icon="md-refresh" @click="Refresh" size="large" ghost>
        刷新
      </Button>
    </ButtonGroup>
    <Tabs value="1" class="tabs">
      <TabPane label="经济管理类" name="1" icon="md-book">
        <Table stripe border :loading="loading" :columns="columns1" :data="data_economy" class="table" height="750"
               size="large"></Table>
      </TabPane>
      <TabPane label="人文哲学类" name="2" icon="md-calendar">
        <Table sriple border :columns="columns2" :data="data_philosophy"
               table="table" :loading="loading" size="large"></Table>
      </TabPane>
      <TabPane label="计算科学类" name="3" icon="md-document">
        <Table sriple border :columns="columns3" :data="data_compute"
               table="table" :loading="loading" size="large"></Table>
      </TabPane>
      <TabPane label="生物化学类" name="4" icon="md-checkbox">
        <Table sriple border :columns="columns4" :data="data_biology"
               table="table" :loading="loading" size="large"></Table>
      </TabPane>
    </Tabs>
    <Modal v-if="modal3_delay" v-model="modal3" :title="infoTitle" width="900px">
      <p>项目描述：{{data5.projectCategoryDescription}}</p>
      <br>
      <p>业务员手机：{{data5.principalPhone}}</p>
      <br>
      <p>经费额度：{{data5.maxMoney}}元</p>
      <br>
      <p>申请经费：{{data5.projectMoney}}元</p>
      <br>
<!--      申报人类型：<p style="display: inline-flex;" v-for="item in data5.applicantType">{{item}}&nbsp;</p>-->
<!--      <br>-->
<!--      <br>-->
<!--      专家名单：<p style="display: inline-flex;" v-for="item in data5.expertList">{{item.userName}}&nbsp;</p>-->
<!--      <br>-->
<!--      <br>-->
      <p>项目成员(默认第一个为项目负责人)：</p>
      <br>
      <Table :columns="columns5" :data="data5.members" size="small" stripe></Table>
      <br>
      <p>项目申请书：<a @click="downloadProjectMaterial(data5.projectApplicationDownloadAddress)">点击下载</a></p>
      <br>
      <p v-if="data5.interimAddress===null">项目中期报告：未提交</p>
      <p v-if="data5.interimAddress!==null">项目中期报告：<a @click="downloadProjectMaterial(data5.interimAddress)">点击下载</a>
      </p>
      <br>
      <p v-if="data5.concludingAddress===null">项目结题报告：未提交</p>
      <p v-if="data5.concludingAddress!==null">项目结题报告：<a
        @click="downloadProjectMaterial(data5.concludingAddress)">点击下载</a></p>
    </Modal>
  </div>
</template>

<script>
    import axios from 'axios'
    import download from '../../../assets/js/download'

    export default {
        name: "projectCategory",
        watch: {
            modal3(val) {
                if (val) {
                    this.modal3_delay = val
                }
            }
        },
        data() {
            return {
                loading: false,
                modal1: false,
                modal2: false,
                modal3: false,
                modal3_delay: false,
                infoTitle: null,
                index: 0,
                middleReportAddress: '',
                lastReportAddress: '',
                columns1: [
                    {
                        title: '项目名称',
                        key: 'projectName',
                        align: 'center',
                    },
                    {
                        title: '项目简介',
                        key: 'projectDescription',
                        align: 'center',
                        tooltip: true
                    },
                    {
                        title: '项目状态',
                        key: 'reviewPhase',
                        align: 'center'
                    },
                    {
                        title: '操作',
                        key: 'operation',
                        align: 'center',
                        width: 250,
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.details1(params.index)
                                        }
                                    }
                                }, '详情')
                            ])
                        }
                    }
                ],
                columns2: [
                    {
                        title: '项目名称',
                        key: 'projectName',
                        align: 'center',
                    },
                    {
                        title: '项目简介',
                        key: 'projectDescription',
                        align: 'center',
                        tooltip: true
                    },
                    {
                        title: '项目状态',
                        key: 'reviewPhase',
                        align: 'center'
                    },
                    {
                        title: '操作',
                        key: 'operation',
                        align: 'center',
                        width: 250,
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.details2(params.index)
                                        }
                                    }
                                }, '详情')
                            ])
                        }
                    }
                ],
                columns3: [
                    {
                        title: '项目名称',
                        key: 'projectName',
                        align: 'center',
                    },
                    {
                        title: '项目简介',
                        key: 'projectDescription',
                        align: 'center',
                        tooltip: true
                    },
                    {
                        title: '项目状态',
                        key: 'reviewPhase',
                        align: 'center'
                    },
                    {
                        title: '操作',
                        key: 'operation',
                        align: 'center',
                        width: 250,
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.details3(params.index)
                                        }
                                    }
                                }, '详情')
                            ])
                        }
                    }
                ],
                columns4: [
                    {
                        title: '项目名称',
                        key: 'projectName',
                        align: 'center',
                    },
                    {
                        title: '项目简介',
                        key: 'projectDescription',
                        align: 'center',
                        tooltip: true
                    },
                    {
                        title: '项目状态',
                        key: 'reviewPhase',
                        align: 'center'
                    },
                    {
                        title: '操作',
                        key: 'operation',
                        align: 'center',
                        width: 250,
                        render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'info'
                                    },
                                    style: {
                                        marginRight: '5px'
                                    },
                                    on: {
                                        click: () => {
                                            this.details4(params.index)
                                        }
                                    }
                                }, '详情')
                            ])
                        }
                    }
                ],
                columns5: [
                    {
                        title: '姓名',
                        key: 'userName',
                        align: 'center'
                    },
                    {
                        title: '学号',
                        key: 'userId',
                        align: 'center'
                    },
                    {
                        title: '学院',
                        key: 'department',
                        align: 'center'
                    },
                    {
                        title: '电话',
                        key: 'phone',
                        align: 'center'
                    },
                    {
                        title: '邮箱',
                        key: 'mail',
                        align: 'center'
                    }
                ],
                data_economy: [],
                data_philosophy: [],
                data_compute: [],
                data_biology: [],
                data5: []
            }
        },
        mounted() {
            this.initData('初始化成功！')
        },
        methods: {
            Refresh() {
                this.initData('刷新成功！');
            },
            initData(msg) {
                this.data_economy = [];
                this.data_philosophy = [];
                this.data_compute = [];
                this.data_biology = [];
                this.loading = true;
                axios({
                    url: apiRoot + '/leader/ProjectByLeader',
                    method: 'get'
                }).then((res) => {
                    if (res.data.code === 'SUCCESS') {
                        console.log(res.data)
                        let projects = res.data.data
                        // console.log(projects)
                        projects.forEach(item => {
                            if(item.applicantType == "经济管理类")
                            {
                                this.data_economy.push(item)
                            }
                            else if(item.applicantType == '人文哲学类') {
                                this.data_philosophy.push(item)
                            }
                            else if(item.applicantType == '计算科学类'){
                                this.data_compute.push(item)
                            }
                            else{
                                this.data_biology.push(item)
                            }
                        })

                        console.log(this.data_economy)


                        this.loading = false;

                    }
                    else{
                        this.$Message.error(res.data.message)
                        this.loading = false;
                    }
                }).catch(() => {
                    this.$Message.error("初始化失败，请检查网络连接！")
                    this.loading = false
                })
            },
            details1(index) {
                const b = axios({
                    url: apiRoot + '/leader/MoreInfo?applicationId=' + this.data_economy[index].projectApplicationId.toString(),
                    method: 'get'
                }).then((res) => {
                    if (res.data.code === 'SUCCESS') {
                        this.infoTitle = res.data.data.projectName
                        this.data5 = res.data.data
                        if(this.data5.isInterimReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isInterimReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }
                        if(this.data5.isConcludingReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isConcludingReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }

                    }
                })
                this.modal3 = true
            },
            details2(index) {
                const b = axios({
                    url: apiRoot + '/leader/MoreInfo?applicationId=' + this.data_economy[index].projectApplicationId.toString(),
                    method: 'get'
                }).then((res) => {
                    if (res.data.code === 'SUCCESS') {
                        this.infoTitle = res.data.data.projectName
                        this.data5 = res.data.data
                        if(this.data5.isInterimReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isInterimReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }
                        if(this.data5.isConcludingReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isConcludingReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }

                    }
                })
                this.modal3 = true
            },
            details3(index) {
                const b = axios({
                    url: apiRoot + '/leader/MoreInfo?applicationId=' + this.data_economy[index].projectApplicationId.toString(),
                    method: 'get'
                }).then((res) => {
                    if (res.data.code === 'SUCCESS') {
                        this.infoTitle = res.data.data.projectName
                        this.data5 = res.data.data
                        if(this.data5.isInterimReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isInterimReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }
                        if(this.data5.isConcludingReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isConcludingReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }

                    }
                })
                this.modal3 = true
            },
            details4(index) {
                const b = axios({
                    url: apiRoot + '/leader/MoreInfo?applicationId=' + this.data_economy[index].projectApplicationId.toString(),
                    method: 'get'
                }).then((res) => {
                    if (res.data.code === 'SUCCESS') {
                        this.infoTitle = res.data.data.projectName
                        this.data5 = res.data.data
                        if(this.data5.isInterimReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isInterimReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }
                        if(this.data5.isConcludingReportActivated == 2)
                        {
                            this.data5.isInterimReportActivated = 0
                        }
                        else if(this.data5.isConcludingReportActivated == 1)
                        {
                            this.data5.isInterimReportActivated = 1
                        }

                    }
                })
                this.modal3 = true
            },
            downloadProjectMaterial(address) {
                console.log(address, '!')
                const that = this
                var filename = address.split('---')[1]
                axios({
                    url: address,
                    method: 'get',
                    headers: {Authorization: localStorage.getItem('token')},
                    responseType: 'blob'
                }).then((res) => {
                    if (res.status === 200) {
                        download(res.data, filename, 'text/plain');
                        this.$Message.success('下载成功！');
                    } else {
                        this.$Message.error('下载失败！');
                    }
                }).catch(() => {
                    this.$Message.error('下载失败，请检查网络连接！')
                })
            }
        }
    }

</script>

<style scoped lang="scss">
  @import "projectCategory";
</style>
