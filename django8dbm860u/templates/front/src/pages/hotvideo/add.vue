<template>
	<div class="add-update-preview">
		<el-form
			class="add-update-form"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
			label-width="180px"
			>
			<el-form-item class="add-item" label="标题" prop="title">
				<el-input v-model="ruleForm.title" 
					placeholder="标题" clearable :disabled=" false  ||ro.title"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="时长（秒）" prop="duration">
				<el-input v-model.number="ruleForm.duration" 
					placeholder="时长（秒）" clearable :disabled=" false  ||ro.duration"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="封面" v-if="type!='cross' || (type=='cross' && !ro.cover)" prop="cover">
				<file-upload
					tip="点击上传封面"
					action="file/upload"
					:limit="3"
					:multiple="true"
					:fileUrls="ruleForm.cover?ruleForm.cover:''"
					@change="coverUploadChange"
					></file-upload>
			</el-form-item>
			<el-form-item class="add-item" v-else label="封面" prop="cover">
				<img v-if="ruleForm.cover.substring(0,4)=='http'" class="upload-img" v-bind:key="index" :src="ruleForm.cover.split(',')[0]">
				<img v-else class="upload-img" v-bind:key="index" v-for="(item,index) in ruleForm.cover.split(',')" :src="baseUrl+item">
			</el-form-item>
			<el-form-item class="add-item" label="up主" prop="author">
				<el-input v-model="ruleForm.author" 
					placeholder="up主" clearable :disabled=" false  ||ro.author"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="播放量" prop="playcount">
				<el-input v-model.number="ruleForm.playcount" 
					placeholder="播放量" clearable :disabled=" false  ||ro.playcount"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="弹幕量" prop="danmaku">
				<el-input v-model.number="ruleForm.danmaku" 
					placeholder="弹幕量" clearable :disabled=" false  ||ro.danmaku"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="点赞" prop="likes">
				<el-input v-model.number="ruleForm.likes" 
					placeholder="点赞" clearable :disabled=" false  ||ro.likes"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="收藏" prop="favorite">
				<el-input v-model.number="ruleForm.favorite" 
					placeholder="收藏" clearable :disabled=" false  ||ro.favorite"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="分享" prop="share">
				<el-input v-model.number="ruleForm.share" 
					placeholder="分享" clearable :disabled=" false  ||ro.share"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="评论数" prop="commentcount">
				<el-input v-model.number="ruleForm.commentcount" 
					placeholder="评论数" clearable :disabled=" false  ||ro.commentcount"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="上传地点" prop="publocation">
				<el-input v-model="ruleForm.publocation" 
					placeholder="上传地点" clearable :disabled=" false  ||ro.publocation"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="上传时间" prop="pubtime">
				<el-input v-model="ruleForm.pubtime" 
					placeholder="上传时间" clearable :disabled=" false  ||ro.pubtime"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="类型" prop="typename">
				<el-input v-model="ruleForm.typename" 
					placeholder="类型" clearable :disabled=" false  ||ro.typename"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="来源" prop="laiyuan">
				<el-input v-model="ruleForm.laiyuan" 
					placeholder="来源" clearable :disabled=" false  ||ro.laiyuan"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="投币" prop="coin">
				<el-input v-model.number="ruleForm.coin" 
					placeholder="投币" clearable :disabled=" false  ||ro.coin"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="简介" prop="info">
				<el-input
					type="textarea"
					:rows="8"
					placeholder="简介"
					v-model="ruleForm.info">
					</el-input>
			</el-form-item>

			<el-form-item class="add-btn-item">
				<el-button class="submitBtn"  type="primary" @click="onSubmit">
					<span class="icon iconfont "></span>
					<span class="text">提交</span>
				</el-button>
				<el-button class="closeBtn" @click="back()">
					<span class="icon iconfont "></span>
					<span class="text">取消</span>
				</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				baseUrl: '',
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
				type: '',
				userTableName: localStorage.getItem('UserTableName'),
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
					discussnum: '',
					storeupnum: '',
				},


				rules: {
					title: [
					],
					info: [
					],
					duration: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					cover: [
					],
					author: [
					],
					playcount: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					danmaku: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					likes: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					favorite: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					share: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					commentcount: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
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
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					clicktime: [
					],
					discussnum: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
					storeupnum: [
						{ validator: this.$validate.isIntNumer, trigger: 'blur' },
					],
				},
				centerType: false,
			};
		},
		computed: {



		},
		components: {
		},
		created() {
			if(this.$route.query.centerType){
				this.centerType = true
			}
			//this.bg();
			let type = this.$route.query.type ? this.$route.query.type : '';
			this.init(type);
			this.baseUrl = this.$config.baseUrl;
		},
		methods: {
			getMakeZero(s) {
				return s < 10 ? '0' + s : s;
			},
			// 下载
			download(file){
				window.open(`${file}`)
			},
			// 初始化
			init(type) {
				this.type = type;
				if(type=='cross'){
					var obj = JSON.parse(localStorage.getItem('crossObj'));
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
							this.ruleForm.cover = obj[o]?obj[o].split(",")[0]:'';
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
				}else if(type=='edit'){
					this.info()
				}

				if (localStorage.getItem('raffleType') && localStorage.getItem('raffleType') != null) {
					localStorage.removeItem('raffleType')
					setTimeout(() => {
						this.onSubmit()
					}, 300)
				}
			},

			// 多级联动参数
			// 多级联动参数
			info() {
				this.$http.get(`hotvideo/detail/${this.$route.query.id}`, {emulateJSON: true}).then(res => {
					if (res.data.code == 0) {
						this.ruleForm = res.data.data;
					}
				});
			},
			// 提交
			async onSubmit() {
				if(!this.ruleForm.id) {
					delete this.ruleForm.userid
				}
				await this.$refs["ruleForm"].validate(async valid => {
					if(valid) {
						if(this.type=='cross'){
							var statusColumnName = localStorage.getItem('statusColumnName');
							var statusColumnValue = localStorage.getItem('statusColumnValue');
							if(statusColumnName && statusColumnName!='') {
								var obj = JSON.parse(localStorage.getItem('crossObj'));
								if(!statusColumnName.startsWith("[")) {
									for (var o in obj){
										if(o==statusColumnName){
											obj[o] = statusColumnValue;
										}
									}
									var table = localStorage.getItem('crossTable');
									await this.$http.post(table+'/update', obj).then(res => {});
								}
							}
						}


						await this.$http.post(`hotvideo/${this.ruleForm.id?'update':this.centerType?'save':'add'}`, this.ruleForm).then(async res => {
							if (res.data.code == 0) {
								this.$message({
									message: '操作成功',
									type: 'success',
									duration: 1500,
									onClose: () => {
										this.$router.go(-1);
										
									}
								});
							} else {
								this.$message({
									message: res.data.msg,
									type: 'error',
									duration: 1500
								});
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
				this.$router.go(-1);
			},
			coverUploadChange(fileUrls) {
				this.ruleForm.cover = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");
			},
		}
	};
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.add-update-preview {
		padding: 0 0 20px;
		margin: 0px auto;
		color: #666;
		background: #f6f6f6;
		width: 1400px;
		font-size: 16px;
		position: relative;
		.add-update-form {
			border: 0px solid #fcbb78;
			padding: 20px;
			margin: 20px 0;
			background: #fff;
			width: 100%;
			position: relative;
			.add-item.el-form-item {
				border-radius: 0px;
				padding: 6px 0 0;
				margin: 0 0 20px 0;
				background: none;
				border-color: #475a8310;
				border-width:  0 0 0px;
				border-style: solid;
				/deep/ .el-form-item__label {
					padding: 0 10px 0 0;
					color: #666;
					font-weight: 500;
					width: 180px;
					font-size: inherit;
					line-height: 40px;
					text-align: right;
				}
				/deep/ .el-form-item__content {
					margin-left: 180px;
				}
				.el-input {
					width: auto;
				}
				.el-input /deep/ .el-input__inner {
					border: 1px solid #ddd;
					border-radius: 4px;
					padding: 0 12px;
					box-shadow: none;
					color: inherit;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				.el-input /deep/ .el-input__inner[readonly="readonly"] {
					border: 0;
					cursor: not-allowed;
					border-radius: 0px;
					padding: 0 12px;
					box-shadow: none;
					color: rgba(85, 85, 127, 1.0);
					background: none;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				.el-input-number /deep/ .el-input__inner {
					text-align: left;
					border: 1px solid #ddd;
					border-radius: 4px;
					padding: 0 12px;
					box-shadow: none;
					color: inherit;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				.el-input-number /deep/ .is-disabled .el-input__inner {
					text-align: left;
					border: 0;
					cursor: not-allowed;
					border-radius: 0px;
					padding: 0 12px;
					box-shadow: none;
					color: rgba(85, 85, 127, 1.0);
					background: none;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				.el-input-number /deep/ .el-input-number__decrease {
					display: none;
				}
				.el-input-number /deep/ .el-input-number__increase {
					display: none;
				}
				.el-select {
					width: auto;
				}
				.el-select /deep/ .el-input__inner {
					border: 1px solid #ddd;
					border-radius: 4px;
					padding: 0 10px;
					color: inherit;
					width: 100%;
					font-size: 16px;
					min-width: inherit !important;
					height: 40px;
				}
				.el-select /deep/ .is-disabled .el-input__inner {
					border: 0;
					cursor: not-allowed;
					border-radius: 0px;
					padding: 0 10px;
					box-shadow: none;
					color: inherit;
					background: none;
					width: 100%;
					font-size: 16px;
					height: 40px;
				}
				.el-date-editor {
					width: auto;
				}
				.el-date-editor /deep/ .el-input__inner {
					border: 1px solid #ddd;
					border-radius: 4px;
					padding: 0 10px 0 30px;
					box-shadow: none;
					color: inherit;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				.el-date-editor /deep/ .el-input__inner[readonly="readonly"] {
					border: 0;
					cursor: not-allowed;
					border-radius: 0px;
					padding: 0 10px 0 30px;
					box-shadow: none;
					color: inherit;
					background: none;
					width: auto;
					font-size: 16px;
					height: 40px;
				}
				/deep/ .el-upload--picture-card {
					background: transparent;
					border: 0;
					border-radius: 0;
					width: auto;
					height: auto;
					line-height: initial;
					vertical-align: middle;
				}
				/deep/ .upload .upload-img {
					border: 1px solid #ddd;
					cursor: pointer;
					border-radius: 4px;
					color: #999;
					background: #fff;
					width: 80px;
					font-size: 26px;
					line-height: 60px;
					text-align: center;
					height: 60px;
				}
				/deep/ .el-upload-list .el-upload-list__item {
					border: 1px solid #ddd;
					cursor: pointer;
					border-radius: 4px;
					color: #999;
					background: #fff;
					width: 80px;
					font-size: 26px;
					line-height: 60px;
					text-align: center;
					height: 60px;
					font-size: 14px;
					line-height: 1.8;
				}
				/deep/ .el-upload .el-icon-plus {
					border: 1px solid #ddd;
					cursor: pointer;
					border-radius: 4px;
					color: #999;
					background: #fff;
					width: 80px;
					font-size: 26px;
					line-height: 60px;
					text-align: center;
					height: 60px;
				}
				/deep/ .el-upload__tip {
					color: #888;
					font-size: 16px;
				}
				.el-textarea /deep/ .el-textarea__inner {
					border: 1px solid #ddd;
					border-radius: 4px;
					padding: 12px;
					box-shadow: none;
					color: inherit;
					width: auto;
					font-size: 16px;
					min-height: 150px;
					min-width: 48%;
					height: auto;
				}
				.el-textarea /deep/ .el-textarea__inner[readonly="readonly"] {
					border: 0px solid #ddd;
					cursor: not-allowed;
					border-radius: 0px;
					padding: 12px;
					box-shadow: none;
					color: inherit;
					background: none;
					width: auto;
					font-size: 16px;
					min-height: 150px;
					min-width: 50%;
					height: auto;
				}
				/deep/ .el-input__inner::placeholder {
					color: inherit;
					font-size: inherit;
				}
				/deep/ textarea::placeholder {
					color: inherit;
					font-size: inherit;
				}
				.editor {
					background-color: #fff;
					border-radius: 0;
					padding: 0;
					box-shadow: none;
					margin: 0;
					width: 100%;
					min-height: 350px;
					border-color: #ccc;
					border-width: 1px;
					border-style: solid;
					height: auto;
				}
				.upload-img {
					object-fit: cover;
					width: 100px;
					height: 100px;
				}
				.viewBtn {
					border: 0;
					cursor: pointer;
					border-radius: 4px;
					padding: 0 20px;
					margin: 0;
					color: #fff;
					background: #0674fc60;
					display: inline-block;
					width: auto;
					font-size: 14px;
					line-height: 34px;
					height: 34px;
				}
				.viewBtn:hover {
				}
				.unviewBtn {
					border: 0;
					cursor: pointer;
					padding: 0 20px;
					margin: 0;
					color: #333;
					display: inline-block;
					font-size: 14px;
					line-height: 34px;
					border-radius: 4px;
					outline: none;
					background: #eee;
					width: auto;
					height: 34px;
				}
				.unviewBtn:hover {
				}
			}
			.add-btn-item {
				padding: 0;
				margin: 20px 0;
				.submitBtn {
					border: 0;
					cursor: pointer;
					padding: 0 24px;
					margin: 0 20px 0 0;
					display: inline-block;
					font-size: 16px;
					line-height: 44px;
					border-radius: 4px;
					background: #0674fc;
					width: auto;
					text-align: center;
					min-width: 120px;
					height: 44px;
					.icon {
						color: #fff;
					}
					.text {
						color: #fff;
					}
				}
				.submitBtn:hover {
					.icon {
					}
					.text {
					}
				}
				.closeBtn {
					border: 0px solid #ddd;
					cursor: pointer;
					padding: 0 24px;
					margin: 0 20px 0 0;
					color: #fff;
					display: inline-block;
					font-size: 16px;
					line-height: 44px;
					border-radius: 4px;
					background: #858585;
					width: auto;
					text-align: center;
					min-width: 120px;
					height: 44px;
					.icon {
						color: #fff;
					}
					.text {
						color: #fff;
					}
				}
				.closeBtn:hover {
					.icon {
					}
					.text {
					}
				}
			}
		}
	}
	.el-date-editor.el-input {
		width: auto;
	}
</style>
