<!-- ---------------------------模板区--------------------------- -->
<template>
    <div id="about">
        <el-row :gutter="30">
            <!--左侧视图-->
            <el-col :md="16">
                <!--关于我-->
                <info/>

                <!--评论-->
                <comment :commentsInfos="comments"/>
            </el-col>

            <!--右侧视图-->
            <el-col :md="8">
                <right/>
            </el-col>
        </el-row>
    </div>
</template>


<!-- ---------------------------脚本区--------------------------- -->
<script>
    import Right from "@/components/this-porject/Right";
    import Info from "./childComp/AboutInfo";
    import Comment from "@/components/this-porject/Comment";

    /**
     * 网络请求
     */
    import {findComment} from "../../network/Comment"

    export default {
        name: "About",
        components: {
            Right,
            Info,
            Comment
        },
        data() {
            return {
                comments: [], //评论信息
                httpError: false,
            }
        },
        created() {
            //
            let belong = this.$route.path.replace("/", '');

            //根据模块获取评论
            findComment(belong).then(res => {
                this.comments = res.data;

            }).catch(err => {
                console.log(err)
                this.httpError = true;
            });
        },
        mounted() {
            document.documentElement.scrollTop = 865;
        }

    }
</script>

<!-- ---------------------------样式区--------------------------- -->
<style scoped>
    #about {
        width: 80%;
        margin: 0px auto;
    }
</style>