<!-- ---------------------------模板区--------------------------- -->
<template>
    <div>
        <div class="right-list-box">
            <!--第一个盒子: 联系方式 -->
            <section>
                <!-- 图片-->
                <div calass="right-head-1">
                    <img src="~@/assets/img/right/dm01.jpg">
                </div>

                <!--其它信息-->
                <div class="right-body-1">
                    <p>你能找到我么？</p>
                    <!--一些按钮-->
                    <div class="buttons">
                        <!--第一排按钮-->
                        <div>
                            <el-tooltip class="item" content="Github" placement="top">
                                <a href="https://github.com/sgiao" target="_blank"><i class="fa fa-fw fa-github"></i></a>
                            </el-tooltip>

                            <!--/img/qq.874454d2.png-->
                            <el-tooltip class="item" effect="dark" content="QQ" placement="top">
                                <a href="/img/qq.874454d2.png" target="_blank"><i class="fa fa-fw fa-qq"></i></a>
                            </el-tooltip>

                            <!--/img/wx.307cf18d.png-->
                            <el-tooltip class="item" effect="dark" content="微信" placement="top">
                                <a href="/img/wx.307cf18d.png" target="_blank"><i class="fa fa-fw fa-wechat"></i></a>
                            </el-tooltip>

                        </div>
                        <!--第二排按钮-->
                        <div>
                            <el-tooltip class="item" effect="dark" content="微博" placement="top">
                                <a href="https://www.weibo.com/login.php/" target="_blank"><i class="fa fa-fw fa-weibo"></i></a>
                            </el-tooltip>

                            <el-tooltip class="item" effect="dark" content="CSDN" placement="top">
                                <a href="https://www.csdn.net/" target="_blank"><i class="">C</i></a>
                            </el-tooltip>

                            <el-tooltip class="item" effect="dark" content="简历" placement="top">
                                <a href="#" target="_blank"><i
                                        class="fa fa-fw fa-file-word-o"></i></a>
                            </el-tooltip>

                            <el-tooltip class="item" effect="dark" content="更多" placement="top">
                                <a href="javascript:void(0)" @click="toAbout"><i class="el-icon-more"></i></a>
                            </el-tooltip>
                        </div>
                    </div>
                </div>
            </section>


            <!--第二个盒子： Do you like Bolg? 点赞  -->
            <section id="like" @click="cliLike">
                <p>Do you like the Bolg?</p>
                <div class="like-box">
                    <i :class="isShow?'heart':'heart active'"></i>
                    <span>{{LikeNumber}}</span>
                </div>
            </section>


            <!-- 第三个 空行，分割-->
            <section>
<!--                <img src="~@/assets/img/right/qq.png" alt="">-->
<!--                <img src="~@/assets/img/right/wx.png" alt="">-->
            </section>


            <!--第四个盒子: 大家都在说-->
            <section>
                <div class="table">
                    <table-page message="最新评论"/>
                </div>

                <ul class="said-list">
                    <li v-for="item in said">
                        <a :href="'/detailPage?aid='+item.belongArticle"> <!--取出评论的文章-->
                            <div class="said-list-avatar">
                                <img src="~@/assets/img/common/Tourists-Logo.jpg" alt="">
                            </div>
                            <div class="said-list-text">
                                <p class="p1">游客 在「...文章」中说:</p>
                                <p class="p2">{{item.content}}</p>
                            </div>
                        </a>
                    </li>

                </ul>

            </section>


            <!--第五个盒子：大家都在看-->
            <section>
                <div class="table">
                    <table-page message="大家都在看"/>
                </div>
                <ul class="show-list">
                    <li v-for="item in RecommendArticle">
                        <a :href="'/detailPage?aid='+item.id" target="_blank">{{item.title}}</a>
                        <span>——{{item.readNumber}} 次围观</span>
                    </li>
                </ul>
            </section>
        </div>
    </div>
</template>

<!-- ---------------------------脚本区--------------------------- -->
<script>
    import TablePage from "../common/TablePage";
    /**
     * 网络请求相关方法
     */
    import {increase, getRedHeartNumber, newComment, findRecommendArticle} from "../../network/Right"

    export default {
        name: "Right",
        props: {},
        data() {
            return {
                LikeNumber: 0, //统计赞个数
                isShow: true, // 默认可以点击
                OnlookersNumber: 200,// 围观数量
                said: [],//大家都在说
                // currentIndex: 0,
                RecommendArticle: [],//推荐文章
            }
        },
        created() {
            // 向服务器请求数据：点赞红心数据
            getRedHeartNumber().then(res => {
                this.LikeNumber = res.data;
            })

            // 最新评论 数据
            newComment().then(res => {
                this.said = res.data;
            })

            //获取观看最多的文章
            findRecommendArticle().then(res => {
                this.RecommendArticle = res.data
                console.log(res.data)
            })


        },
        components: {
            TablePage
        },
        methods: {
            /*点赞*/
            cliLike() {
                // 点击后成功后，立即就不让点击
                if (this.isShow == true) {
                    this.isShow = false;

                    this.LikeNumber += 1;

                    let timer = setTimeout(() => {
                        //1.5秒后才让点击
                        this.isShow = true;
                        clearTimeout(timer);
                    }, 1500);

                    /**
                     * 网络请求点赞
                     */
                    increase();
                }
            },

            /*去关于页面*/
            toAbout() {
                //如果是当前页面，就不用跳了
                if (this.$route.path.indexOf("/about") != -1) {
                    return "";
                } else {
                    this.$router.push("/about")
                }

            },

        }
    }
</script>

<!-- ---------------------------样式区--------------------------- -->
<style scoped>
    /****************************所有盒子通用样式 ***********************/
    section {
        padding: 15px;
        margin-bottom: 20px;
        background-color: #fff;
        border-radius: 5px;

        /*过度效果，不生硬*/
        transition: 0.5s;
    }

    section:hover {
        transform: translate(0px, -5px);
        box-shadow: 1px 15px 30px rgba(0, 0, 0, .1);
    }

    /***********************************你能找到我么？***********************************/
    /*第一个盒子 的图片*/
    section img {
        width: 100%;
        height: 100%;
    }

    /*第一个盒子的 文字*/
    .right-body-1 > p {
        color: black;
        font-size: 14px;
        text-align: center;
        line-height: 45px;
        font-weight: bold;
    }

    /*第一个盒子 的按钮*/
    .buttons {
        text-align: center;
    }

    .buttons a {
        display: inline-block;
        margin-bottom: 8px;
        position: relative;
        text-decoration: none;
    }

    .buttons i {
        display: inline-block;
        font-size: 18px;
        width: 42px;
        height: 42px;
        line-height: 42px;

        margin: 0 6px;
        color: rgba(0, 0, 0, 0.5);

        background: rgba(0, 0, 0, 0.1);
        border-radius: 50%;
    }

    .buttons i:hover {
        color: #fff;
        background: #F4692C;
    }

    /***********************************Do you like the Bolg***********************************/

    /*进入模块 显示小手*/
    #like:hover {
        cursor: pointer;
    }

    /*Do you like the Bolg 文字的样式*/
    #like > p {
        text-align: center;
        font-size: 16px;
        font-weight: bold;
        color: #DF2050;
        margin: 10px 0px;
    }


    /* 红心和点赞数量相关的盒子*/
    .like-box {
        text-align: center;
        height: 60px;
        /*margin:0px auto;*/

        /*position: relative;*/
        /*top: 0;*/
    }

    /*点赞数量文字*/
    .like-box span {
        display: inline-block;
        font-size: 28px;
        color: #DF2050;
        padding-right: 45px;

        position: relative;
        top: -15px;

    }

    /*图片*/
    .like-box i.heart {
        /*margin-top: 20px;*/
        display: inline-block;
        vertical-align: middle;

        line-height: 50px;
        width: 100px;
        height: 100px;

        position: relative;
        top: -20px;
        left: 20px;


        /*动画效果相关*/
        background: url(../../assets/img/right/heart.png) no-repeat;
        transition: background-position 1s steps(28);
        transition-duration: 0s;
    }

    /*动画效果相关*/
    i.heart.active {
        transition-duration: 1s;
        background-position: -2800px 0;
    }

    /*鼠标进入图片，图片放大*/
    .like-box i.heart:hover {
        transform: scale(1.1);
    }

    /***********************************大家都在说***********************************/
    /*标签,相关布局*/
    .table {
        position: relative;
        top: -4px;
        left: -30px;
    }

    /*评论信息*/
    .said-list {
        list-style: none;
    }

    .said-list li {
        font-size: 14px;
        line-height: 20px;
    }

    .said-list li a {
        color: black;
        font-size: 14px;


        display: block;
        margin-bottom: 10px;
        padding: 5px;
        border-bottom: 1px solid #ddd;
        cursor: pointer;
    }

    .said-list li a p {
        display: block;

    }

    .said-list li a:hover {
        background: rgba(230, 244, 250, .8);
        border-radius: 5px;
    }

    .said-list-avatar {
        width: 38px;
        height: 38px;
        border-radius: 50%;

        float: left;
    }

    .said-list-avatar img {
        width: 100%;
        border-radius: 50%;

        transition: 0.4s;
    }

    .said-list-avatar img:hover {
        transform: rotate(360deg);
    }

    .said-list-text {
        margin-left: 40px;
        padding: 0px 5px;
        height: auto;

        /*超出部分换行*/
        word-break: break-all;
    }

    /* 评论第一行子图加粗*/
    .p1 {
        font-weight: 700;
    }

    .p2 {
        font-size: 12px;
    }

    /***********************************大家都在看***********************************/
    .show-list {
        list-style: none;
        text-align: justify;
    }

    .show-list li {
        padding: 8px 0px;
    }

    .show-list li a {
        color: black;
        font-weight: 700;
        line-height: 20px;

        font-size: 15px;
    }

    .show-list li span {
        font-size: 12px;
    }
</style>