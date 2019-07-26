<template>
    <div class="editdetail">
        <div class="detaillist">
            <span class="title">产品编号：</span>
            <div class="contant">
                <a-input class="einput" placeholder="请输入" v-model="prodata.num"></a-input>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">产品名称：</span>
            <div class="contant">
                <a-input class="einput" placeholder="请输入" v-model="prodata.name"></a-input>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">产品介绍：</span>
            <div class="contant">
                <a-textarea class="einput" placeholder="请输入" v-model="prodata.briefinfo" :rows="4" :autosize="false"></a-textarea>
            </div>
        </div>

        <div class="detaillist">
            <span class="title">产品图片：</span>
            <div class="contant">
                
            </div>
        </div>

        <div class="detaillist">
            <span class="title">产品定价：</span>
            <div class="contant">
                <a-input class="einput" placeholder="请输入" v-model="prodata.price"></a-input>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">单位：</span>
            <div class="contant">
                <a-select v-model="prodata.unit" class="einput" placeholder="请选择">
                    <a-select-option v-for="item in unitopo"  :key="item.val">{{item.label}}</a-select-option>
                </a-select>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">库存：</span>
            <div class="contant">
                <a-input class="einput" placeholder="请输入" v-model="prodata.stock"></a-input>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">所属店铺：</span>
            <div class="contant">
                <a-select
                    class="einput"
                    v-model="prodata.store"
                    placeholder="请选择分馆"
                    showSearch
                    optionFilterProp="children"
                    :filterOption='filterOption'
                >
                    <a-select-option v-for="(item,index) in branchlist" :key="index" :value='item.val'>{{item.label}}</a-select-option>
                </a-select>
            </div>
        </div>
        <div class="detaillist">
            <span class="title">是否上架：</span>
            <div class="contant">
                <a-radio-group class="state" v-model="prodata.state">
                    <a-radio :value="1">正常</a-radio>
                    <a-radio :value="2">冻结</a-radio>
                </a-radio-group>
            </div>
        </div>
        <div class="btnlist">
            <a-button type="primary" @click.native="save">保存</a-button>
            <a-button @click.native="cencal" style="margin-left:20px;">取消</a-button>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            prodata:{//输入框绑定的值
                num:"",
                name:"",
                briefinfo:"",

                price:"",
                unit:"",
                stock:"",
                store:"",
                state:1
            },
            unitopo:[//单位select的选项数据
                {
                    val:'1',
                    label:"KG"
                },
                {
                    val:'2',
                    label:"个"
                }
            ],
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
            ]
        }
    },
    methods:{
        filterOption(input, option) {//根据输入项进行筛选
            return option.componentOptions.children[0].text.toLowerCase().indexOf(input.toLowerCase()) >= 0
        },
        save(){//点击保存按钮的回调
            console.log(this.prodata)
        },
        cencal(){//点击取消的回调
            this.$router.back(-1);
        }
    }
}
</script>
<style lang="less" scoped>
.editdetail{
    background: #fff;
    box-sizing: border-box;
    padding: 40px 40px;
    font-size: 16px;
    .detaillist{
        overflow: hidden;
        margin: 35px 0;
        .title{
            display: block;
            width: 35%;
            text-align: right;
            float: left;
            line-height: 32px;
            font-weight: 600;
            box-sizing: border-box;
            padding-right: 10px;
        }
        .contant{
            width: 65%;
            float: right;
            .einput{
                width: 60%;
            }
            .state{
                line-height: 32px;
            }
        }
    }
    .btnlist{
        margin: 100px auto;
        text-align: center;
    }
}
</style>
