---
layout:   post
title:    Android GPS Setup
date:     2015-03-25
summary:  어플리케이션에 필요한 GPS 설정 여부를 판단하는 코드에 대한 정리입니다.
---

#### GPS 설정 여부 확인  

로딩 중 설정 여부 판단을 위해 `LoadingActivity`에 *onCreate*안에 아래의 코드를 추가한다. 사용자 휴대폰의 GPS 설정을 체크하여 비활성 상태일 경우 팝업창을 띄우고, 활성 상태일 경우 그냥 지나간다.

{% highlight ruby %}
String context = Context.LOCATION_SERVICE;
LocationManager locationManager = (LocationManager)getSystemService(context);

if(!locationManager.isProviderEnabled(LocationManager.GPS_PROVIDER)) {
  alertCheckGPS();
} else {
  finish();
}
{% endhighlight %}  


#### GPS 설정 팝업창  

GPS 설정화면으로 이동 여부를 묻는 팝업창이다. '확인'을 누를 경우 설정 창으로 이동하며, '취소'를 누를 경우 `MainActiviy`로 바로 이동한다.

{% highlight ruby %}
private void alertCheckGPS() {
  AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("위치 서비스 설정").setMessage("위치 정보 사용 설정이 꺼져있습니다. 설정으로 이동하시겠습니까?").setCancelable(false).setPositiveButton("확인", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {
      Intent intent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
                        startActivity(intent);
    }
  
  }).setNegativeButton("취소", new DialogInterface.OnClickListener(){
    @Override
    public void onClick(DialogInterface dialog, int which) {
      dialog.cancel();
    }
  }).show();
}
{% endhighlight %}  


#### GPS permission 설정  

> permission?  
> 1. 허락, 허가  
> 2. (문서로 된) 승인  
> 출처: [네이버 영어사전][naver-endic]  

시스템에서 특정 파일이 특정 역할을 할 수 있게 권한을 주는 작업을 해야한다. 퍼미션을 주지 않으면 어플리케이션이 강제 종료되므로 `AndroidManifast`에 아래의 코드를 추가한다.

{% highlight ruby %}
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
{% endhighlight %}  


![팝업창 이미지](/images/img1.png)



[naver-endic]: http://endic.naver.com/enkrEntry.nhn?sLn=kr&entryId=ed047f1c3922452eb106d322f1f3c318