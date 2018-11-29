
<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 同步数据
            </h4>
            <Row>
                <Col span="5" :offset="1">
                    <Card style="width:100%;height: 600px;">
                        <p slot="title">
                            采集分组
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Select  style="width:120px;text-align:left" placeholder="物联网接口" v-model="search_status">
                                    <Option v-for="item in search_status_list" :value="item.value" :key="item.value">{{ item.label }}</Option>
                                </Select>
                                <Input icon="search" placeholder="输入关键字" style="width: 187px" />
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Tree :data="searchList"></Tree>
                            </Col>
                        </Row>
                    </Card>
                </Col>
                 <Col span="5" :offset="1">
                    <Card style="width:100%;height: 600px;">
                        <p slot="title">
                           设备
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Input icon="search" placeholder="设备名称" style="width: 100%" />
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <div style="margin-bottom: 10px;text-align: center;">
                                    <Button type="primary" size="large" >设备</Button>
                                    <Button type="primary" size="large" >数据包</Button>
                                </div>
                                <Table border :columns="InternetTit" :data="InternetData"></Table>
                                <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                            </Col>
                        </Row>
                    </Card>
                </Col>
                 <Col span="10" :offset="1">
                    <Card style="width:100%;height: 600px;">
                        <p slot="title">
                            设备信息
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                 <Form 
                                    ref="userForm"
                                    :model="userForm" 
                                    :label-width="100" 
                                    label-position="right"
                                    :rules="inforValidate"
                                >
                                    <FormItem label="ID：" >
                                        <div style="display:inline-block;width:300px;">
                                            <Input disabled value="32"></Input>
                                        </div>
                                    </FormItem>
                                     <FormItem label="名称：" >
                                        <div style="display:inline-block;width:300px;">
                                            <Input disabled value="水位监测设备1"></Input>
                                        </div>
                                    </FormItem>
                                    <FormItem label="采集周期：" >
                                        <div style="display:inline-block;width:300px;">
                                            <Input disabled value="32"></Input>
                                        </div>
                                    </FormItem>
                                </Form>
                                <!-- <div style="margin-bottom: 10px;text-align: center;">
                                    <Button type="primary" size="large" >变量启用</Button>
                                    <Button type="primary" size="large" >变量停用</Button>
                                </div> -->
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Table border :columns="InternetTit2" :data="InternetData2"></Table>
                                <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                            </Col>
                        </Row>
                    </Card>
                </Col>
            </Row>
            
        </Card>
    </div>
</template>

<script>

import {InternetData, InternetTit} from '../datas/tableData.js';
export default {
    name: '',
    data () {
        return {
            search_status: '',
            InternetTit: [
            {
                type: 'selection',
                width: 60,
                align: 'center'
            },{
                key:'name',
                title: '设备名称'
            }],
            InternetData: [{
                name: '设备名称1'
            }],
            InternetTit2: [
            {
                title: '序号',
                type: 'index',
                width: 65,
                align: 'center'
            },{
                key:'name',
                title: '变量名称'
            },{
                key:'txt',
                title: '变量描述'
            },{
                key:'txt1',
                title: '映射采集变量'
            },{
                title: '操作',
                key: 'Operation',
                width: 65,
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
                                    name: 'configure-map'
                                })
                            }
                        }
                        }, '映射')
                        ]);
                    }
                }
            ],
            InternetData2: [{
                name: '变量1',
                txt: '变量描述',
                txt1: 'IOServer变量'
            }],
            pageIndex: 1,
            totalItemCount: 0,
            pageSize: 2,
            searchList: [{
                title: 'XXXX厂',
                expand: true,
                children: [
                    {
                        title: '污水处理区2',
                        expand: true,
                        children: [
                            {
                                title: '转速组1'
                            },
                            {
                                title: '转速组2'
                            }
                        ]
                    },
                ]
            }]
        };
    },
    mounted () {
        this.totalItemCount =   this.InternetData.length
    }
};
</script>
