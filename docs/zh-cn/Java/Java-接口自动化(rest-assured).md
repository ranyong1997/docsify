# 介绍
REST Assured是一个可以简化HTTP Builder顶层 基于REST服务的测试过程的Java **DSL**（针对某一领域，具有受限表达性的一种计算机程序设计语言）。它支持发起POST,GET,PUT,DELETE,OPTIONS,PATCH和HEAD请求，并且可以用来验证和校对这些请求的响应信息。
## 基础学习
官网文档：[https://github.com/artemave/rest-assured](https://github.com/artemave/rest-assured)
官网地址: [https://rest-assured.io/](https://rest-assured.io/)
框架搭建地址： [https://www.sohu.com/a/438327368_371153](https://www.sohu.com/a/438327368_371153)
# 核心代码
```java
public class BaseCase {
    public static String testurl = "接口测试地址(不变的那一部分)";
    public static String videourl = "测试环境地址";
    public static String RSAPublicKey = "RSA公钥";
    public static String RSAPrivateKey = "RSA私钥";
    public static String AESTestKey = "AES测试金钥";
    public static String AESMastKey = "AES主密钥";
    public static String accessKeyId = "存取金钥编号";
    public static String accessKeySecret = "访问密钥机密";
    public static String duid = "";
    public static MongoDbOperater operater = new MongoDbOperater("mongodb://.....");
    public static Mongodbutils mongodbutils = new Mongodbutils();


    @BeforeSuite
    public void beforeSuite() {

    }

    @AfterMethod
    public void afterSuite() {

    }

    public static Map<String, Object> headers() {
        Map<String, Object> map = new HashMap<String, Object>();
        Md5Utils md5Util = new Md5Utils();
        int ts = (int) (System.currentTimeMillis() / 1000);
        String openId = "";
        String key = "测试环境密钥";
         // 签名
        String sign = md5Util.doMD5(openId + ts + key);   // 拼接API密钥
        map.put("ts", valueOf(ts));  // 格式化字符串
        map.put("openid", openId);
        map.put("sign", sign);
        map.put("appVersion", "1.33.0");   // 必传现版本编码
        return map;
    }
}
```
# 万精油代码
```java
Get请求
@Test   // 采用了Test测试框架 最基本的注解，用来把方法标记为测试的一部分
    public void getTaskConfigP0() {
        final String url = testurl + "接口地址";  // final 用来修饰类（成员变量）、方法和变量
        String str = given().headers(headers()).params(getParams()).log().all().
                when().get(url).
                then().
                statusCode(200).assertThat().
                body("code",equalTo(200)).
                body("message",equalTo("success")).
                log().all().
                extract().path("data");
        System.out.println(AesUtils.decryptData(str,AESTestKey));    // 进行AES加密模式
    }

    private Map getParams() {
        Map<String, Object> params = new LinkedHashMap<>();
        params.put("key1", "value1");
        params.put("key2", "value2");
        return params;
    }
}

Post请求
@Test  // 采用了Test测试框架 最基本的注解，用来把方法标记为测试的一部分
    public void postAiUploadApi1() {
        final String url = testurl + "接口地址";  // final 用来修饰类（成员变量）、方法和变量
        String str = given().headers(headers()).body(getParams()).
        		when().post(url).
                then().
                statusCode(200).assertThat().
                body("code", equalTo(200)).
                log().all().
                extract().path("data");
        System.out.println(AesUtils.decryptData(str,AESTestKey));   // 进行AES加密模式
    }

	private String getParams(){
        Map map = new HashMap();
        map.put("key1","value1");
        map.put("key2","value2");
        String aesData = AesUtils.encryptData(JSON.toJSONString(map), AESTestKey);   // 数据加密
        return aesData;
    }
```


