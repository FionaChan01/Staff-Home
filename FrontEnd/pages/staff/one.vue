<template>
	<view>
		<u-cell-group>
			<u-cell-item class="" :value='user.username' :valueStyle='valueStyle' :title-style="titleStyle"
				:arrow='false'>
				<u-image width='120rpx' height='120rpx' slot="title" :src="user.faceurl" shape="circle"></u-image>
			</u-cell-item>
			<u-gap height="15" bg-color="#f9f9f9"></u-gap>

			<!-- 动态实现用户信息展现，即获取的用户信息属性中没有的将不进行展示 -->
			<!-- 动态实现方法一 -->
			<!-- 通过key和中文描述绑定json格式，循环访问 -->
			<u-cell-group>
				<u-cell-item v-for="(value,key) in labelDict" :key='key' :title='value' :value='user[key]'
					:valueStyle='valueStyle' :title-style="titleStyle" :arrow='false' v-show="user[key]?true:false">
				</u-cell-item>
			</u-cell-group>
			
			<!-- 动态实现方法二 -->
			<!-- 先定一个数组，然后把这两条数据关联起来，组成对象，然后根据空值，加个是否显示的条件。然后便利数组 -->

		</u-cell-group>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// cell-item样式
				valueStyle: {
					'text-align': 'left'
				},
				titleStyle: {
					'width': '160rpx'
				},
				
				// key和label的对应
				labelDict: {
					// username: '姓名',
					sex: '性别',
					birthday: '出生日期',
					cardid: '身份证号码',
					deptname: '部门',
					jobname: '职位',
					education: '学历',
					email: '邮箱',
					tel: '手机',
					qqnum: 'QQ号码',
					party: '政治面貌',
					address: '联系地址',
					postcode: '邮政编码',
					remark:'备注',
					// loginname:'账户名', 
					// password:'密码', 
					// status:'权限', 
				},
				user: {}
			}
		},
		
		// 页面加载
		onLoad(item) {
			console.log('页面跳转数据',item)
			this.$api.staffOneByUseridSendData({
				userid:item.userid
			}).then(res => {
				this.user = res.data.data
				uni.setNavigationBarTitle({
					title: this.user.username
				})
				// 默认头像处理
				if (!this.user.faceurl) {
					if (this.user.sex == '男') {
						this.user.faceurl = '/static/boy1.svg'
					} else if (this.user.sex == '女') {
						this.user.faceurl = '/static/girl1.svg'
					} else {
						this.user.faceurl = '/static/头像.svg'
					}
				}
			})
		},
		
		methods: {}
	}
</script>

<style>

</style>
