<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area" @click="chooseVideo">
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
chooseVideo(){
                // 上传视频
                uni.chooseVideo({
                    maxDuration:60,
                    count: 1,
                    camera: this.cameraList[this.cameraIndex].value,
                    sourceType: ['album'],
                    success: (responent) => {
                        let videoFile = responent.tempFilePath;
                        uni.uploadFile({
                            url:this.config.fileUrl,
                            method:"POST",
                            header:{
                                'Authorization':'bearer '+ uni.getStorageSync('token')
                            },
                            filePath:videoFile,
                            name:'file',
                            success: (res) => {                    
                                // let videoUrls = JSON.parse(res.data) //微信和头条支持
                                let videoUrls = res.data //百度支持
                                this.imagesUrlPath = this.imagesUrlPath.concat(videoUrls.result.filePath);
                                this.src = videoUrls.result.filePath; //微信
                                if(this.src) {
                                    this.itemList = ['图片']
                                } else {
                                    this.itemList = ['图片','视频']
                                }
                                
                            }
                        })
                        // this.src = responent.tempFilePath;  //头条
                    }
                })
            },
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
