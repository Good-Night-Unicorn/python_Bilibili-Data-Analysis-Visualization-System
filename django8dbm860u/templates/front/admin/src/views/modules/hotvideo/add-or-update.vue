<template>
	<div class="addEdit-block">
		<el-form
			class="add-update-preview"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
			label-width="180px"
		>
			<template >
				<el-form-item class="input" v-if="type!='info'"  label="标题" prop="title" >
					<el-input v-model="ruleForm.title" placeholder="标题" clearable  :readonly="ro.title"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="标题" prop="title" >
					<el-input v-model="ruleForm.title" placeholder="标题" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="时长（秒）" prop="duration" >
					<el-input v-model.number="ruleForm.duration" placeholder="时长（秒）" clearable  :readonly="ro.duration"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="时长（秒）" prop="duration" >
					<el-input v-model="ruleForm.duration" placeholder="时长（秒）" readonly></el-input>
				</el-form-item>
				<el-form-item class="upload" v-if="type!='info' && !ro.cover" label="封面" prop="cover" >
					<file-upload
						tip="点击上传封面"
						action="file/upload"
						:limit="3"
						:multiple="true"
						:fileUrls="ruleForm.cover?ruleForm.cover:''"
						@change="coverUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item class="upload" v-else-if="ruleForm.cover" label="封面" prop="cover" >
					<img v-if="ruleForm.cover.substring(0,4)=='http'&&ruleForm.cover.split(',w').length>1" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.cover" width="100" height="100">
					<img v-else-if="ruleForm.cover.substring(0,4)=='http'" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.cover.split(',')[0]" width="100" height="100">
					<img v-else class="upload-img" style="margin-right:20px;" v-bind:key="index" v-for="(item,index) in ruleForm.cover.split(',')" :src="$base.url+item" width="100" height="100">
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="up主" prop="author" >
					<el-input v-model="ruleForm.author" placeholder="up主" clearable  :readonly="ro.author"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="up主" prop="author" >
					<el-input v-model="ruleForm.author" placeholder="up主" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="播放量" prop="playcount" >
					<el-input v-model.number="ruleForm.playcount" placeholder="播放量" clearable  :readonly="ro.playcount"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="播放量" prop="playcount" >
					<el-input v-model="ruleForm.playcount" placeholder="播放量" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="弹幕量" prop="danmaku" >
					<el-input v-model.number="ruleForm.danmaku" placeholder="弹幕量" clearable  :readonly="ro.danmaku"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="弹幕量" prop="danmaku" >
					<el-input v-model="ruleForm.danmaku" placeholder="弹幕量" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="点赞" prop="likes" >
					<el-input v-model.number="ruleForm.likes" placeholder="点赞" clearable  :readonly="ro.likes"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="点赞" prop="likes" >
					<el-input v-model="ruleForm.likes" placeholder="点赞" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="收藏" prop="favorite" >
					<el-input v-model.number="ruleForm.favorite" placeholder="收藏" clearable  :readonly="ro.favorite"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="收藏" prop="favorite" >
					<el-input v-model="ruleForm.favorite" placeholder="收藏" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="分享" prop="share" >
					<el-input v-model.number="ruleForm.share" placeholder="分享" clearable  :readonly="ro.share"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="分享" prop="share" >
					<el-input v-model="ruleForm.share" placeholder="分享" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="评论数" prop="commentcount" >
					<el-input v-model.number="ruleForm.commentcount" placeholder="评论数" clearable  :readonly="ro.commentcount"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="评论数" prop="commentcount" >
					<el-input v-model="ruleForm.commentcount" placeholder="评论数" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="上传地点" prop="publocation" >
					<el-input v-model="ruleForm.publocation" placeholder="上传地点" clearable  :readonly="ro.publocation"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="上传地点" prop="publocation" >
					<el-input v-model="ruleForm.publocation" placeholder="上传地点" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="上传时间" prop="pubtime" >
					<el-input v-model="ruleForm.pubtime" placeholder="上传时间" clearable  :readonly="ro.pubtime"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="上传时间" prop="pubtime" >
					<el-input v-model="ruleForm.pubtime" placeholder="上传时间" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="类型" prop="typename" >
					<el-input v-model="ruleForm.typename" placeholder="类型" clearable  :readonly="ro.typename"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="类型" prop="typename" >
					<el-input v-model="ruleForm.typename" placeholder="类型" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="来源" prop="laiyuan" >
					<el-input v-model="ruleForm.laiyuan" placeholder="来源" clearable  :readonly="ro.laiyuan"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="来源" prop="laiyuan" >
					<el-input v-model="ruleForm.laiyuan" placeholder="来源" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="投币" prop="coin" >
					<el-input v-model.number="ruleForm.coin" placeholder="投币" clearable  :readonly="ro.coin"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="投币" prop="coin" >
					<el-input v-model="ruleForm.coin" placeholder="投币" readonly></el-input>
				</el-form-item>
			</template>
			<el-form-item class="textarea" v-if="type!='info'" label="简介" prop="info" >
				<el-input
					style="min-width: 200px; max-width: 600px;"
					type="textarea"
					:rows="8"
					placeholder="简介"
					v-model="ruleForm.info" >
				</el-input>
			</el-form-item>
			<el-form-item v-else-if="ruleForm.info" label="简介" prop="info" >
				<span class="text">{{ruleForm.info}}</span>
			</el-form-item>
			<el-form-item class="btn">
				<el-button class="btn3"  v-if="type!='info'" type="success" @click="onSubmit">
					<span class="icon iconfont icon-xihuan"></span>
					提交
				</el-button>
				<el-button class="btn4" v-if="type!='info'" type="success" @click="back()">
					<span class="icon iconfont icon-xihuan"></span>
					取消
				</el-button>
				<el-button class="btn5" v-if="type=='info'" type="success" @click="back()">
					<span class="icon iconfont icon-xihuan"></span>
					返回
				</el-button>
			</el-form-item>
		</el-form>
    

	</div>
</template>
<script>
	import { 
		isIntNumer,
	} from "@/utils/validate";
	export default {
		data() {
			var validateIntNumber = (rule, value, callback) => {
				if(!value){
					callback();
				} else if (!isIntNumer(value)) {
					callback(new Error("请输入整数"));
				} else {
					callback();
				}
			};
			return {
				id: '',
				type: '',
			
			
				ro:{
					title : false,
					info : false,
					duration : false,
					cover : false,
					author : false,
					playcount : false,
					danmaku : false,
					likes : false,
					favorite : false,
					share : false,
					commentcount : false,
					publocation : false,
					pubtime : false,
					typename : false,
					laiyuan : false,
					coin : false,
					clicktime : false,
					discussnum : false,
					storeupnum : false,
				},
			
				ruleForm: {
					title: '',
					info: '',
					duration: '',
					cover: '',
					author: '',
					playcount: '',
					danmaku: '',
					likes: '',
					favorite: '',
					share: '',
					commentcount: '',
					publocation: '',
					pubtime: '',
					typename: '',
					laiyuan: '',
					coin: '',
					clicktime: '',
				},

				rules: {
					title: [
					],
					info: [
					],
					duration: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					cover: [
					],
					author: [
					],
					playcount: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					danmaku: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					likes: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					favorite: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					share: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					commentcount: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					publocation: [
					],
					pubtime: [
					],
					typename: [
					],
					laiyuan: [
					],
					coin: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					clicktime: [
					],
					discussnum: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					storeupnum: [
						{ validator: validateIntNumber, trigger: 'blur' },
					],
				},
			};
		},
		props: ["parent"],
		computed: {



		},
		components: {
		},
		created() {
		},
		methods: {
			// 下载
			download(file){
				window.open(`${file}`)
			},
			// 初始化
			init(id,type) {
				if (id) {
					this.id = id;
					this.type = type;
				}
				if(this.type=='info'||this.type=='else'||this.type=='msg'){
					this.info(id);
				}else if(this.type=='logistics'){
					for(let x in this.ro) {
						this.ro[x] = true
					}
					this.logistics=false;
					this.info(id);
				}else if(this.type=='cross'){
					var obj = this.$storage.getObj('crossObj');
					for (var o in obj){
						if(o=='title'){
							this.ruleForm.title = obj[o];
							this.ro.title = true;
							continue;
						}
						if(o=='info'){
							this.ruleForm.info = obj[o];
							this.ro.info = true;
							continue;
						}
						if(o=='duration'){
							this.ruleForm.duration = obj[o];
							this.ro.duration = true;
							continue;
						}
						if(o=='cover'){
							this.ruleForm.cover = obj[o];
							this.ro.cover = true;
							continue;
						}
						if(o=='author'){
							this.ruleForm.author = obj[o];
							this.ro.author = true;
							continue;
						}
						if(o=='playcount'){
							this.ruleForm.playcount = obj[o];
							this.ro.playcount = true;
							continue;
						}
						if(o=='danmaku'){
							this.ruleForm.danmaku = obj[o];
							this.ro.danmaku = true;
							continue;
						}
						if(o=='likes'){
							this.ruleForm.likes = obj[o];
							this.ro.likes = true;
							continue;
						}
						if(o=='favorite'){
							this.ruleForm.favorite = obj[o];
							this.ro.favorite = true;
							continue;
						}
						if(o=='share'){
							this.ruleForm.share = obj[o];
							this.ro.share = true;
							continue;
						}
						if(o=='commentcount'){
							this.ruleForm.commentcount = obj[o];
							this.ro.commentcount = true;
							continue;
						}
						if(o=='publocation'){
							this.ruleForm.publocation = obj[o];
							this.ro.publocation = true;
							continue;
						}
						if(o=='pubtime'){
							this.ruleForm.pubtime = obj[o];
							this.ro.pubtime = true;
							continue;
						}
						if(o=='typename'){
							this.ruleForm.typename = obj[o];
							this.ro.typename = true;
							continue;
						}
						if(o=='laiyuan'){
							this.ruleForm.laiyuan = obj[o];
							this.ro.laiyuan = true;
							continue;
						}
						if(o=='coin'){
							this.ruleForm.coin = obj[o];
							this.ro.coin = true;
							continue;
						}
						if(o=='clicktime'){
							this.ruleForm.clicktime = obj[o];
							this.ro.clicktime = true;
							continue;
						}
						if(o=='discussnum'){
							this.ruleForm.discussnum = obj[o];
							this.ro.discussnum = true;
							continue;
						}
						if(o=='storeupnum'){
							this.ruleForm.storeupnum = obj[o];
							this.ro.storeupnum = true;
							continue;
						}
					}
				}
			
			},
			// 多级联动参数

			info(id) {
				this.$http({
					url: `hotvideo/info/${id}`,
					method: "get"
				}).then(({ data }) => {
					if (data && data.code === 0) {
						this.ruleForm = data.data;
						//解决前台上传图片后台不显示的问题
						let reg=new RegExp('../../../upload','g')//g代表全部
					} else {
						this.$message.error(data.msg);
					}
				});
			},

			// 提交
			async onSubmit() {
					if(this.ruleForm.cover!=null) {
						this.ruleForm.cover = this.ruleForm.cover.replace(new RegExp(this.$base.url,"g"),"");
					}
					var objcross = this.$storage.getObj('crossObj');
					if(!this.ruleForm.id) {
						delete this.ruleForm.userid
					}
					await this.$refs["ruleForm"].validate(async valid => {
						if (valid) {
							if(this.type=='cross'){
								var statusColumnName = this.$storage.get('statusColumnName');
								var statusColumnValue = this.$storage.get('statusColumnValue');
								if(statusColumnName!='') {
									var obj = this.$storage.getObj('crossObj');
									if(statusColumnName && !statusColumnName.startsWith("[")) {
										for (var o in obj){
											if(o==statusColumnName){
												obj[o] = statusColumnValue;
											}
										}
										var table = this.$storage.get('crossTable');
										await this.$http({
											url: `${table}/update`,
											method: "post",
											data: obj
										}).then(({ data }) => {});
									}
								}
							}
							
							await this.$http({
								url: `hotvideo/${!this.ruleForm.id ? "save" : "update"}`,
								method: "post",
								data: this.ruleForm
							}).then(async ({ data }) => {
								if (data && data.code === 0) {
									this.$message({
										message: "操作成功",
										type: "success",
										duration: 1500,
										onClose: () => {
											this.parent.showFlag = true;
											this.parent.addOrUpdateFlag = false;
											this.parent.hotvideoCrossAddOrUpdateFlag = false;
											this.parent.search();
											this.parent.contentStyleChange();
										}
									});
								} else {
									this.$message.error(data.msg);
								}
							});
						}
					});
			},
			// 获取uuid
			getUUID () {
				return new Date().getTime();
			},
			// 返回
			back() {
				this.parent.showFlag = true;
				this.parent.addOrUpdateFlag = false;
				this.parent.hotvideoCrossAddOrUpdateFlag = false;
				this.parent.contentStyleChange();
			},
			coverUploadChange(fileUrls) {
				this.ruleForm.cover = fileUrls;
			},
		}
	};
</script>
<style lang="scss" scoped>
	.addEdit-block {
		padding: 30px;
	}
	.add-update-preview {
		padding: 20px 35% 0 10%;
		border-color: #eee;
		border-width: 0px 0 0;
		border-style: solid;
	}
	.amap-wrapper {
		width: 100%;
		height: 500px;
	}
	
	.search-box {
		position: absolute;
	}
	
	.el-date-editor.el-input {
		width: auto;
	}
	.add-update-preview /deep/ .el-form-item {
		border: 0px solid #eee;
		padding: 0;
		margin: 0 0 26px 0;
		display: inline-block;
		width: 100%;
	}
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
		padding: 0 10px 0 0;
		color: #666;
		font-weight: 600;
		width: 180px;
		font-size: 16px;
		line-height: 40px;
		text-align: right;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
		margin-left: 180px;
	}
	.add-update-preview .el-form-item span.text {
		padding: 0 10px;
		color: #333;
		background: none;
		font-weight: 500;
		display: inline-block;
		font-size: 16px;
		line-height: 40px;
		min-width: 50%;
	}
	
	.add-update-preview .el-input {
		width: 100%;
	}
	.add-update-preview .el-input /deep/ .el-input__inner {
		border: 0px solid #ccc;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		width: 100%;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-input /deep/ .el-input__inner[readonly="readonly"] {
		border: 0px solid #ccc;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		background: none;
		width: auto;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-input-number {
		text-align: left;
		width: 100%;
	}
	.add-update-preview .el-input-number /deep/ .el-input__inner {
		text-align: left;
		border: 0px solid #ccc;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		width: 100%;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-input-number /deep/ .is-disabled .el-input__inner {
		text-align: left;
		border: 0px solid #ccc;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		background: none;
		width: auto;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-input-number /deep/ .el-input-number__decrease {
		display: none;
	}
	.add-update-preview .el-input-number /deep/ .el-input-number__increase {
		display: none;
	}
	.add-update-preview .el-select {
		width: 100%;
	}
	.add-update-preview .el-select /deep/ .el-input__inner {
		border: 0px solid #ccc;
		border-radius: 0px;
		padding: 0 10px;
		color: #666;
		width: 100%;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-select /deep/ .is-disabled .el-input__inner {
		border: 0;
		cursor: not-allowed;
		border-radius: 4px;
		padding: 0 10px;
		color: #666;
		background: none;
		width: auto;
		font-size: 16px;
		height: 34px;
	}
	.add-update-preview .el-date-editor {
		width: 100%;
	}
	.add-update-preview .el-date-editor /deep/ .el-input__inner {
		border: 0px solid #ccc;
		border-radius: 0px;
		padding: 0 10px 0 30px;
		color: #666;
		background: #fff;
		width: 100%;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .el-date-editor /deep/ .el-input__inner[readonly="readonly"] {
		border: 0;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 10px 0 30px;
		color: #666;
		background: none;
		width: auto;
		font-size: 16px;
		height: 40px;
	}
	.add-update-preview .viewBtn {
		border: 0px solid #ccc;
		cursor: pointer;
		border-radius: 0px;
		padding: 0 15px;
		margin: 0 20px 0 0;
		color: #666;
		background: #fff;
		width: auto;
		font-size: 15px;
		line-height: 34px;
		height: 34px;
		.iconfont {
			margin: 0 2px;
			color: #666;
			font-size: 16px;
			height: 34px;
		}
	}
	.add-update-preview .viewBtn:hover {
		opacity: 0.8;
	}
	.add-update-preview .downBtn {
		border: 0px solid #ccc;
		cursor: pointer;
		border-radius: 0px;
		padding: 0 15px;
		margin: 0 20px 0 0;
		color: #666;
		background: #fff;
		width: auto;
		font-size: 15px;
		line-height: 34px;
		height: 34px;
		.iconfont {
			margin: 0 2px;
			color: #666;
			font-size: 16px;
			height: 34px;
		}
	}
	.add-update-preview .downBtn:hover {
		opacity: 0.8;
	}
	.add-update-preview .unBtn {
		border: 0;
		cursor: not-allowed;
		border-radius: 4px;
		padding: 0 0px;
		margin: 0 20px 0 0;
		outline: none;
		color: #999;
		background: none;
		width: auto;
		font-size: 16px;
		line-height: 40px;
		height: 40px;
		.iconfont {
			margin: 0 2px;
			color: #fff;
			display: none;
			font-size: 14px;
			height: 34px;
		}
	}
	.add-update-preview .unBtn:hover {
		opacity: 0.8;
	}
	.add-update-preview /deep/ .el-upload--picture-card {
		background: transparent;
		border: 0;
		border-radius: 0;
		width: auto;
		height: auto;
		line-height: initial;
		vertical-align: middle;
	}
	
	.add-update-preview /deep/ .upload .upload-img {
		border: 0px solid #ccc;
		cursor: pointer;
		border-radius: 0px;
		color: #666;
		background: #fff;
		width: 90px;
		font-size: 24px;
		line-height: 60px;
		text-align: center;
		height: 60px;
	}
	
	.add-update-preview /deep/ .el-upload-list .el-upload-list__item {
		border: 0px solid #ccc;
		cursor: pointer;
		border-radius: 0px;
		color: #666;
		background: #fff;
		width: 90px;
		font-size: 24px;
		line-height: 60px;
		text-align: center;
		height: 60px;
	}
	
	.add-update-preview /deep/ .el-upload .el-icon-plus {
		border: 0px solid #ccc;
		cursor: pointer;
		border-radius: 0px;
		color: #666;
		background: #fff;
		width: 90px;
		font-size: 24px;
		line-height: 60px;
		text-align: center;
		height: 60px;
	}
	.add-update-preview /deep/ .el-upload__tip {
		color: #666;
		font-size: 15px;
	}
	
	.add-update-preview .el-textarea /deep/ .el-textarea__inner {
		border: 0px solid #ccc;
		border-radius: 0px;
		padding: 12px;
		color: #666;
		background: #fff;
		width: auto;
		font-size: 14px;
		min-width: 400px;
		height: 120px;
	}
	.add-update-preview .el-textarea /deep/ .el-textarea__inner[readonly="readonly"] {
				border: 0;
				cursor: not-allowed;
				border-radius: 0px;
				padding: 12px;
				color: #666;
				background: none;
				width: auto;
				font-size: 14px;
				min-width: 400px;
				height: auto;
			}
	.add-update-preview .el-form-item.btn {
		padding: 0;
		margin: 20px 0 0;
		.btn1 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #924EFF;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn1:hover {
			opacity: 0.8;
		}
		.btn2 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #5498DE;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 34px;
			}
		}
		.btn2:hover {
			opacity: 0.8;
		}
		.btn3 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #924EFF;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn3:hover {
			opacity: 0.8;
		}
		.btn4 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #5498DE;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn4:hover {
			opacity: 0.8;
		}
		.btn5 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #5498DE;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn5:hover {
			opacity: 0.8;
		}
	}
</style>
