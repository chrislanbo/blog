---
title: android创建文件并写入数据
date: 2017-05-23 14:34:46
tags: [android]
categories:
---


```java

/**
 *
 * @param filename
 * @param s 内容
 */
public static void writeJsonToSD(String filename , String s) {
    int start  = filename.lastIndexOf("/");
    String FILENAME = filename.substring(start)+".txt";
    String filePath = Environment.getExternalStorageDirectory()+"/"+"json";
    File fiPath = new File(filePath+"/"+FILENAME);
    FileOutputStream fos = null;
    if(!fiPath.exists()){
        //创建文件夹
        File path = new File(filePath);
        if(path.mkdir()){
            Log.i(TAG, "【writeJsonToSD】" + " 创建成功");
        }else{
            Log.i(TAG, "【writeJsonToSD】" + " 创建失败");
        }
    }else{
        Log.i(TAG, "【writeJsonToSD】" + " 文件夹已经存在");
    }
    try {
        File file = new File(filePath,FILENAME);
        fos = new FileOutputStream(file, true);
        fos.write(s.getBytes());
        fos.close();
        Log.i(TAG, "【writeJsonToSD】" + " 写入成功");
    } catch (Exception e) {
        e.printStackTrace();
    }
}

```


