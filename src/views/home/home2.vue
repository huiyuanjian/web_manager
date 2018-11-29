<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 设备监控
            </h4>
            <Row style="margin-bottom: 10px;">
                <Col span="12">
                   <Select  style="width:120px;text-align:left" placeholder="请选择" v-model="search_status">
                        <Option v-for="item in search_status_list" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    </Select>
                    <!-- <Input  icon="search" placeholder="搜索分组名称" style="width: 200px" /> -->
                    <Input  placeholder="搜索IOServer名称" style="width: 200px" />
                    <Input  placeholder="搜索设备名称" style="width: 200px" />
                 
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

import {index2Tit, index2Data} from '../datas/tableData.js';
export default {
    name: '',
    data () {
        return {
            InternetTit: index2Tit,
            InternetData: index2Data,
            pageIndex: 1,
            totalItemCount: 0,
            pageSize: 2,
            search_status: '',
            search_status_list: [
                { value: 0, label:'分组1'},
                { value: 1, label:'分组2'}
            ],
        };
    },
    mounted () {
        this.totalItemCount =   this.InternetData.length
    }
};
</script>
