<template>
	<div>
		<vc-upload>简单版上传</vc-upload>
		<br>
		<vc-upload
			:size="2"
			:show-tips="true"
			:multiple="true"
			@begin="handleBegin"
			@complete="handleComplete"
			@file-before="handleFileBefore"
			@file-start="handleFileStart"
			@file-error="handleFileError"
			@file-success="handleFileSuccess"
			@file-progress="handleFileProgress"
		>限制大小上传以及api</vc-upload>

		<div style="display: flex; flex-wrap: wrap">
			<div 
				v-for="(item, index) in list"  
				:key="index"
				:style="{ backgroundImage: `url(${item})` }"
				class="image"
			/>
		</div>
	</div>
</template>
<script>
import { Message } from 'iview';
import Upload from '../index';
import { VcInstance } from '../../vc/index';

VcInstance.init({
	Upload: {
		URL_UPLOAD_IMG_POST: 'https://wyaoa-new.ruishan666.com/uploadfile/upimg.json?action=uploadimage&encode=utf-8&code=oa',
		URL_UPLOAD_FILE_POST: 'https://wyaoa-new.ruishan666.com/uploadfile/upimg.json?action=uploadimage&encode=utf-8&code=oa'
	}
});

export default {
	name: "vc-upload-basic",
	components: {
		'vc-upload': Upload
	},
	data() {
		return {
			list: []
		};
	},
	computed: {
		
	},
	methods: {
		/**
		 * 总线
		 */
		handleBegin(files) {
			console.log(files);
			Message.loading({
				content: `上传中`
			});
		},
		handleComplete(info = {}) {
			console.log(`Error: ${info.error}, Success: ${info.success}, 总数：${info.total}`);
			console.log(info.imgs);
			Message.destroy();
		},
		/**
		 * 单个文件
		 */
		handleFileBefore(file, fileList) {
			console.log(`上传之前`);
			return new Promise((resolve, reject) => {
				resolve(file);
			});
		},
		handleFileStart(file) {
			console.log(`开始上传`);
		},
		handleFileSuccess(res, file) {
			console.log(`Success：${file.current}, 总数：${file.total}`);
			console.log(res);
			Message.destroy();
			Message.success({
				content: `上传成功`
			});

			this.list.push(res.data.url);
		},
		handleFileProgress(e, file) {
			console.log(`Progress: 当前：${file.current}, 总数：${file.total}`);
			console.log(e.percent);
		},
		handleFileError(res, file) {
			console.log(`Error: 当前：${file.current}, 总数：${file.total}`);
			console.log(res);
			Message.destroy();
			Message.error({
				content: res.msg
			});
		},
	}
};
</script>
<style lang="scss" scoped>
	.image {
		background-size: cover;
		width: 120px;
		height: 120px;
		border-radius: 3px;
		margin: 3px;
	}
</style>
