---
title: Android共享文件（FileProvider）
categories: 前端
tags:
  - Android
abbrlink: 34226
date: 2021-01-18 16:04:08
---

说明：android 7.0以后需要使用FileProvider配置访问路径

1.添加 FileProvider 到 AndroidManifest.xml
```

        <!-- FileProvider配置访问路径，适配7.0及其以上 -->
        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.provider"
            android:grantUriPermissions="true"
            android:exported="false">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_path"/>
        </provider>

    </application>
```
```
<!--@xml/file_path-->
<resources>
    <paths>
        <external-path
            path=""
            name="Export"/>
    </paths>
</resources>
```

2.分享
```
/**
     * 文件分享
     */
    protected void fileShare(String filePath) {
        Intent shareIntent = new Intent(Intent.ACTION_SEND);
        shareIntent.setType("*/*");
        File file = new File(filePath);
        if (file.exists()) {
            shareIntent.putExtra(Intent.EXTRA_STREAM, getFileUri(this, file));
        }
        shareIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        //设置分享列表的标题，并且每次都显示分享列表
        startActivity(Intent.createChooser(shareIntent, "分享到"));
    }

/*根据不同android版本使用不同的方式*/
    private Uri getFileUri(Context context, File file) {
        Uri uri;
        // 低版本直接用 Uri.fromFile
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.N) {
            uri = Uri.fromFile(file);
        } else {
            //使用 FileProvider 会在某些 app 下不支持（在使用FileProvider 方式情况下QQ不能支持图片、视频分享，微信不支持视频分享）
            uri = FileProvider.getUriForFile(context, context.getPackageName() + ".provider", file);
        }
        return uri;
    }
```

注意： android:authorities和FileProvider.getUriForFile的content是一致的。