---
layout: post
title: "CTAssetsPickerController"
date: 2014-12-03 12:14:48 +0800
comments: true
categories: ios
---

![image](http://cdn.cocimg.com/bbs/attachment/Fid_19/19_98590_315f8ebbd058f7e.png "image")



###特色
1. 用用户的相册中选择多张图片以及视频
2. 有过滤的设置，只选择图片或只选择视频
3. 可以设置选择图片以及视频的最大数量
4. 平均5x fps
5. 遵从UIAccessibility协议

<!--more-->

###需要
Xcode 5，iOS6及以上

###安装

1. 导入CTAssetsPickerController.h文件
2. 创建并显示CTAssetsPickerController

		CTAssetsPickerController *picker = [[CTAssetsPickerController alloc] init];
		picker.delegate = self;
		[self presentViewController:picker animated:YES completion:NULL];




###定制
你可以设置最大选择文件的数量：

	picker.maximumNumberOfSelection = 10;

如果你希望只选择图片或者视频，创建ALAssetsFilter 并把它分配给assetsFilter

	picker.assetsFilter = [ALAssetsFilter allPhotos]; // 只选择图片

如果选择器是以弹窗的形式显示，可以去掉选择按钮

	picker.showsCancelButton = NO;

####实现CTAssetsPickerControllerDelegate
用户选择完成：

	- (void)assetsPickerController:(CTAssetsPickerController *)picker didFinishPickingAssets:(NSArray *)assets
	// assets contains ALAsset objects.



用户取消选择：

	- (void)assetsPickerControllerDidCancel:(CTAssetsPickerController *)picker;



####注意
CTAssetsPickerController并不会压缩所选择的图片和视频。但是你可以通过defaultRepresentation 属性来处理选择的对象。

例如：如果选择的是图片的话，可以像这样创建一个UIImage


	ALAssetRepresentation *representation = alAsset.defaultRepresentation;

    UIImage *fullResolutionImage =
    [UIImage imageWithCGImage:representation.fullResolutionImage
                        scale:1.0f
                  orientation:(UIImageOrientation)representation.orientation];



如果选择的是视频，则创建一个NSData：

	ALAssetRepresentation *representation = alAsset.defaultRepresentation;

    NSURL *url          = representation.url;
    AVAsset *asset      = [AVURLAsset URLAssetWithURL:url options:nil];

    AVAssetExportSession *session =
    [AVAssetExportSession exportSessionWithAsset:asset presetName:AVAssetExportPresetLowQuality];

    session.outputFileType  = AVFileTypeQuickTimeMovie;
    session.outputURL       = VIDEO_EXPORTING_URL;

    [session exportAsynchronouslyWithCompletionHandler:^{

        if (session.status == AVAssetExportSessionStatusCompleted)
        {
            NSData *data    = [NSData dataWithContentsOfURL:session.outputURL];
        }

    }];



请参考ALAssetRepresentation和AVAssetExportSession的文档

GitHub地址 [https://github.com/chiunam/CTAssetsPickerController](https://github.com/chiunam/CTAssetsPickerController "url")