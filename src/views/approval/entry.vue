<template>
    <div class="entry_approval">
        <div class="search">
            <a-input-search
                placeholder="请按商品编号/产品名称/所属店铺搜索"
                style="width:330px;float:right;"
                @search="searchnum"
            />
        </div>
        <div class="main">
            <a-table class="detaillist" :columns="columns" :dataSource="tabdata" :pagination="false">
                <span slot="ope" slot-scope="text,record">
                    <a @click.prevent="examine(record)">审核</a>
                </span>
            </a-table>
            <a-pagination
                class="fy"
                :total="pagedata.total"
                :showTotal="total => `总共 ${total} 条`"
                :pageSize="pagedata.pagesize"
                :current="pagedata.current"
                :defaultCurrent="1"
                :hideOnSinglePage='true'
            />
        </div>

        <!-- 审核的弹窗 -->
        <a-modal
            v-model="examinedata.show"
            wrapClassName="approval_examine"
            :title='examinedata.title'
            width='800px'
            :centered='true'
            @ok="examinecheck"
        >
            <ul class="examinelist">
                <li>
                    <span class="title">是否通过：</span>
                    <a-select v-model="examinedata.result" class="einput" placeholder="请选择">
                        <a-select-option v-for="item in examineopo"  :key="item.val">{{item.label}}</a-select-option>
                    </a-select>
                </li>
                <li>
                    <span class="title">理由：</span>
                    <a-textarea v-model="examinedata.reason" class="einput" placeholder="请输入备注" :rows="4" :autosize="false" />
                </li>
            </ul>
        </a-modal>
    </div>
</template>
<script>
export default {
    data(){
        return{
            columns:[
                {
                    title:"入职员工",
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
                    title:"上级业务员",
                    dataIndex: 'supname',
                    key: 'supname',
                    align:"center",
                },
                {
                    title:"上级手机号码",
                    dataIndex: 'suptel',
                    key: 'suptel',
                    align:"center",
                },
                {
                    title:"入职时间",
                    dataIndex: 'entrytime',
                    key: 'entrytime',
                    align:"center",
                },
                {
                    title:"所属分店",
                    dataIndex: 'store',
                    key: 'store',
                    align:"center",
                },
                {
                    title: '操作',
                    key: 'ope',
                    scopedSlots: { customRender: 'ope' },
                    align:"center",
                }
            ],
            tabdata:[
                {
                    key:"1",
                    id:"36",
                    name:"张小伟",
                    tel:"15658556234",
                    supname:"陈美琳",
                    suptel:"15332669564",
                    entrytime:"2019-03-25",
                    store:"杭州分店"
                }
            ],
            pagedata:{//分页数据
                pagesize:5,
                current:1,
                total:100//总条数
            },
            examinedata:{//审核弹窗的配置
                show:false,
                title:"审核结果",
                data:{},
                result:"",
                reason:""
            },
            examineopo:[//审核通过select的选项数据
                {
                    val:'1',
                    label:"是"
                },
                {
                    val:'2',
                    label:"否"
                }
            ],
        }
    },
    methods:{
        searchnum(val){//右上方搜索框的方法

        },
        examine(record){//列表点击审核按钮的方法
            this.examinedata.show=true;
            this.examinedata.data=record;
        },
        examinecheck(){//审核弹窗的确定按钮
            console.log(this.examinedata)
        }
    }
}
</script>
<style lang="less" scoped>
.entry_approval{
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
        .fy{
            margin-top: 50px;
            text-align: center;
        }
    }
}
</style>
