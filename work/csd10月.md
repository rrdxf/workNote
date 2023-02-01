### 9月14日

**笔记：**

#### 在内联事件处理器中访问事件参数

9月18日

1、熟悉devops项目代码

2、解决

#### exchange

```java
HttpHeaders headers = new HttpHeaders();
headers.add("Content-Type","application/x-www-form-urlencoded");
//拼接URL和参数
UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl(url)
        .queryParam("loginMobile", "17333164450")
        .queryParam("password","ls111111.");
//封装头 //发送请求
HttpEntity<JSONObject> request = new HttpEntity<>(null, headers);
ResponseEntity<String> response=restTemplate.exchange(builder.build().encode().toUri(), HttpMethod.POST,request,String.class);
JSONObject res=JSON.parseObject(response.getBody());//       获取返回的请求body
```

