<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 控制接口管理
            </h4>
            <Row style="margin-bottom: 10px;">
                <Col span="24">
                    <Input placeholder="搜索控制接口服务名称" style="width: 200px" />
                    <Input placeholder="搜索物联网接口服务名称" style="width: 200px" />
                    <Input placeholder="ID" style="width: 200px" />
                    <!-- <Select  style="width:120px;text-align:left" placeholder="请选择状态..." v-model="search_status">
                        <Option v-for="item in search_status_list" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select> -->
                    <Button type="primary" size="large">查询</Button>
                    <Button type="default" size="large">清空</Button>
                </Col>
                <!-- <Col  span="12" style="text-align: right">
                    <Button type="primary" size="large"><Icon type="ios-add"></Icon> 新增</Button>
                </Col> -->
            </Row>
            <Row :gutter="10">
                <Col span="24">
                    <Table :loading="table_loading" border :columns="InternetTit" :data="InternetData"></Table>
                    <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                </Col>
            </Row>
        </Card>
    </div>
</template>

<script>

import {controlTit, controlData} from '../datas/tableData.js';
export default {
    name: '',
    data () {
        return {
            InternetTit: [{
    title: '序号',
    type: 'index',
    width: 80,
    align: 'center'
    },
    {
        title: '控制接口服务名称',
        key: 'name'
    },
    {
        title: '对应的物联网接口服务名称',
        key: 'name1'
    },
    {
        title: '描述',
        key: 'txt'
    },
    {
        title: 'ID',
        key: 'id'
    },
    {
        title: '设备状态',
        key: 'status'
    },
    {
        title: '最后一次记录时间',
        key: 'time'
    },
    {
        title: '操作',
        key: 'Operation',
        width: 150,
        align: 'center',
        render: (h, params) => {
            return h('div', [
                h('Button', {
                    props: {
                    type: 'primary',
                    size: 'small'
                    },
                    style: {
                    marginRight: '5px'
                    },
                    on: {
                    click: () => {
                         this.$router.push({
                            name: 'control-setting',
                            query: {
                                type: 'control'
                            }
                        })
                    }
                }
                }, '配置')
                // h('Button', {
                //     props: {
                //     type: 'error',
                //     size: 'small'
                //     },
                //     on: {
                //         click: () => {
                //             this.remove(params.index)
                //         }
                //     }
                //     }, '详情')
                ]);
            }
        }
    ],
            InternetData: controlData,
            pageIndex: 1,
            totalItemCount: 0,
            pageSize: 2,
            search_status: 0,
            search_status_list: [
                { value: 0, label:'请选择'},
                { value: 1, label:'在线'},
                {value: 2, label:'连线'},
                {value: 3, label:'尝试中'}
            ],
        };
    },
    mounted () {
        this.totalItemCount =   this.InternetData.length
    }
};
</script>
