<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 分发接口管理
            </h4>
            <Row style="margin-bottom: 10px;">
                <Col span="24">
                    <Input placeholder="搜索分发接口服务名称" style="width: 200px" />
                    <Input placeholder="ID" style="width: 200px" />
                    <Select  style="width:120px;text-align:left" placeholder="请选择" v-model="search_status">
                        <Option v-for="item in search_status_list" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select>
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

import {distributionData, distributionTit} from '../datas/tableData.js';
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
        title: '分发接口服务名称',
        key: 'name'
    },
    {
        title: 'ID',
        key: 'id'
    },
    {
        title: '描述',
        key: 'txt'
    },
    {
        title: '下发分发配置',
        key: 'distribution'
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
                            name: 'distribute-setting',
                            query: {
                                type: 'distribute'
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
            InternetData: distributionData,
            pageIndex: 1,
            totalItemCount: 0,
            pageSize: 2,
            search_status: '',
            search_status_list: [
                // { value: 0, label:'请选择'},
                { value: 1, label:'已经下发'},
                {value: 2, label:'未下发'}
            ],
        };
    },
    mounted () {
        this.totalItemCount =   this.InternetData.length
    }
};
</script>
