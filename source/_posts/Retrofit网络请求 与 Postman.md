---
title: Retrofit与Postman的对应关系
categories: 移动端
tags:
  - Android
abbrlink: 4005
date: 2021-04-12 15:42:00
---

1.postman里面的form-data
对应 @Part 标签，上传数据。
```
@Multipart
@POST
Call<JsonObject> post1(@Url String url, @PartMap Map<String, RequestBody> params, @PartMap Map<String, MultipartBody.Part> parts);

实现：
参数一：RequestBody name = RequestBody.create(MediaType.parse("application/form-data"), name);

参数二：RequestBody fileRQ = RequestBody.create(MediaType.parse("image/*"), file);
MultipartBody.Part part = MultipartBody.Part.createFormData("file", file.getName(), fileRQ);
```

2.postman里面的x-www-form-urlencoded
对应 @Field，以表单形式提交数据
```
@FormUrlEncoded
@POST
Call<JsonObject> post(@Url String url, @FieldMap Map<String, Object> params);
```

3.postman里面的raw
对应@Body ,在body中以json或text字符串的形式提交数据
```
@POST
Call<JsonObject> post2(@Url String url, @Body RequestBody test);
例子：RequestBody type = RequestBody.create(MediaType.parse("application/json"), string);

//或者不传RequestBody，直接传对象
@POST
Call<JsonObject> post2(@Url String url, @Body Object o);

例子：post(url，User),post(url，hashmap)

```

---
示例：
```
//将json数据打包成字符串一次性传输
Gson gson = new GsonBuilder().disableHtmlEscaping().create();
JSONObject  object = new JSONObject(gson.toJson(entity));
object.put("","");
HashMap<String, Object> hashMap = new HashMap<>();
hashMap.put("requestText", object.toString());
service.post(url,hashmap);

[JSONObject 和 JsonObject 的区别](https://blog.csdn.net/ceovip/article/details/77980832)
```