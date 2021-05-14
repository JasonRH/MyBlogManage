---
title: Retrofit + okhttp 下载问题
categories: 移动端
tags:
  - Android
abbrlink: 39047
date: 2020-08-05 17:45:00
---

#### 问题

 使用retrofit，添加okhttp拦截器，平常的json返回正常，但下载文件时，文件不准确。
 
 #### 解决
 原先：return response.newBuilder()
                    .body(okhttp3.ResponseBody.create(mediaType, content))
                    .build();
 
 建议：直接返回chain.request()不做其它操作没有问题，不能更改return，后续会出问题。建议不打印response.body()或者暂时取消拦截器。
 

```

 private static  Interceptor MY_LOGGING_INTERCEPTOR = new Interceptor() {
        private String TAG = "MY_LOGGING_INTERCEPTOR";

        @Override
        public okhttp3.Response intercept(Chain chain) throws IOException {
            Request request = chain.request();
            long t1 = System.nanoTime();
            okhttp3.Response response = chain.proceed(chain.request());
            long t2 = System.nanoTime();
            okhttp3.MediaType mediaType = response.body().contentType();
            Log.d(TAG, "intercept:返回的类型为： " + mediaType);
            String content = response.body().string();
            //printParams(request.body());
            Log.d(TAG, "-----LoggingInterceptor----- :\nrequest url:" + request.url() + "\ntime:" + (t2 - t1) / 1e6d + "\nbody:" + content + "\n");
            
            //下载文件会有问题
            /*return response.newBuilder()
                    .body(okhttp3.ResponseBody.create(mediaType, content))
                    .build();*/
        }
     
 }
          
```
