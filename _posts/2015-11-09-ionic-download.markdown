---
layout:     post
title:      "Ionic 添加自动升级功能"
subtitle:   "Ionic 自动升级"
date:       2015-11-09 22:05:00
author:     "Franklee"
header-img: "img/post-bg-02.jpg"
---
ionic plugin add uk.co.whiteoctober.cordova.appversion
ionic plugin add org.apache.cordova.file
ionic plugin add org.apache.cordova.file-transfer


$http.get('version.xml',function(data){
     $cordovaAppVersion.getAppVersion().then(function(version){
          if(version!=data.version){
              showUpdateConfirm(data);
          }
    });
});


function showUpdateConfirm(data){
  $ionicPopup.confirm({
      title:'版本升级',
      template:data.content,
      cancelTest:'取消',
      okText:'升级'
  }).then(function(res){
      if(!res)return;
      $ionicLoading.show({template:'已下载:0%'});
      var targetPath='file:///storage/sdcard0/Download/sjxx.apk';
      $cordovaFileTransfer.download(data.url,targetPath,{},true).then(function(result){
          $cordovaFileOpener2.open(targetPath,'application/vnd.android.package-archive').then(function(){
              $ionicLoading.show({template:'更新成功',duration:1000});
          },function(){
              $ionicLoading.show({template:'更新失败',duration:1000});
          });
          $ionicLoading.hide();    
      },function(err){
          $ionicPopup.alert({title:'发生错误',template:'下载失败\n'+JSON.stringify(err)});
          $ionicLoading.hide();
      },function(progress){
          $timeout(function(){
              var downloadProgress=(progress.loaded/progress.total)*100;
              $ionicLoading.show({
                  template:'已经下载:'+Math.floor(downloadProgress)+'%';
              });
              if(downloadProgress>99){
                  $ionicLoading.hide();
              }
          });  
      });

  });
}
