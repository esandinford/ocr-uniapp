<template>
    <view class="content" align="center">

        <view align="center">
            <image class='esand_logo' src="../../static/esand_logo.png"></image>
        </view>

        <view class="btn-row">
            <button type="primary" class="primary item" @tap="processOcr(0)">身份证正面</button>
        </view>

        <view class="btn-row">
            <button type="primary" class="primary item" @tap="processOcr(1)">身份证反面</button>
        </view>

        <img :src="image"/>
        <div align="center">
            <label>code: {{code}}</label>
            <label>msg: {{msg}}</label>
            <label>requestId: {{requestId}}</label>
            <textarea :value="data"/>
        </div>
    </view>
</template>
<script>
	const esandOcrModule = uni.requireNativePlugin('Esand-OcrModule');
	const APPCODE="换成你的APPCODE";
	export default {
	    data() {
	        return {
	            title: 'ocr证件识别',
	            code: 'code',
	            msg: 'msg',
	            requestId: 'requestId',
	            data: 'data',
	            image: ''
	        }
	    },
	    onLoad() {
	    },
	    methods: {
	        processOcr(ocrType) {
	            let that = this
	            uni.request({
	                // OCR 初始化后端地址, 为了您的数据安全，请保护好您的APPCODE, 最好把初始化这段逻辑放在服务器端。
	                url: "http://edisocr.market.alicloudapi.com/ocr/init",
	                method: 'POST',
	                data: {
	                    ocrType: ocrType
	                },
	                header: {
	                    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
	                    // TODO APPCODE切勿泄漏，需替换成您的APPCODE
	                    'Authorization': 'APPCODE ' + APPCODE,
	                },
	                success: (res) => {
	                    console.log('网络请求成功' + JSON.stringify(res));
	                    console.log("esandOcrModule : " + esandOcrModule)
	                    esandOcrModule.processOcr({
	                            ocrType: ocrType,
	                            token: res.data.token,
	                            from: 0,// 0：用相机拍摄，1：从相册中选取
	                        },
	                        result => {
								/*
							     *	      code：执行状态码，0000：正常，0001：本地执行异常，0002：服务器端返回异常，0003：用户取消了操作，0004：未授予权限
								 *        msg：执行结果描述
								 *        requestId：请求ID(用于定位问题)
								 *        data：OCR结果（执行成功才有此字段）
								 *        image：拍摄/选择的照片（B64编码的字符串）
								 * */
	                            that.code = result.code
	                            that.msg = result.msg
	                            that.requestId = result.requestId
	                            that.image = "data:image/png;base64," + result.image
	                            that.data = result.data
	                        });
	                },
	                fail: (res) => {
	                    console.log('网络请求失败' + JSON.stringify(res));
	                    return JSON.stringify(res.data);
	                },
	                complete: (res) => {
	                    console.log("执行完成");
	                }
	            });
	        }
	    }
	}
</script>
<style>
    .content {
        position: absolute;
        width: 100%;
        height: 100%;
    }

    .esand_logo {
        margin-top: 100rpx;
        margin-bottom: 100rpx;
        width: 200rpx;
        height: 200rpx;
        align-self: center;
    }

    .item {
        margin-bottom: 15rpx;
        margin-top: 15rpx;
        width: 400rpx;
    }

    img {
        position: relative;
        margin-top: 50rpx;
        width: 600rpx;
        height: 400rpx;
    }

    label {
        display: block;
        margin-top: 10rpx;
    }

    textarea {
        margin-top: 20rpx;
        height: 400rpx;
    }
</style>
