## 概述
Android, IOS 证件OCR插件（身份证，银行卡等）

---


## 插件接口说明
插件地址：[https://ext.dcloud.net.cn/plugin?id=2528](https://ext.dcloud.net.cn/plugin?id=2528)

接口如下
### 执行OCR识别
```
/*
 * 执行OCR识别
 * @param options(JSONObject), 包括如下字段：
 *        token：从服务器端获取，只可使用一次
 *        ocrType：ocr 类型，0：身份证正面，1：身份证反面
 *        from：从哪儿获取数据，0：从摄像头拍摄，1：从相册选择
 * @param callback 执行结果回调，传入参数为对象result，result包括如下几个字段：
 *        code：执行状态码，0000：正常，0001：本地执行异常，0002：服务器端返回异常，0003：用户取消了操作，0004：未授予权限
 *        msg：执行结果描述
 *        requestId：请求ID(用于定位问题)
 *        data：OCR结果（执行成功才有此字段）
 *        image：拍摄/选择的照片（B64编码的字符串）
 * 上述data字段内容如下（json字符串）：
 * 1. 身份证正面
    {
        "front":{
            "name":"李...", -- 姓名
            "gender":"男",  -- 性别
            "nationality":"汉", -- 民族
            "birthDay":"19930504", -- 生日
            "idNumber":"441283.....", -- 身份证号码
            "address":"广东省广州市......", -- 住址
            "portrait":"/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAQICAgICAg........." -- 截取身份证的照片
        }
    }
 * 2 身份证背面
  {
    "back":{
        "issuedBy":"北京海淀公安局", -- 签发机构
        "validDateStart":"2017.09.26", -- 开始有效日期
        "validDateEnd":"2037.09.26",  -- 失效日期
        "completeness":0 --身份证完整性: 0: 完整 1: 不完整，但关键信息内容区域都在图片内 2: 不完整，且有部分内容在区域外
    }
 }
 */
esandOcrModule.processOcr(options, callback);

```
## 其他信息
 1.完整接入文档：https://esandinfo.yuque.com/books/share/80f28828-9ab9-45f4-a73d-b10dc6f91083?#
 2.服务器端协议文档：https://market.aliyun.com/products/57000002/cmapi00042834.html
 3.后端管理控制台地址: http://openali.esandcloud.com
 4.技术支持
```
微信：esand_info
qq: 3626921591
电话：18033076802
邮箱：ruide.li@esandinfo.com
```
![wechatqrcode](http://open.esandcloud.com/share/index.php/s/hzT4Gb0BN81svae/download)

