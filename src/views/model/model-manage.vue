
<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 同步数据
            </h4>
            <Row>
                <Col span="6" >
                    <Card style="width:calc(100% - 15px);height: 800px;">
                        <p slot="title">
                            分组
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Input v-model="search.tree_name" icon="search" placeholder="输入关键字" style="width: 100%" />
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <my-tree 
                                :list="tree.treeList" 
                                :isRightAction="true" 
                                :parentAction="tree.parentAction" 
                                :childAction="tree.childAction" 
                                :form='modalForm'
                                :modalFormValidate='modalFormValidate'
                                @clickFun='synchroData'></my-tree>
                            </Col>
                        </Row>
                    </Card>
                </Col>
                <Col span="18">
                    <Card style="width:calc(100% - 15px);height: 800px;margin-left: 7px;">
                        <p slot="title">
                           设备详情
                        </p>
                        <div v-if="synchro_loading" style="position:absolute;width: 98%;height: calc(100% - 70px);z-index:2">
                            <Spin fix>
                                <Icon type="load-c" size=18 class="demo-spin-icon-load"></Icon>
                                <div>正在同步，请稍等。。。</div>
                            </Spin>
                        </div>
                        <Row style="margin-bottom: 10px;z-index:0;position:relative;">
                            <Col span="6">
                                <Input icon="search" placeholder="设备名称" style="width: 100%" />
                            </Col>
                            <Col span="18" style="text-align: right">
                                <Button type="primary" size="large" @click="tc">新增设备</Button>
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24" style="position:relative;z-index:0;">
                                <i-switch v-model="switch1" style="position:absolute; top: 9px;left:15px;z-index:1;" @on-change="changeWitch"> 
                                    <span slot="open">开</span>
                                    <span slot="close">关</span>
                                </i-switch>
                                <Table height='670' :columns="InternetTit" :data="InternetData"></Table>
                                <!-- <Page class="page-position" :current="page.pageIndex" :total="page.totalItemCount" show-total :page-size='page.pageSize'></Page> -->
                            </Col>
                        </Row>
                    </Card>
                </Col>
            </Row>
        </Card>
         <Modal v-model="editPasswordModal" :closable='false' :mask-closable=false :width="500">
            <h3 slot="header" style="color:#2D8CF0">{{vlTit}}</h3>
            <Form :label-width="80" label-position="right" >
                <FormItem label="名称：" >
                    <Input  placeholder="请输入设备名称" ></Input>
                </FormItem>
                 <FormItem label="采集周期：">
                    <Input  placeholder="请输入采集周期" ><span slot="append">秒</span></Input>
                </FormItem>
                 <FormItem label="设备描述：">
                    <Input  type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入描述信息"></Input>
                </FormItem>
            </Form>
            <div slot="footer">
                <Button type="text" @click="editPasswordModal = false">取消</Button>
                <Button type="primary" @click="editPasswordModal = false">保存</Button>
            </div>
        </Modal>
        <Modal v-model="updPasswordModal" :closable='false' :mask-closable=false :width="500">
            <h3 slot="header" style="color:#2D8CF0">{{vlTit}}</h3>
            <Form :label-width="80" label-position="right" >
                <FormItem label="名称：" >
                    <Input  placeholder="请输入变量名称" ></Input>
                </FormItem>
                <FormItem label="采集周期：">
                    <Input  placeholder="请输入采集周期" ><span slot="append">秒</span></Input>
                </FormItem>
                <FormItem label="变量描述：">
                    <Input  type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入描述信息"></Input>
                </FormItem>
                
            </Form>
            <div slot="footer">
                <Button type="text" @click="updPasswordModal = false">取消</Button>
                <Button type="primary"  @click="updPasswordModal = false">保存</Button>
            </div>
        </Modal>
    </div>
</template>

<script>
import myTree from '../main-components/tree/my-tree.vue';
import expandRow from '../main-components/table/table-expand.vue';

export default {
    components: {
        myTree,
        expandRow
    },
    name: '',
    data () {
        return {
            vlTit: '',
            switch1: true,
            editPasswordModal: false,
            updPasswordModal: false,
            renderTable: [
                {
                    title: '序号',
                    type: 'index',
                    width: 80,
                    align: 'center'
                },
                {
                    title: '变量名称',
                    key: 'variableName',
                    align: 'center'
                }, {
                    title: '变量描述',
                    key: 'variableDescribe',
                    align: 'center'
                }, {
                    title: '操作',
                    key: 'action',
                    align: 'center',
                    width:150,
                    render: (h, params) => {
                        return h('div', {
                            style: {
                            // textAlign: 'center'
                            }
                        },[
                            h('a',{
                                style: {
                                marginRight: '10px'
                                },
                                on: {
                                click: () => {
                                     this.vlTit = params.row.deviceName + ' - 修改变量信息'
                                    this.updPasswordModal = true
                                }
                            }
                            }, '修改'),
                            h('a', {
                                style: {
                                color: 'red'                                },
                                on: {
                                    click: () => {
                                        this.remove(params.index)
                                    }
                                }
                                }, '删除 ')
                            ]);
                        }
                }
            ],
            InternetData: [],
            InternetTit: [
                {
                    type: 'expand',
                    width: 80,
                    render: (h, params) => {
                        return h(expandRow, {
                            props: {
                                row: params.row
                            }
                        })
                    }
                },
                {
                    title: '设备ID',
                    key: 'deviceId',
                    align: 'center'
                },
                {
                    title: '设备名称',
                    key: 'deviceName',
                    align: 'center'
                },
                {
                    title: '采集周期(秒)',
                    key: 'acquisitionCycle',
                    align: 'center'
                },
                {
                    title: '操作',
                    key: 'action',
                    align: 'center',
                    width:265,
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
                                    this.vlTit = params.row.deviceName + ' - 新增变量信息'
                                    this.updPasswordModal = true
                                }
                            }
                            }, '新增变量'),
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
                                    this.vlTit = ' 修改设备信息'
                                    this.editPasswordModal = true
                                }
                            }
                            }, '修改'),
                            h('Button', {
                                props: {
                                type: 'error',
                                size: 'small'
                                },
                                on: {
                                    click: () => {
                                        this.remove(params.index)
                                    }
                                }
                                }, '删除 ')
                            ]);
                        }
                }
            ],
            page: {
                pageIndex: 1,
                totalItemCount: 100,
                pageSize: 50
            },
            tree: {
                parentAction: [
                    {title:'添加子分组',registerFuntion: 'addObj'},
                    {title:'重命名',registerFuntion: 'updObj'},
                    {title:'删除',registerFuntion: 'delObj'}
                ],
                childAction: [
                    {title:'添加子分组',registerFuntion: 'addObj'},
                    {title:'重命名',registerFuntion: 'updObj'},
                    {title:'删除',registerFuntion: 'delObj'}
                ],
                treeList: [
                    {
                        id: 1,
                        parentID: 0,
                        title: 'XXXX厂',
                        isSelected: true,
                        isActionShow: false,
                        isHover: false,
                        children: [
                            {
                                id: 10001,
                                parentID: 1,
                                title: '污水处理区1',
                                isSelected: false,
                                isActionShow: false,
                                isHover: false,
                                children: [
                                    {
                                        id: 1000111,
                                        parentID: 10001,
                                        title: '转速组1',
                                        isSelected: false,
                                        isActionShow: false,
                                        isHover: false
                                    },
                                    {
                                        id: 1000111,
                                        parentID: 10001,
                                        title: '转速组2',
                                        isSelected: false,
                                        isActionShow: false,
                                        isHover: false
                                    }
                                ]
                            },
                            {
                                id: 10002,
                                parentID: 1,
                                title: '污水处理区2',
                                isSelected: false,
                                isActionShow: false,
                                isHover: false
                            }
                        ]
                    },  {
                        id: 2,
                        parentID: 0,
                        title: 'XX村',
                        isSelected: false,
                        isActionShow: false,
                        isHover: false,
                        children: [
                            {
                                id: 20001,
                                parentID: 2,
                                title: '污水处理区3',
                                isSelected: false,
                                isActionShow: false,
                                isHover: false
                            },
                            {
                                id: 20002,
                                parentID: 2,
                                title: '污水处理区4',
                                isSelected: false,
                                isActionShow: false,
                                isHover: false
                            }
                        ]
                    }
                ],
            },
            search: {
                tree_name: ''
            },
            modalForm: [
                {
                    name: '分组名称',
                    key: 'name',
                    classParent: 'form-row',
                    inputType: 'text'
                }
            ],
            modalFormValidate: {
                name: [
                    { required: true, message: '请输入分组名称', trigger: 'blur' }
                ]
            },
            synchro_loading: false
        };
    },
    mounted () {
        this.InternetData =  [
            {
                deviceId: 100001,
                deviceName: '设备1',
                acquisitionCycle: 20,
                _expanded: true,
                list: {
                    tit: this.renderTable,
                    data:  [
                        {
                            deviceId: 100001,
                            deviceName: '设备1',
                            variableName: '变量11',
                            SN: 1,
                            ID: 1,
                            variableDescribe: 2,
                            mapVariable: 1222
                        },
                        {
                            deviceId: 100001,
                            deviceName: '设备1',
                            variableName: '变量12',
                            SN: 1,
                            ID: 1,
                            variableDescribe: 2,
                            mapVariable: '变量11'
                        },
                        {
                            deviceId: 100001,
                            deviceName: '设备1',
                            variableName: '变量12',
                            SN: 1,
                            ID: 1,
                            variableDescribe: 2,
                            mapVariable: ''
                        }
                    ]
                }
            },{
                deviceId: 100002,
                deviceName: '设备2',
                acquisitionCycle: 20,
                list: {
                    tit: this.renderTable,
                    data:  [
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量21',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        },
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量22',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        }
                    ]
                }
            },{
                deviceId: 100003,
                deviceName: '设备3',
                acquisitionCycle: 20,
                list: {
                    tit: this.renderTable,
                    data:  [
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量21',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        },
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量22',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        }
                    ]
                }
            },{
                deviceId: 100004,
                deviceName: '设备4',
                acquisitionCycle: 20,
                list: {
                    tit: this.renderTable,
                    data:  [
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量21',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        },
                        {
                            deviceId: 100002,
                            deviceName: '设备2',
                            variableName: '变量22',
                            variableDescribe: 2,
                            SN: 1,
                            ID: 1,
                            mapVariable: 1222
                        }
                    ]
                }
            }
        ];
         if (this.switch1) {
            this.InternetData.map(one => {
                one._expanded = true
            })
        } else {
            this.InternetData.map(one => {
                one._expanded = false
            })
        }
    },
    methods: {
        tc () {
            this.editPasswordModal = true
            this.vlTit = '新增设备信息'
        },
        changeWitch (val) {
            if (val) {
                this.InternetData.map(one => {
                    one._expanded = true
                })
            } else {
                this.InternetData.map(one => {
                    one._expanded = false
                })
            }
        },
        synchroData (data) {
            this.synchro_loading = true
            setTimeout(() => {
                this.synchro_loading = false
            }, 1000)
        }
    }
};
</script>
