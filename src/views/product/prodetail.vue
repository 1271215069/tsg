<template>
    <div class="prodetail">
        <p class="moduletitle">
            <span class="text">产品详情</span>
            <a-button class="edit" type="primary" @click.native="edit">编辑</a-button>
        </p>
        <div class="listitem">
            <span class="title">
                产品编号：
            </span>
            <div class="contant">
                {{prodata.num}}
            </div>
        </div>
        <div class="listitem">
            <span class="title">
                产品名称：
            </span>
            <div class="contant">
                {{prodata.name}}
            </div>
        </div>
        <div class="listitem">
            <span class="title">
                产品简介：
            </span>
            <div class="contant">
                {{prodata.briefinfo}}
            </div>
        </div>
        <div class="listitem">
            <span class="title">
                产品图片：
            </span>
            <div class="contant imglist">
                <img v-if="prodata.img[0]" :src="prodata.img[0]" alt="">
                <img v-if="prodata.img[1]" :src="prodata.img[1]" alt="">
            </div>
        </div>
        <p class="moduletitle" style="font-size:18px;">
            <span class="text">产品定价：¥{{prodata.price}}/{{prodata.unit}}</span>
        </p>
        <ul class="detailbox">
            <li class="detailliat">
                <span class="title">单位：</span>
                {{prodata.unit}}
            </li>
            <li class="detailliat">
                <span class="title">库存：</span>
                {{prodata.stock}}
            </li>
            <li class="detailliat">
                <span class="title">状态：</span>
                {{prodata.state}}
            </li>
            <li class="detailliat">
                <span class="title">所属店铺：</span>
                {{prodata.store}}
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    data(){
        return{
            prodata:{
                num:"3588",
                name:"紫砂壶",
                briefinfo:"紫砂罐源自于宁海县岔路镇上金村的一座山峰,紫砂罐源自于宁海县岔路镇上金村的一座山峰,紫砂罐源自于宁海县岔路镇上金村的一座山峰,紫砂罐源自于宁海县岔路镇上金村的一座山峰",
                img:[
                    require("@/assets/img/l1.jpg"),
                    require("@/assets/img/l2.jpg")
                ],
                price:"568.05",
                unit:"个",
                stock:"6842",
                state:"上架",
                store:"杭州分店"
            }
        }
    },
    methods:{
        edit(){
            this.$router.push({
                name:"editdetail",
                query: {
                    id: '12',
                    type:"edit"
                }
            })
        }
    }
}
</script>
<style lang="less" scoped>
.prodetail{
    background: #fff;
    box-sizing: border-box;
    padding: 40px 40px;
    font-size: 16px;
    .moduletitle{
        overflow: hidden;
        .text{
            font-size: 18px;
            font-weight: 600;
            display: block;
            float: left;
        }
        .edit{
            float: right;
        }
    }
    .listitem{
        overflow: hidden;
        margin: 25px 0;
        .title{
            display: block;
            float: left;
            width: 100px;
            text-align: right;
            font-weight: 600;
            line-height: 30px;
            margin-right: 10px;
        }
        .contant{
            float: left;
            width: 50%;
            line-height: 30px;
        }
        .imglist{
            img{
                width: 260px;
                height: 160px;
                margin-right: 30px;
            }
        }
    }
    .detailbox{
        padding-left: 20px;
        overflow: hidden;
        width: 60%;
        .detailliat{
            list-style: none;
            width: 50%;
            float: left;
            line-height: 30px;
            margin: 10px 0;
            .title{
                font-weight: 600;
                margin-right: 10px;
            }
        }
    }
}
</style>
