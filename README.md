

# 第三方打卡接入贝聊文档

## 必读
	* 为了唯一标识、安全、接入简单的因素综合考虑，贝聊第三方api统一使用appid、appsecret认证机制。第三方厂商需要在接入贝聊系统一开始就申请该信息，当前只支持人工申请。
	* 当第三方销售打卡器给到某个幼儿园时，为了准确匹配该幼儿园在贝聊系统的身份，需要第三方联系贝聊客服人员确保该幼儿园已经接入贝聊，并申请该幼儿园的接入许可码
	* 得到上述信息后，第三方的程序就可以开始进行接入贝聊的具体工作了。
	* 使用幼儿园许可码可以获取最新的幼儿园组织结构数据，包括幼儿园名称、班级列表
	* 绑定卡片号码到某个家长（支持一卡批量绑定都多个帐号）
	* 上传打卡信息
	

## 步骤一（人工）：申请第三方应用接入认证信息
	+ appid	APP标志	(测试：08560e99366c6653e9a408f154da734c)
	+ appsecret APP校验码 （测试：c724f2d614aabc62376c94f67170c458）

## 步骤二（人工）：申请单个幼儿园的接入许可码
	+ 注意：为了系统的安全性，单个幼儿园的可控绑定特性，每个幼儿园都有关联appid的唯一许可接入码，该号码需要在打卡系统接入每个幼儿园时单独向贝聊客服索取。
	+ authkid	幼儿园授权码 （测试：4b77d8e78c6ef930130170c13ee7ffc3）


## 步骤三（API）：根据幼儿园许可码获取幼儿园组织信息（包括幼儿园名称、班级列表）
	+ 说明：
		该接口需要定期调用，或者第三方程序需要有个手动更新的按钮。因为幼儿园的班级列表会经常发生变化。

	+ 请求参数：
		* appid
		* appsecret
		* action：（service/meta/get）
		* authkid	（幼儿园授权码）
	
	+ HTTP方法
		* POST

	+ curl请求实例
		curl 

	+ 返回实例




## 步骤四（API）：批量绑定卡片信息到手机号码

	+ 说明：
		该接口支持批量功能，已使用贝聊的用户可以直接绑定、未使用贝聊的用户会先注册后绑定

	+ 请求参数：
		* appid
		* appsecret	
		* action：	（service/card/match）
		* authkid	（幼儿园授权码）
		* cardnum	（卡号）
		* classid	（班级id）
		* data	（13632241882,XXX;13632231882,DDD;）（单个用户使用半角分号分隔，用户自己的手机号码和名称使用逗号分隔，手机号码长度固定11位，名称不超过20个中文字符）

	+ HTTP方法
		* POST

	+ curl请求实例
		curl 

	+ 返回实例

## 步骤五（API）：上传卡片打卡信息
	+ 说明：
		每次打卡时间发生时，第三方的程序上报打卡数据给到贝聊接口
		
	+ 参数：
		* appid
		* appsecret
		* action：（service/card/kick）
		* authkid	（幼儿园授权码）
		* cardnum	（卡号）
		* time_kick （打卡时间，用户实际的打卡时间）
		* image_base64 （图片内容，将图片的二进制内容先进行base64编码处理，然后使用该参数上传给贝聊）

	+ HTTP方法
		* POST

	+ curl请求实例
		curl 

	+ 返回实例


## API请求可能涉及到的错误码定义
	* 5001: 未授权第三方应用，请联系客服
	* 5002: 未授权幼儿园，请联系客服
	* 5101: 授权幼儿园信息已删除
	* 5102: 绑定数据格式错误，请认真检查数据
	* 5103: 绑定数据错误，手机号码格式错误
	* 5104: 绑定数据错误，用户名长度错误，最大20个中文字符，不能为空
	* 5105: 绑定数据错误，班级不存在或被删除或已经毕业归档
	* 5106: 绑定数据错误，卡号未绑定或绑定已失效

