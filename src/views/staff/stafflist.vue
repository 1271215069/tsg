<template>
    <div class="stafflist">
        <div class="search">
            <a-select
                v-model="chosestore"
                placeholder="请选择分馆"
                showSearch
                optionFilterProp="children"
                :filterOption='filterOption'
                style="width: 200px;float:left;"
                @change="searchstore"
            >
                <a-select-option v-for="(item,index) in branchlist" :key="index" :value='item.val'>{{item.label}}</a-select-option>
            </a-select>
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            <a-table class="detaillist" :columns="columns" :dataSource="tabdata" :pagination="false">
                <span slot="member" slot-scope="text,record">
                    <a @click.prevent="lookup(record)">查看</a>
                </span>
                <span slot="ope" slot-scope="text,record">
                    <a @click.prevent="godetail(record)">详情</a>
                    <a-divider type="vertical" />
                    <a v-if="record.frozen=='0'" @click.prevent="showfrozen(record)">冻结</a>
                    <a v-if="record.frozen=='1'" @click.prevent="showfrozen(record)">解冻</a>
                </span>
            </a-table>
        </div>
        <!-- 确认冻结弹窗 -->
        <a-modal v-model="frozendata.show" :title="frozendata.title" centered @ok="frozenok">
            <div style="text-align:center;">确定对该员工账号进行{{frozendata.text}}吗？</div>
        </a-modal>
        <!-- 团队成员查看弹窗 -->
        <a-modal    
            v-model="memberdata.show"
            wrapClassName="staff_member"
            width='900px'
            title="团队成员"
            :centered='true'
            :footer="null"
            :zIndex='1100'
            >
            <div class="memberdetil">
                <div class="supperson">
                    <p>
                        <span class="title">上级业务员：</span>林梅西
                    </p>
                    <p>
                        <span class="title">上上级业务员：</span>陈丽梅
                    </p>
                </div>
                <div class="subperson">
                    <span class="title">下级业务员：</span>
                    <ul class="sublist">
                        <li>陈丽妃（提供收益：25894.53元，下级：490个）<a-icon @click.native="showmedetail" type="down-square" style="color:#1890FF;cursor: pointer;"/></li>
                        <li>陈丽妃（提供收益：25894.53元，下级：490个）<a-icon @click.native="showmedetail" type="down-square" style="color:#1890FF;cursor: pointer;"/></li>
                    </ul>
                </div>
              </div>
        </a-modal>
        <!-- 下下级成员查看弹窗 -->
        <a-modal
            v-model="medetaildata.show"
            wrapClassName="staff_member"
            width='800px'
            :centered='true'
            :footer="null"
            :zIndex='1200'
        >
            <ul class="subperlist">
                <li>陈丽妃（提供收益：25894.53元）</li>
                <li>陈丽妃（提供收益：25894.53元）</li>
            </ul>
        </a-modal>
    </div>
</template>
<script>
export default {
    data(){
        return{
            chosestore:"",//分店的选中值绑定
            branchlist:[//分店select的可选项
                {
                    val:'1',
                    label:"杭州分店"
                },
                {
                    val:'2',
                    label:"宁波分店"
                },
                {
                    val:'3',
                    label:"上海分店"
                },
            ],
            columns:[//表格头部数据
                {
                    title:"员工姓名",
                    dataIndex: 'name',
                    key: 'name',
                    align:"center",
                },
                {
                    title:"手机号码",
                    dataIndex: 'tel',
                    key: 'tel',
                    align:"center",
                },
                {
                    title: '团队成员',
                    key: 'member',
                    scopedSlots: { customRender: 'member' },
                    align:"center",
                },
                {
                    title:"业务等级",
                    dataIndex: 'levl',
                    key: 'levl',
                    align:"center",
                },
                {
                    title:"个人总业绩（元）",
                    dataIndex: 'achieve',
                    key: 'achieve',
                    align:"center",
                },
                {
                    title:"个人总订单（件）",
                    dataIndex: 'ordernum',
                    key: 'ordernum',
                    align:"center",
                },
                {
                    title:"回款率",
                    dataIndex: 'moneyback',
                    key: 'moneyback',
                    align:"center",
                },
                {
                    title:"店内排名",
                    dataIndex: 'ranking',
                    key: 'ranking',
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[//表格列表数据
                {
                    key:'1',
                    id:'20',
                    name:'杨大礼',
                    tel:'15959400909',
                    levl:'Lv.5',
                    achieve:'300000.00',
                    ordernum:'3495',
                    moneyback:'100%',
                    ranking:'1',
                    frozen:'0',//员工是否冻结，0正常，1冻结
                },
                {
                    key:'2',
                    id:'20',
                    name:'杨大礼',
                    tel:'15959400909',
                    levl:'Lv.5',
                    achieve:'300000.00',
                    ordernum:'3495',
                    moneyback:'100%',
                    ranking:'1',
                    frozen:'1',//员工是否冻结，0正常，1冻结
                }
            ],
            frozendata:{//冻结弹窗配置
                show:false,
                data:{},
                title:"",
                text:""
            },
            memberdata:{//团队成员弹窗配置
                show:false,
                data:{}
            },
            medetaildata:{//下下级业务员弹窗配置
                show:false,
                data:{}
            }
        }
    },
    methods:{
        filterOption(input, option) {//根据输入项进行筛选
            return option.componentOptions.children[0].text.toLowerCase().indexOf(input.toLowerCase()) >= 0
        },
        searchstore(val,opt){//获取到输入框值改变的回调,执行查询更新table的数据
            console.log(val,666)
            console.log(opt,333)
        },
        searchnum(val){//点击右侧搜索图标的回调
            console.log(val,555)
        },
        showfrozen(record){//列表点击冻结的按钮回调
            this.frozendata.show=true;
            if(record.frozen=='0'){
                this.frozendata.title="冻结员工";
                this.frozendata.text="冻结";
            }else if(record.frozen=='1'){
                this.frozendata.title="解冻员工";
                this.frozendata.text="解冻";
            }
            this.frozendata.data=record;
        },
        frozenok(){//确认冻结或解冻的方法

        },
        godetail(record){//列表点击详情的按钮回调
            this.$router.push({
                name:"staffdetail",
                query:{
                    id:record.id
                }
            })
        },
        lookup(record){//列表点击查看的按钮回调
            this.memberdata.show=true;
            this.memberdata.data=record;
        },
        showmedetail(){//弹窗内点击查看下下级业务员的按钮方法
            this.medetaildata.show=true;
        },
        
    }
}
</script>
<style lang="less" scoped>
.stafflist{
    background: #fff;
    box-sizing: border-box;
    padding: 20px;
    font-size: 16px;
    .search{
        overflow: hidden;
    }
    .main{
        .detaillist{
            margin: 30px 0;
        }
    }
}
</style>
    