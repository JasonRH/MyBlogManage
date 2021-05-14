---
title: NFC触碰连接蓝牙
categories: 前端
tags:
  - Android
abbrlink: 9503
date: 2021-03-25 18:38:00
---

工具类
```
package com.southgnss.survx.device.nfc;

import android.app.Activity;
import android.app.PendingIntent;
import android.content.Intent;
import android.content.IntentFilter;
import android.nfc.NdefMessage;
import android.nfc.NdefRecord;
import android.nfc.NfcAdapter;
import android.nfc.tech.MifareClassic;
import android.nfc.tech.NfcA;
import android.os.Parcelable;
import android.os.SystemClock;
import android.text.TextUtils;
import android.util.Log;

import com.southgnss.library.device.ConnectListener;
import com.southgnss.library.device.DeviceParManage;
import com.southgnss.library.device.DeviceType;
import com.southgnss.library.device.TopDataIOFactory;
import com.southgnss.library.device.TopDeviceManage;
import com.southgnss.library.device.message.FunctionEnablePar;
import com.southgnss.library.util.ProgramConfigWrapper;
import com.southgnss.survx.device.setting.StringManage;

public class NfcUtils {
    private NfcAdapter mNfcAdapter;
    private IntentFilter[] mIntentFilter = null;
    private PendingIntent mPendingIntent = null;
    private String[][] mTechList = null;
    private long lSetLocationSysTime = 0L;

    private static NfcUtils nfcUtils;

    public static NfcUtils getInstance(Activity activity) {
        if (nfcUtils == null) {
            synchronized (NfcUtils.class) {
                if (nfcUtils == null) {
                    nfcUtils = new NfcUtils(activity);
                }
            }
        }
        return nfcUtils;
    }

    public NfcUtils(Activity activity) {
        NfcInit(activity);
        mNfcAdapter = NfcAdapter.getDefaultAdapter(activity);
    }

    /**
     * 首先，检查NFC是否打开
     */
    public boolean isEnabled() {
        return mNfcAdapter != null && mNfcAdapter.isEnabled();
    }

    /**
     * 初始化nfc设置
     */
    private void NfcInit(Activity activity) {
        Intent intent = new Intent(activity, activity.getClass());
        intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
        mPendingIntent = PendingIntent.getActivity(activity, 0, intent, 0);
        //做一个IntentFilter过滤你想要的action 这里过滤的是ndef
        IntentFilter filter = new IntentFilter(NfcAdapter.ACTION_NDEF_DISCOVERED);
        try {
            filter.addDataType("*/*");
        } catch (IntentFilter.MalformedMimeTypeException e) {
            e.printStackTrace();
        }
        mTechList = new String[][]{{MifareClassic.class.getName()}, {NfcA.class.getName()}};
        //生成intentFilter
        mIntentFilter = new IntentFilter[]{filter};
    }

    /**
     * 开启前台调度系统
     */
    public void enableForegroundDispatch(Activity activity) {
        if (mNfcAdapter != null && mIntentFilter != null) {
            mNfcAdapter.enableForegroundDispatch(activity, mPendingIntent, mIntentFilter, mTechList);
        }
    }

    /**
     * 关闭前台调度系统
     */
    public void disableForegroundDispatch(Activity activity) {
        if (mNfcAdapter != null && mIntentFilter != null) {
            mNfcAdapter.disableForegroundDispatch(activity);
        }
    }

    /**
     * 读取到的NFC数据
     */
    public void resolveIntent(Intent intent) {
        if (SystemClock.elapsedRealtime() - lSetLocationSysTime < 8000L) {
            return;
        }
        lSetLocationSysTime = SystemClock.elapsedRealtime();
        String action = intent.getAction();
        if (NfcAdapter.ACTION_TAG_DISCOVERED.equals(action) || NfcAdapter.ACTION_TECH_DISCOVERED.equals(action) || NfcAdapter.ACTION_NDEF_DISCOVERED.equals(action)) {
            Parcelable[] rawArray = intent.getParcelableArrayExtra(NfcAdapter.EXTRA_NDEF_MESSAGES);
            NdefMessage[] msgs;
            //处理扫描蓝牙地址的
            if (rawArray != null) {
                msgs = new NdefMessage[rawArray.length];
                for (int i = 0; i < rawArray.length; i++) {
                    msgs[i] = (NdefMessage) rawArray[i];
                }

                for (final NdefRecord record : msgs[0].getRecords()) {
                    byte[] src = record.getPayload();
                    if (record.getPayload().length < 36) {
                        return;
                    }
                    StringBuilder macBuilder = new StringBuilder();
                    //解析蓝牙mac地址，倒过来放的
                    char[] buffer = new char[2];
                    for (int i = 7; i > 1; i--) {
                        buffer[0] = Character.toUpperCase(Character.forDigit((src[i] >>> 4) & 0x0F, 16));
                        buffer[1] = Character.toUpperCase(Character.forDigit(src[i] & 0x0F, 16));
                        macBuilder.append(buffer);
                        if (i == 2) {
                            break;
                        }
                        macBuilder.append(":");
                    }

                    StringBuilder boothBuilder = new StringBuilder();
                    for (int i = 21; i < src.length; i++) {
                        buffer[0] = Character.toUpperCase(Character.forDigit((src[i] >>> 4) & 0x0F, 16));
                        buffer[1] = Character.toUpperCase(Character.forDigit(src[i] & 0x0F, 16));
                        boothBuilder.append(buffer);
                    }
                    String strDeviceName = StringManage.decode(boothBuilder.toString());
                    String strDevice = strDeviceName + "|" + macBuilder.toString();
                }
            }
        }
    }
    
   /**
     * 程序退出，释放
     */
    public void nfcRelease() {
        if (mNfcAdapter != null) {
            mNfcAdapter = null;
        }
        if (nfcUtils != null) {
            nfcUtils = null;
        }
    }

    public NfcAdapter getNfcAdapter() {
        return mNfcAdapter;
    }


    public IntentFilter[] getIntentFilter() {
        return mIntentFilter;
    }

    public PendingIntent getPendingIntent() {
        return mPendingIntent;
    }

    public String[][] getTechList() {
        return mTechList;
    }
    
}


```

使用
```
public class MainActivity extends BaseActivity{
private NfcUtils nfcUtils;

     @Override
    public void initData() {
        //nfc初始化设置
        nfcUtils = NfcUtils.getInstance(this);
    }
    
     @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        //当该Activity接收到NFC标签时，运行该方法
        //调用工具方法，读取到的NFC数据
        if (intent.getAction() != null && intent.getAction().equalsIgnoreCase("android.nfc.action.NDEF_DISCOVERED")) {
            setIntent(intent);
            if (nfcUtils != null) {
                nfcUtils.resolveIntent(intent);
            }
            return;
        }
    }
    
    @Override
    protected void onResume() {
        super.onResume();
        if (nfcUtils != null && nfcUtils.isEnabled()) {
            nfcUtils.enableForegroundDispatch(this);
        }
    }
 
    @Override
    protected void onPause() {
        super.onPause();
        if (nfcUtils != null && nfcUtils.isEnabled()) {
            nfcUtils.disableForegroundDispatch(this);
        }
    }
    
    @Override
    protected void onDestroy() {
        if (nfcUtils != null ) {
            nfcUtils.nfcRelease();
        }
        super.onDestroy();
    }
   

}
```