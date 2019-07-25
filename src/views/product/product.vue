<template>
    <div class="product">
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
            <a-button type="primary" style="float:right;margin-left:20px;">新增</a-button>
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            <a-table class="protable" :columns="columns" :dataSource="tabdata" :pagination="false">
                
                <span slot="ope" slot-scope="text,record">
                    <a v-html="record.statecode==1?'下架':'上架'" @click.prevent="record.statecode==1? shelf(2,record):shelf(1,record)"></a>
                    <a-divider type="vertical" />
                    <a @click.prevent="del(record)">删除</a>
                    <a-divider type="vertical" />
                    <a @click.prevent="detail(record)">详情</a>
                </span>
            </a-table>
        </div>
        <a-modal v-model="shelfdata.show" :title="'产品'+shelfdata.text" centered @ok="shelfok">
            <div style="text-align:center;">确定对该产品{{shelfdata.text}}吗？</div>
        </a-modal>
        <a-modal v-model="deldata.show" title="删除产品" centered @ok="delok">
            <div style="text-align:center;">确定删除该产品吗？</div>
        </a-modal>
    </div>
</template>
<script>
export default {
    name:"product",
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
            columns:[//table的表头
                {
                    title:"编号",
                    dataIndex: 'num',
                    key: 'num',
                    align:"center",
                },
                {
                    title:"产品名称",
                    dataIndex: 'name',
                    key: 'name',
                    align:"center",
                },
                {
                    title:"单位",
                    dataIndex: 'unit',
                    key: 'unit',
                    align:"center",
                },
                {
                    title:"库存",
                    dataIndex: 'stock',
                    key: 'stock',
                    align:"center",
                },
                {
                    title:"价格（元）",
                    dataIndex: 'price',
                    key: 'price',
                    align:"center",
                },
                {
                    title:"所属店铺",
                    dataIndex: 'store',
                    key: 'store',
                    align:"center",
                },
                {
                    title:"状态",
                    dataIndex: 'state',
                    key: 'state',
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[//table内的数据
                {
                   key:"1",
                   id:3589,
                   num:"358080",
                   name:"紫砂壶",
                   unit:"个",
                   stock:"102个",
                   price:"6666/个",
                   store:"杭州分店",
                   state:"上架",
                   statecode:1,//1为上架，2为下架
                }
            ],
            shelfdata:{//上下架弹窗的配置
                 show:false,
                 data:{},//点击的这一条的数据
                 edittype:'',//需要修改上下架的值
                 text:"",//显示的标题
            },
            deldata:{//删除弹窗的配置
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
        del(record){//点击删除的方法
            this.deldata.show=true;
            this.deldata.data=record;
        },
        delok(){
            console.log('确认删除了')
        },
        detail(record){//点击详情的按钮
            this.$router.push({name:"prodetail"})
        },
        shelf(statecode,record){//显示上下架弹窗并传参
            this.shelfdata.show=true;
            this.shelfdata.date=record;
            this.shelfdata.edittype=statecode;
            if(statecode==1){
                this.shelfdata.text="上架";
            }else if(statecode==2){
                this.shelfdata.text="下架";
            }
        },
        shelfok(){//上下架弹窗确认按钮
            console.log('确认上下架了')
        }
    }
}
</script>
<style lang="less" scoped>
.product{
    background: #fff;
    box-sizing: border-box;
    padding: 20px;
    font-size: 16px;
    .search{
        overflow: hidden;
    }
    .main{
        .protable{
            margin: 30px 0;
        }
    }
}
</style>
