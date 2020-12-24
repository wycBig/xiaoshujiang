## 主题：xml与实体类相互转换

### 一、实体转xml
	1.使用XmlMapper中的writeValueAsString(V request ) 方法
	2.`  try {
            requestXml = xmlMapper.writeValueAsString(shareMessageRquest);
        } catch (JsonProcessingException e) {
            throw new AppException("解析参数异常：" + e.getMessage());
        }`
		## 注释：requestXml解析后的xml文件，shareMessageRquest是入参实体类，入参是类型要用相关的xml注解来指定对应的节点名称。
		

### 二、xml转实体
	1. 使用XmlMapper中的readValue(reponseXMl,MessageResponse.class)方法
	2. ` try {
            messageResponse = xmlMapper.readValue(responseXml, MessageResponse.class);
        } catch (JsonProcessingException e) {
            throw new AppException("反参解析失败：" + e.getMessage());
        }
		## 注释：`responseXml 是调接口返回的xml报文，MessageResponse 是反参实体类，反参对应的类名以及参数名也要用相关注解 注明相关节点类型。

#### eg：

##### 入参实体：

		``` @Data
		@JacksonXmlRootElement(localName = "Root")
		public class ShareMessageRquest implements Serializable {

			@JacksonXmlProperty(localName = "Phone")
			private String phone;

			@JacksonXmlProperty(localName = "Message")
			private String message;

			@JacksonXmlProperty(localName = "Linkid")
			private String linkId;

			@JacksonXmlProperty(localName = "SmsType")
			private String messageType;
		}
		```

##### 		反参实体：

		
		``` @Data
				@JacksonXmlRootElement(localName = "Response")
				public class MessageResponse {

					@JacksonXmlProperty(localName = "ResponseCode")
					private String code;

					@JacksonXmlProperty(localName = "ResponseMessage")
					private String message;

				}
		```