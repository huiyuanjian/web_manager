<style lang="less">
    @import "./home.less";
    @import "../../styles/common.less";
</style>
<template>
    <div class="home-main">

         <Row>
            <Card>
                <!-- <p slot="title" class="card-title">
                    <Icon type="map"></Icon>
                    今日服务调用地理分布
                </p> -->
                <div class="map-con">
                     <Col span="16" class="map-incon">
                        <Row type="flex" justify="center" align="middle">
                            <home-map :city-data="cityData"></home-map>
                        </Row>
                    </Col>
                    <Col span="8">
                        <map-data-table :cityData="cityData" height="281" :style-obj="{margin: '12px 0 0 11px'}"></map-data-table>
                    </Col>
                </div>
            </Card>
        </Row>
         <Row :gutter="10" class="margin-top-10">
            <Col span="24" :style="{marginBottom: '10px'}">
               <Table :columns="index1Tit01"  height="231"  :data="index1Data01" size="small" ref="tableExcel"></Table>
            </Col>
            <Col span="24">
                 <Table :columns="index1Tit02"  height="231" :data="index1Data02" size="small" ref="tableExcel"></Table>
            </Col>
        </Row>
    </div>
</template>

<script>
import cityData from './map-data/get-city-value.js';
import homeMap from './components/map.vue';
import dataSourcePie from './components/dataSourcePie.vue';
import visiteVolume from './components/visiteVolume.vue';
import serviceRequests from './components/serviceRequests.vue';
import userFlow from './components/userFlow.vue';
import countUp from './components/countUp.vue';
import inforCard from './components/inforCard.vue';
import mapDataTable from './components/mapDataTable.vue';
import toDoListItem from './components/toDoListItem.vue';
import {index1Tit01, index1Data01,index1Tit02,index1Data02} from '../datas/tableData.js';
export default {
    name: 'home',
    components: {
        homeMap,
        dataSourcePie,
        visiteVolume,
        serviceRequests,
        userFlow,
        countUp,
        inforCard,
        mapDataTable,
        toDoListItem
    },
    data () {
        return {
            toDoList: [
                {
                    title: '去iView官网学习完整的iView组件'
                },
                {
                    title: '去iView官网学习完整的iView组件'
                },
                {
                    title: '去iView官网学习完整的iView组件'
                },
                {
                    title: '去iView官网学习完整的iView组件'
                },
                {
                    title: '去iView官网学习完整的iView组件'
                }
            ],
            count: {
                createUser: 496,
                visit: 3264,
                collection: 24389305,
                transfer: 39503498
            },
            cityData: cityData,
            showAddNewTodo: false,
            newToDoItemValue: '',
            index1Tit01: index1Tit01,
            index1Data01: index1Data01,
            index1Tit02: index1Tit02,
            index1Data02: index1Data02
        };
    },
    computed: {
        avatorPath () {
            return localStorage.avatorImgPath;
        }
    },
    methods: {
        addNewToDoItem () {
            this.showAddNewTodo = true;
        },
        addNew () {
            if (this.newToDoItemValue.length !== 0) {
                this.toDoList.unshift({
                    title: this.newToDoItemValue
                });
                setTimeout(() => {
                    this.newToDoItemValue = '';
                }, 200);
                this.showAddNewTodo = false;
            } else {
                this.$Message.error('请输入待办事项内容');
            }
        },
        cancelAdd () {
            this.showAddNewTodo = false;
            this.newToDoItemValue = '';
        }
    }
};
</script>
