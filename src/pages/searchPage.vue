<template>
    <div class="search-view has-header">
        <div class="search">
            <form id="search_form" onsubmit="return false">
                <input type="text" name="query" v-model.trim.lazy="queryStr" placeholder="搜索 书 / 影 / 音 / 标签">
                <a  @click="fileSearch">搜索</a>
            </form>
        </div>
        <div v-if="queryStr" class="search-res">
            <search-list title="影视" :items="queryMovieData">
                <a class="list-link" :href="movieUrl" slot="more">查看更多影视结果</a>
            </search-list>
            <search-list title="图书" :items="queryBookData">
                <a class="list-link" :href="bookUrl" slot="more">查看更多图书结果</a>
            </search-list>
            <search-list title="音乐" :items="queryMusicData">
                <a class="list-link" :href="musicUrl" slot="more">查看更多音乐结果</a>
            </search-list>
        </div>
    </div>
</template>

<script>
    import {mapState} from 'vuex'
    import searchList from "@/components/searchList"

    export default {
        name: 'searchPage',
        components: {
            searchList
        },
        data() {
            return {
                queryStr: '',
                movieUrl:'',
                bookUrl:'',
                musicUrl:''
            }
        },
        watch:{
            queryStr(val, oldVal){
                this.movieUrl = "https://m.douban.com/search?type=movie&query="+val;
                this.bookUrl = "https://m.douban.com/search?type=book&query="+val;
                this.musicUrl = "https://m.douban.com/search?type=music&query="+val;
            }
        },
        computed: {
            ...mapState({
                queryMovieData: state => state.search.queryMovieData,
                queryBookData: state => state.search.queryBookData,
                queryMusicData: state => state.search.queryMusicData
            })
        },
        methods:{
            fileSearch(){
                if (this.queryStr) {
                    this.$store.dispatch({
                        type: 'query',
                        queryStr: this.queryStr
                    })
                }
            }
        },
        created: function () {
            const thisQueryStr = this.$route.query.queryStr;
            this.queryStr = thisQueryStr;
            this.fileSearch();
        }
    }
</script>

<style lang="scss" scoped>
    .search-view {
        .search {
            padding: 0.7rem 2rem;
            border-bottom: 0.1rem solid #F2F2F2;
            overflow: hidden;

            input {
                width: 85%;
                height: 3rem;
                padding: 0.5rem 0.8rem;
                box-sizing: border-box;
                font-size: 1.4rem;
                color: #111;
                background: #f5f5f5;
                border-radius: 0.3rem;
                border: 0;
                outline: 0;
            }

            a {
                display: inline-block;
                float: right;
                width: 10%;
                height: 3rem;
                padding-left: 5%;
                line-height: 3rem;
                font-size: 1.4rem;
                color: #333;
                text-decoration: none;
            }
        }

        .list-link {
            display: block;
            margin-bottom: 1.5rem;
            padding: 1.5rem 0 1.5rem 5rem;
            font-size: 1.6rem;
            line-height: 1.8rem;
            color: #42bd56;
        }
    }
</style>
