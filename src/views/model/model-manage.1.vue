
<template>
    <div class="warp-main">
        <Card>
            <h4 class="title-h4"  slot="title">
                <Icon type="ios-paper-outline"></Icon> 模型管理
            </h4>
            <Row>
                <Col span="6">
                    <Card style="width:calc(100% - 15px);height: 600px;">
                        <p slot="title">
                            分组
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Input icon="search" placeholder="输入关键字" style="width: 100%" />
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                            <ul id="demo" v-for="data1 in searchList">
                                    <item
                                        class="item"
                                        :model="data1" @on-del="delitem" @on-show="showChild" @on-action="action">
                                    </item>
                                </ul>
                                <!-- <Tree :data="searchList"></Tree> -->
                            </Col>
                        </Row>
                    </Card>
                </Col>
                 <Col span="6" >
                    <Card style="width:calc(100% - 15px);height: 600px;margin-left: 7px;">
                        <p slot="title">
                           设备与数据包
                           <Button type="primary" style="width: 80px;position:absolute;right: 10px;top:10px;" @click="editPasswordModal = true">新增</Button>
                        </p>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <Input icon="search" placeholder="设备名称" style="width: 100%;position:relative" />
                                
                            </Col>
                        </Row>
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                                <div style="margin-bottom: 10px;text-align: center;position:relative">
                                     
                                   <Tabs active-key="key1"  type="card" style="margin-top: 5px;">
                                       
                                        <Tab-pane label="设备" key="key1">
                                             <!-- <div style="margin-bottom: 10px;">
                                                <Button type="primary" style="width: 100px;" @click="editPasswordModal = true">新增</Button>
                                                <Button type="success" style="width: 100px;"  @click="updPasswordModal = true">修改</Button>
                                                <Button type="error" style="width: 100px;"  @click="saveEdit">删除</Button>
                                            </div> -->
                                            <Table border :columns="InternetTit" :data="InternetData"></Table>
                                            <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                                        </Tab-pane>
                                        <Tab-pane label="数据包" key="key2">
                                            <!-- <div style="margin-bottom: 10px;">
                                                <Button type="primary" style="width: 100px;" @click="editPasswordModal = true">新增</Button>
                                                <Button type="success" style="width: 100px;"  @click="updPasswordModal = true">修改</Button>
                                                <Button type="error" style="width: 100px;"  @click="saveEdit">删除</Button>
                                            </div> -->
                                            <Table border :columns="InternetTit01" :data="InternetData01" style="margin-left:5px;" ></Table>
                                            <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                                        </Tab-pane>
                                    </Tabs>
                                </div>
                                
                            </Col>
                        </Row>
                    </Card>
                </Col>
                 <Col span="12" >
                    <Card style="width:calc(100% - 15px);height: 600px;margin-left: 15px;">
                        <p slot="title">
                            设备信息
                        </p>
                        <Row >
                            <Col span="24">
                                <!-- <div style="border-bottom: 1px solid #ccc;margin-bottom: 5px;">设备信息</div> -->
                                 <Form  class="d-form"
                                    ref="userForm"
                                    :model="userForm" 
                                    :label-width="150" 
                                    label-position="right"
                                    :rules="inforValidate"
                                >
                                    <FormItem label="ID：" >
                                        32
                                    </FormItem>
                                     <FormItem label="名称：" >
                                        水位监测设备1
                                    </FormItem>
                                     <FormItem label="采集周期：" >
                                        <div style="display:inline-block;width:200px;">
                                            <i-input>
                                                <span slot="append">秒</span>
                                            </i-input>
                                        </div>
                                    </FormItem>
                                </Form>
                                <!-- <div style="margin-bottom: 10px;text-align: center;">
                                    <Button type="primary" size="large" >变量启用</Button>
                                    <Button type="primary" size="large" >变量停用</Button>
                                </div> -->
                           
                            </Col>
                        </Row>
                        <div style="border-top: 1px solid #ccc;margin: 10px 0 15px;text-align:right;padding-top:15px;">
                            <Button type="primary" style="" @click="updPasswordModal = true">新增变量</Button>
                        </div>
                     
                        <Row style="margin-bottom: 10px;">
                            <Col span="24">
                             
                                <!-- <div style="margin-bottom: 10px;">
                                    
                                    <Button type="success" style="width: 100px;"  @click="updPasswordModal = true">修改</Button>
                                    <Button type="error" style="width: 100px;"  @click="saveEdit">删除</Button>
                                </div> -->
                                <Table border :columns="InternetTit2" :data="InternetData2"></Table>
                                <Page class="page-position" :current="pageIndex" :total="totalItemCount" show-total :page-size='pageSize'></Page>
                            </Col>
                        </Row>
                    </Card>
                </Col>
            </Row>
            
        </Card>
        <Modal v-model="editPasswordModal" :closable='false' :mask-closable=false :width="500">
            <h3 slot="header" style="color:#2D8CF0">新增设备信息</h3>
            <Form :label-width="150" label-position="right" >
                <FormItem label="名称：" >
                    <Input  placeholder="请输入设备名称" ></Input>
                </FormItem>
                <FormItem label="描述：">
                    <Input  type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入描述信息"></Input>
                </FormItem>
                <FormItem label="采集周期（单位/秒）:">
                    <Input  placeholder="请再次采集周期" ></Input>
                </FormItem>
            </Form>
            <div slot="footer">
                <Button type="text" @click="editPasswordModal = false">取消</Button>
                <Button type="primary" :loading="savePassLoading" @click="editPasswordModal = false">保存</Button>
            </div>
        </Modal>
        <Modal v-model="updPasswordModal" :closable='false' :mask-closable=false :width="500">
            <h3 slot="header" style="color:#2D8CF0">新增变量信息</h3>
            <Form :label-width="150" label-position="right" >
                <FormItem label="名称：" >
                    <Input  placeholder="请输入变量名称" ></Input>
                </FormItem>
                <FormItem label="描述：">
                    <Input  type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入描述信息"></Input>
                </FormItem>
                <FormItem label="采集周期（单位/秒）:">
                    <Input  placeholder="请再次采集周期" ></Input>
                </FormItem>
            </Form>
            <div slot="footer">
                <Button type="text" @click="updPasswordModal = false">取消</Button>
                <Button type="primary" :loading="savePassLoading" @click="updPasswordModal = false">保存</Button>
            </div>
        </Modal>
    </div>
</template>

<script>
import item from '../tree/item.vue';
import {InternetData, InternetTit} from '../datas/tableData.js';
export default {
     components: {
        item
    },
    name: '',
    data () {
         const validePhone = (rule, value, callback) => {
            var re = /^1[0-9]{10}$/;
            if (!re.test(value)) {
                callback(new Error('请输入正确格式的手机号'));
            } else {
                callback();
            }
        };
        const valideRePassword = (rule, value, callback) => {
            if (value !== this.editPasswordForm.newPass) {
                callback(new Error('两次输入密码不一致'));
            } else {
                callback();
            }
        };
        return {
            editPasswordModal: false,
            updPasswordModal: false,
            inforValidate: {
                name: [
                    { required: true, message: '请输入姓名', trigger: 'blur' }
                ],
                cellphone: [
                    { required: true, message: '请输入手机号码' },
                    { validator: validePhone }
                ]
            },
            userForm: {
                name: '',
                cellphone: '',
                company: '',
                department: ''
            },
            search_status: '',
             InternetTit01: [{
                key:'name',
                title: '数据包名称'
            }, {
            title: '操作',
            key: 'Operation',
            width: 150,
            align: 'center',
            render: (h, params) => {
                return h('div', [
                    h('Button', {
                        props: {
                        type: 'success',
                        size: 'small'
                        },
                        style: {
                        marginRight: '5px'
                        },
                        on: {
                        click: () => {
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
            }],
            InternetData01: [{
                name: '数据包名称1'
            }],
            InternetTit: [{
                key:'name',
                title: '设备名称'
            }, {
            title: '操作',
            key: 'Operation',
            width: 150,
            align: 'center',
            render: (h, params) => {
                return h('div', [
                    h('Button', {
                        props: {
                        type: 'success',
                        size: 'small'
                        },
                        style: {
                        marginRight: '5px'
                        },
                        on: {
                        click: () => {
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
            }],
            InternetData: [{
                name: '设备名称1'
            }],
            InternetTit2: [{
                key:'name',
                title: '变量名称'
            },{
                key:'txt',
                title: '变量描述'
            },{
                key:'txt1',
                title: '是否映射',
                width: 95
            }, {
        title: '操作',
        key: 'Operation',
        width: 125,
        align: 'center',
        render: (h, params) => {
            return h('div', [
                h('Button', {
                    props: {
                    type: 'success',
                    size: 'small'
                    },
                    style: {
                    marginRight: '5px'
                    },
                    on: {
                    click: () => {
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
            InternetData2: [{
                name: '变量1',
                txt: '变量描述',
                txt1: '是',
                ID: 1
            }],
            search_status_list: [],
            pageIndex: 1,
            totalItemCount: 0,
            pageSize: 2,
            searchList: [{
                id: '2',
                name: 'XXXX厂',
                expand: true,
                showAction: false,
                isEdit: false,
                actionList:[{
                    actionName: '添加子分组',
                    link: 'ioserver-add',
                    actionFlag: 'function'
                },{
                    actionName: '重命名',
                    link: 'ioserver-add',
                    actionFlag: 'function'
                },{
                    actionName: '删除',
                    link: 'ioserver-add',
                    actionFlag: 'function'
                }],
                children: [
                    {
                        name: '污水处理区2',
                        expand: true,
                        children: [
                            {
                                name: '转速组1',
                                showAction: false,
                                isEdit: false
                            },
                            {
                                name: '转速组2',
                                showAction: false,
                                isEdit: false
                            }
                        ]
                    },
                ]
            }]
        };
    },
    mounted () {
        this.totalItemCount =   this.InternetData.length
    },
    methods: {
      delitem (item) {
        console.log(item)
      },
      showChild (item) {
      },
      action (item) {
          console.log(item)
          if(item.actionFlag === 'url') {
             this.$router.push({name: item.link})
          }
      }
    }
};
</script>
