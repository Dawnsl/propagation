<template>

  <el-row :gutter="10">
  	<el-col :span="4">
      <side_nav></side_nav>
    </el-col>
    <el-col :span="20">
    	<el-card>
    		<div v-if="loading">
         		 加载中...
        	</div>
        	 <div v-else>
          <h3 slot="header"><i class="el-icon-setting"></i>内容详情</h3>

          <h3 style="margin: 15px auto">{{pageData&&pageData.title}}</h3>
          <small>创建于：{{pageData&&new Date(pageData.create_at).toLocaleString()}}</small>

          <hr>

          <div class="markdown-body" v-html="pageContent"></div>
        </div>
    	</el-card>	
    </el-col>	
  
  
  </el-row>

</template>

<script>
	
	import {mapState,mapActions} from 'vuex'
	import side_nav from './side_nav'
	export default {
		name:"page",
		data(){
          return {
              loading:false
          }
        },
        components:{
        	side_nav
      	},
      	computed:{
         ...mapState([
           'pageData',
           'pageContent',
           'pageReplies'
         ])
        },
        methods:{
         ...mapActions([
            'getData'
         ])
        },
        beforeMount(){
          this.loading = true;
          this.getData(this.$route.params.id)
            .then(()=>{
              this.loading = false;
              console.log(this.pageReplies);
            });
      	}
		
	}
	
</script>

<style>
  .markdown-body {
    box-sizing: border-box;
    min-width: 200px;
    max-width: 980px;
    margin: 0 auto;
    padding: 45px 0;
  }

  @media (max-width: 767px) {
    .markdown-body {
      padding: 15px;
    }
  }
</style>