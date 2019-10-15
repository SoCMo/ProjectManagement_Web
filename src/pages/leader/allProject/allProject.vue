<template>
  <div class="wrapper">
    <div class="operation">
      <ButtonGroup>
        <Button type="success" icon="md-refresh" @click="Refresh" size="large" ghost>
          刷新
        </Button>
      </ButtonGroup>
    </div>
    <Tabs class="tabs">
      <TabPane label="经济管理类" name="1" icon="md-book">
        <Table sriple border :columns="columns1" :data="data_economy"
               table="table" :loading="loading" size="large"></Table>
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
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'allProject',
        data() {
            return {
                loading: false,
                columns1: [
                    {
                        title: '项目名称',
                        align: 'center',
                        key: 'projectName'
                    },
                    {
                        title: '截止日期',
                        key: 'deadLine',
                        align: 'center'
                    },
                    {
                        title: '项目简介',
                        key: 'introduce',
                        align: 'center',
                        tooltip: true
                    }
                ],
                columns2: [
                    {
                        title: '项目名称',
                        align: 'center',
                        key: 'projectName'
                    },
                    {
                        title: '截止日期',
                        key: 'deadLine',
                        align: 'center'
                    },
                    {
                        title: '项目简介',
                        key: 'introduce',
                        align: 'center',
                        tooltip: true
                    }
                ],
                columns3: [
                    {
                        title: '项目名称',
                        align: 'center',
                        key: 'projectName'
                    },
                    {
                        title: '截止日期',
                        key: 'deadLine',
                        align: 'center'
                    },
                    {
                        title: '项目简介',
                        key: 'introduce',
                        align: 'center',
                        tooltip: true
                    }
                ],
                columns4: [
                    {
                        title: '项目名称',
                        align: 'center',
                        key: 'projectName'
                    },
                    {
                        title: '截止日期',
                        key: 'deadLine',
                        align: 'center'
                    },
                    {
                        title: '项目简介',
                        key: 'introduce',
                        align: 'center',
                        tooltip: true
                    }
                ],
                data_economy: [],
                data_philosophy: [],
                data_compute: [],
                data_biology: []
            };
        },
        mounted() {
            this.initData('初始化成功！')
        },
        methods: {
            Refresh() {
                this.initData('刷新成功！');
            },
            initData(msg) {
                this.loading = true
                this.data_philosophy = [];
                this.data_compute = [];
                this.data_biology = [];
                this.data_economy = [];
                axios({
                    url: apiRoot + '/leader/AllAviProjectCategory',
                    method: 'get'
                }).then((res) => {
                    console.log(res.data)
                    if (res.data.code === 'SUCCESS') {
                        let projects = res.data.data
                        projects.forEach(item => {
                            if(item.type == '经济管理类')
                            {
                                // console.log(item)
                                this.data_economy.push(item)
                            }
                            else if(item.type == '人文哲学类') {
                                this.data_philosophy.push(item)
                            }
                            else if(item.type == '计算科学类'){
                                this.data_compute.push(item)
                            }
                            else{
                                this.data_biology.push(item)
                            }
                        })
                        console.log(this.data_economy)
                        this.loading = false
                        this.$Message.success(msg)
                    } else {
                        this.$Message.error(res.data.message)
                        this.loading = false
                    }
                }).catch(() => {
                    this.$Message.error("初始化失败，请检查网络连接！")
                    this.loading = false
                })
            }
        }
    }
</script>

<style scoped lang="scss">
  @import "allProject";
</style>
