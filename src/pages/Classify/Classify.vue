<!--
 * @,@Author: ,: your name
 * @,@Date: ,: 2020-11-02 15:47:54
 * @,@LastEditTime: ,: 2020-11-02 16:17:16
 * @,@LastEditors: ,: Please set LastEditors
 * @,@Description: ,: In User Settings Edit
 * @,@FilePath: ,: \goods\vue_dewu\src\pages\Classify\Classify.vue
 -->
<template>
    <div>
       <classify-header></classify-header>
       <classify-search :Brands="Brands"></classify-search>
       <classify-list :Brands="Brands" :hotBrands="hotBrands" :letter="letter"></classify-list>
       <classify-alphabet :Brands="Brands"
       @change="handleLetterClick"></classify-alphabet>
    </div>
</template>
<script>
import axios from 'axios'
import ClassifyHeader from '@/pages/Classify/components/Header.vue'
import ClassifySearch from '@/pages/Classify/components/Search.vue'
import ClassifyList from '@/pages/Classify/components/List.vue'
import ClassifyAlphabet from '@/pages/Classify/components/Alphabet.vue'
export default {
    name:'Classify',
    // 注册局部组件
    components: {
        ClassifyHeader,
        ClassifySearch,
        ClassifyList,
        ClassifyAlphabet
    },
     data(){
    return{
      Brands:{},
      hotBrands:[],
      letter:''
    }
  },
    methods:{
        getClssifyInfo(){
            axios.get('/static/mock/Classify.json')
            .then(this.handleGetClassifyInfoSucc)
        },
        handleGetClassifyInfoSucc(res){
            // console.log(res)
            res=res.data
            if(res.ret&&res.data){
               const data =res.data
               this.Brands=data.Brands
               console.log("得到的数据是",this.Brands)
               this.hotBrands=data.hotBrands
                console.log("得到的数据是",this.hotBrands)
            }
        },
        handleLetterClick(letter){
            this.letter=letter
        //  console.log(letter);
        }
    },
    mounted(){
        this.getClssifyInfo()
    }
}
</script>
<style lang="stylus" scoped>

</style>