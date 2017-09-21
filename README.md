片尾水印流程：
根据视频大小生成一个定长的黑视频存储在本地，如果本地已经有了这个视频，那么取出本地的视频使用。视频可能有多个分辨率，用文件名区分。
取到空视频之后 再取视频的最后一帧 使用混合滤镜将最后一帧和黑视频的每一帧混合。
然后再将模糊滤镜和水印滤镜加上，最终生成片尾滤镜视频。
最后再将片尾滤镜视频和正片视频合并，导出最终的视频。
取视频的最后一帧使用UIImage的一个类方法 thumbImage

另一种思路是在片尾截取1.5s的视频

+ (UIImage *_Nullable)thumbnailImageForVideo:(NSURL *_Nonnull)videoURL atTime:(NSTimeInterval)time 


## MaxVideo
多张图片合成视频；多个小视频合成大视频

## Usage

导入 `HandlerVideo.h` 头文件

```
/**
  *  图片合成视频
  *@param videoFullPath 合成路径
  *@param frameImgs 图片数组
  *@param fps 帧率
  *@param progressImageBlock 进度回调
  *@param completedBlock 完成回调
 */
- (void)composesVideoFullPath:(NSString *)videoFullPath
                    frameImgs:(NSArray<UIImage *> *)frameImgs
                          fps:(int32_t)fps
           progressImageBlock:(CompProgressBlcok)progressImageBlock
               completedBlock:(CompCompletedBlock)completedBlock;

/**
 *  多个小视频合成大视频
 *@param subsectionPaths 视频地址数组
 *@param videoFullPath 合成视频路径
 *@param completedBlock 完成回调
 */
- (void)combinationVideosWithVideoPath:(NSArray<NSString *> *)subsectionPaths videoFullPath:(NSString *)videoFullPath completedBlock:(CompFinalCompletedBlock)completedBlock;

/**
 * 将视频分解成图片
 *@param fileUrl 视频路径
 *@param fps 帧率
 *@param splitCompleteBlock 分解完成回调
 */
- (void)splitVideo:(NSURL *)fileUrl fps:(float)fps splitCompleteBlock:(SplitCompleteBlock) splitCompleteBlock;
```

Example

- 将视频分解成图片
```
 if (![[NSFileManager defaultManager] fileExistsAtPath:kSplitImages]) {
        [[NSFileManager defaultManager]createDirectoryAtPath:kSplitImages withIntermediateDirectories:YES attributes:nil error:nil];
    }
    
   // 分解视频
    NSString *path = @"/Users/vs/Desktop/07F3E1F6-534A-4339-A945-4FA5939E5F55.mp4";
    [[HandlerVideo sharedInstance] splitVideo:[NSURL fileURLWithPath:path] fps:10 splitCompleteBlock:^(BOOL success, NSMutableArray *splitimgs) {
        if (success && splitimgs.count != 0) {
            NSLog(@"----->> success");
            NSLog(@"---> splitimgs个数:%lu",(unsigned long)splitimgs.count);
        }
    }];

```

- 视频合并

```
    NSString *path1 = @"/Users/vs/Desktop/07F3E1F6-534A-4339-A945-4FA5939E5F55.mp4";
    NSString *path2 = @"/Users/vs/Desktop/F6220BFB-6550-4085-B8F1-BAA2F39907F4.mp4";
    self.conbineVideos = [NSMutableArray arrayWithObjects:path1,path2,nil];
    
    [[HandlerVideo sharedInstance] combinationVideosWithVideoPath:self.conbineVideos videoFullPath:kCombineVideo completedBlock:^(BOOL success, NSString *errorMsg) {
        if (success) {
            NSLog(@"---->  SUCCESS");
        }
    }];

```
