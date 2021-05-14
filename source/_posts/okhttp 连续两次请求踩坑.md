---
title: okhttp 连续两次请求踩坑
categories: 移动端
tags:
  - Android
abbrlink: 11605
date: 2020-08-05 17:30:00
---

1.现象

APP只请求了一次，服务器显示有两次请求，回复了两次。结果网络拦截器中显示的服务器返回的response.body()是正确的，而retrofit2实际回调的response.body()是另一个返回结果。

2.原因
okhttp拦截器中使用了两次chain.process()。这就请求了两次。 第二次返回其它的结果可能是因为你已经取过一次body用于打印log，因为body只能取一次，取了之后就置空了。真想提前查看body可以用peekbody方法，也可以使用httpLoggingIntercept这个库打印。


3.错误示例

```
private static Interceptor MY_LOGGING_INTERCEPTOR = new Interceptor() {
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
            printParams(request.body());
            Log.d(TAG, "-----LoggingInterceptor----- :\nrequest url:" + request.url() + "\ntime:" + (t2 - t1) / 1e6d + "\nbody:" + content + "\n");
            return chain.proceed(chain.request());
        }

```

4.正确示例
```
private static Interceptor MY_LOGGING_INTERCEPTOR = new Interceptor() {
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
            printParams(request.body());
            Log.d(TAG, "-----LoggingInterceptor----- :\nrequest url:" + request.url() + "\ntime:" + (t2 - t1) / 1e6d + "\nbody:" + content + "\n");
            return response.newBuilder()
                    .body(okhttp3.ResponseBody.create(mediaType, content))
                    .build();
        }
```
