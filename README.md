# 拇指先生 Mrthumb
[![](https://jitpack.io/v/Reone/Mrthumb.svg)](https://jitpack.io/#Reone/Mrthumb)

## 预览图
![img](https://github.com/Reone/Mrthumb/blob/master/simple/preview.gif)

## 库说明
- a simple easy video thumbnail provider
- 顺滑的获取视频缩略图
- 支持网络视频缩略图
- 使用方便

## 原理说明
- 使用MediaMetadataRetriever获取视频信息及缩略图
- 使用MediaMetadataRetrieverCompat支持FFmpeg和自带两种解码方式
- 使用线程异步加载缩略图并缓存
- 支持两种不同的加载顺序选择：顺序、乱序
- 获取不到缩略图时取最近的缩略图

## 引用说明
```groovy
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
```groovy
dependencies {
    implementation 'com.github.Reone:Mrthumb:v1.0.2'
}
```

## 使用说明

### 1.加载缩略图
```java
if (playState == NiceVideoPlayer.STATE_PREPARED) {
    //视频准备好后开始加载缩略图
    Mrthumb.obtain().buffer(videoUrl, videoDuration, Mrthumb.Default.COUNT);
    //更详细的可以调用如下方法
    //Mrthumb.obtain().buffer(videoUrl, null, videoDuration, Mrthumb.Default.RETRIEVER_TYPE, Mrthumb.Default.COUNT, Mrthumb.Default.THUMBNAIL_WIDTH, Mrthumb.Default.THUMBNAIL_HEIGHT);
}
```

### 2.获取缩略图 
```java
float percentage = (float) seekBar.getProgress() / seekBar.getMax();
Bitmap bitmap = Mrthumb.obtain().getThumbnail(percentage);
```

### 3.添加缓存进度回调
```java
Mrthumb.obtain().addProcessListener(new ProcessListener() {

    @Override
    public void onProcess(final int index, final int cacheCount, final int maxCount, final long time, final long duration) {
        if (delegate != null) {
            delegate.thumbProcessLog("cache " + time / 1000 + "s at " + index + " process:" + (cacheCount * 100 / maxCount) + "%");
        }
    }
});
```

### 4.回收资源
```java
Mrthumb.obtain().release();
```
