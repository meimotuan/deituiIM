<template>
	<div class="main-body" id="app">
		 
		<div class="sglist">
			 
			<div v-for="(item,index) in pageData.list" :key="index"  class="sglist-item">
				
				<div @click="goBlog(item.id)" class="sglist-title flex" v-html="item.content"></div>
				<div @click="goBlog(item.id)" class="sglist-imglist">
					 
					<img v-for="(img,imgIndex) in item.imgslist" :key="imgIndex" :src="img+'.100x100.jpg'" class="sglist-imglist-img" />
					
				</div>
				<div class="sglist-ft">
					<div class="sglist-ft-love">{{item.love_num}}</div>
					<div class="sglist-ft-cm">{{item.comment_num}}</div>
					<div class="sglist-ft-view">{{item.view_num}}</div>
					<div @click="del(item.id)" class="flex-1 cl-danger iconfont pointer icon-delete"></div>
				</div>
				
			</div>
			
		</div>
		
	</div>
</template>

<script>
	export default({

		data:function(){
			return {
				pageData:{},
				pageLoad:false
			}
		},
		created:function(){
			this.getPage();
		},
		methods:{
	 
			goBlog:function(id){
				uni.navigateTo({
					url:"../sblog_blog/show?id="+id
				})
			},
			del:function(id){
				var that=this;
				uni.showModal({
					title: '提示',
					content: '删除后不可恢复，确认删除吗？',
					success: function (res) {
						if (res.confirm) {
							that.app.get({
								url:that.app.apiHost+"/module.php?m=sblog_blog&a=delete&ajax=1&id="+id,
							
								dataType:"json",
								success:function(res){
									var list=that.pageData.list;
									var newlist=[];
									for(var i=0;i<list.length;i++){
										if(list[i].id!=id){
											newlist.push(list[i]);
										}
									}
									that.pageData.list=newlist;
									 
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
				 
				
			},
			getPage:function(){
				var that=this;
				that.app.get({
					url:that.app.apiHost+"/module.php?m=sblog_blog&a=my&ajax=1",
					
					dataType:"json",
					success:function(res){
						that.pageLoad=true;
						that.pageData=res.data;
					}
				})
			}
		}
	})
</script>

<style>
</style>
