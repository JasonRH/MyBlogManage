---
title: Android Uri和Path
date: 2019-01-19 14:09:23
categories: 前端
tags: [Android]
---

### 起因

拍照后要保存图片，我们需要指定一个存储图片路径的Uri。此时需要将file path转换为Uri。

打开相册，选取本地的二维码图片识别，此时需要将Uri转Path，以获取图片路径。



### Android Uri to Path

Android在4.4之后的版本(包括4.4)中，从相册中选取图片返回Uri进行了改动。所以我们无法通过该Uri来取得文件路径，从而解码图片，将其显示出来。

在4.4之后的，包括4.4的版本，返回的Uri有可能是以下的一种:

- content://com.android.providers.media.documents/document/image:642
- content://com.android.providers.downloads.documents/document/
- content://media/external/images/media/

不能直接通过前两种Uri直接获取到对应的表，所以需要"翻译一下":

```java
private void handleImageOnKitKat(Intent data) {
        String imagePath = null;
        Uri uri = data.getData();

        if (DocumentsContract.isDocumentUri(this, uri)) {
            String docId = DocumentsContract.getDocumentId(uri);
       if ("com.android.providers.media.documents".equals(uri.getAuthority())) {
            //Log.d(TAG, uri.toString());
            String id = docId.split(":")[1];
            String selection = MediaStore.Images.Media._ID + "=" + id;
            imagePath = getImagePath(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, selection);
          } else if ("com.android.providers.downloads.documents".equals(uri.getAuthority())) {
             //Log.d(TAG, uri.toString());
             Uri contentUri = ContentUris.withAppendedId(
                   Uri.parse("content://downloads/public_downloads"),
                   Long.valueOf(docId));
                   imagePath = getImagePath(contentUri, null);
            }
        } else if ("content".equalsIgnoreCase(uri.getScheme())) {
            //Log.d(TAG, "content: " + uri.toString());
            imagePath = getImagePath(uri, null);
        }
    }
    


private String getImagePath(Uri uri, String selection) {
        String path = null;
        Cursor cursor = getContentResolver().query(uri, null, selection, null, null);
        if (cursor != null) {
            if (cursor.moveToFirst()) {
                path = cursor.getString(cursor.getColumnIndex(MediaStore.Images.Media.DATA));
            }

            cursor.close();
        }
        return path;
    }
```



### Android Path To Uri

1.Uri.parse(path)

2.Uri.fromFile(new  File(path))

3. File Path To Media Uri

```java
public static Uri getMediaUriFromPath(Context context, String path) {
        Uri mediaUri = MediaStore.Images.Media.EXTERNAL_CONTENT_URI;
        Cursor cursor = context.getContentResolver().query(mediaUri,
                null,
                MediaStore.Images.Media.DISPLAY_NAME + "= ?",
                new String[] {path.substring(path.lastIndexOf("/") + 1)},
                null);

        Uri uri = null;
        if(cursor.moveToFirst()) {
            uri = ContentUris.withAppendedId(mediaUri,
                    cursor.getLong(cursor.getColumnIndex(MediaStore.Images.Media._ID)));
        }
        cursor.close();
        return uri;
    }
```









参考资料：

https://www.jianshu.com/p/f9a63fcc0b91

https://www.jianshu.com/p/33bc363290e9
