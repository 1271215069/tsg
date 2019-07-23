<template>
    <div class="shopmanage">
        <div class="shopinfo"><!-- 页面上方店铺信息 -->
            <div class="toptitle">
                <a-menu
                class="title"
                :selectedKeys="['mail']"
                mode="horizontal">
                    <a-menu-item key="mail">
                        店铺信息
                    </a-menu-item>
                </a-menu>
                <a-button class="editshop" type="primary" @click.native="showedit">编辑</a-button>
            </div>
            <div class="shopcontant">
                <div class="left">
                    <img :src="shopcontant.imgurl" alt="">
                </div>
                <ul class="right">
                    <li>
                        <span class="rtitle">店铺名称：</span>
                        <span class="rcon">{{shopcontant.name}}</span>
                    </li>
                    <li>
                        <span class="rtitle">店铺地址：</span>
                        <span class="rcon">{{shopcontant.address}}</span>
                    </li>
                    <li>
                        <span class="rtitle">店铺负责人：</span>
                        <span class="rcon">{{shopcontant.person}}</span>
                    </li>
                    <li>
                        <span class="rtitle">联系方式：</span>
                        <span class="rcon">{{shopcontant.tel}}</span>
                    </li>
                    <li class="qrlist">
                        <span class="rtitle">首付款二维码：</span>
                        <div class="qrimg">
                            <img :src="shopcontant.qrcode[0]" alt="">
                            <p>微信二维码</p>
                        </div>
                        <div class="qrimg">
                            <img :src="shopcontant.qrcode[1]" alt="">
                            <p>支付宝二维码</p>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        <a-modal
            v-model="editshop.show"
            wrapClassName="shopmanage_editshop"
            title='店铺信息'
            width='800px'
        >
            <ul class="editlist">
                <li>
                    <span class="title">店铺名称：</span>
                    <a-input class="einput" placeholder="请输入" v-model="editshop.name"></a-input>
                </li>
            </ul>
        </a-modal>
    </div>
</template>
<script>
export default {
    name:'shopmanage',
    data(){
        return{
            shopcontant:{//店铺信息的数据
                imgurl:require('@/assets/img/u77.jpg'),
                name:"宁海高校管理（总店）",
                address:"宁波市宁海县岔路镇上金·财神谷",
                person:"蔡晓芬",
                tel:"15958635843",
                qrcode:[require('@/assets/img/nullimg.png'),require('@/assets/img/nullimg.png')]
            },
            editshop:{//修改店铺弹窗的信息
                show:false,
                photo:require('@/assets/img/u77.jpg'),
                name:"宁海高校管理（总店）",
                address:"宁波市宁海县岔路镇上金·财神谷",
                person:"蔡晓芬",
                tel:"15958635843",
                qrcode:[require('@/assets/img/nullimg.png'),require('@/assets/img/nullimg.png')]
            },
            
        }
    },
    methods:{
        showedit(){//店铺信息点击编辑按钮的方法
            this.editshop.show=!this.editshop.show;
        }
    }
}
</script>
<style lang="less" scoped>
.shopmanage{
    background: #fff;
    font-size: 16px;
    .shopinfo{
        box-sizing: border-box;
        padding: 20px;
        .toptitle{
            position: relative;
            .editshop{
                position: absolute;
                right: 20px;
                top: 50%;
                transform: translateY(-50%);
            }
        }
        .shopcontant{
            overflow: hidden;
            .left{
                float: left;
                width: 17%;
                img{
                    display: block;
                    width: 150px;
                    height: 150px;
                    margin: 30px auto 0;
                    border-radius: 50%;
                }
            }
            .right{
                float: right;
                width: 83%;
                list-style: none;
                overflow: hidden;
                margin: 30px 0 0;
                li{
                    width: 35%;
                    float: left;
                    line-height: 45px;
                    font-size: 14px;
                    .rtitle{
                        font-weight: 600;
                    }
                }
                .qrlist{
                    overflow: hidden;
                    .rtitle{
                        float: left;
                    }
                    .qrimg{
                        float: left;
                        width: 120px;
                        img{
                            width: 80%;
                            margin: 10px auto 0;
                            display: block;
                        }
                        p{
                            text-align: center;
                        }
                    }
                }
            }
        }
    }
    .editshop{
        .editlist{
            background: #000;
        }
    }

}
</style>
