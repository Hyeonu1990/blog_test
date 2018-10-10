//~작성중~---//~작성중~
layout: post
title: 안드로이드에서 유니티, 다른 안드로이드 프로젝트(OpenCV적용) 불러오기
---

본 포스트는 어느 정도 안드로이드 스튜디오의 기초적인 경험이 있다고 생각하고 진행해 나갑니다.
OpenCV가 적용된 안드로이드 프로젝트는 http://webnautes.tistory.com/1054 여기를 참고하며 만들었습니다.

1. 빈 프로젝트에 기존 안드로이드 프로젝트 Import

빈 프로젝트 생성과정은 생략하겠습니다. 빈 프로젝트를 만든 후 기존에 만들었던 프로젝트를 Import 시키려 합니다.

![img]({{ site.baseurl }}/images/android_unity/android_import_module_button.png)

만약 저처럼 Import 시키려는 프로젝트의 각종 명칭이 기본이름으로 되어있다면 아래와 같은 경고메세지가 등장합니다.

![img]({{ site.baseurl }}/images/android_unity/android_ccode_import_error.png)

메인모듈인 app이 필요하므로 Import에 체크하여 주시고 모듈이름을 변경해줍니다.

![img]({{ site.baseurl }}/images/android_unity/android_ccode_import_modify.png)

변경 후 완료를 누르면 프로젝트 구성에 프로젝트가 추가됩니다.(Import한 프로젝트 내부에 openCV 라이브러리 모듈이 들어가있어서 같이 Import 되었습니다.)

![img]({{ site.baseurl }}/images/android_unity/android_ccode_import_result.png)

아까 설명드린바와 같이 Import한 프로젝트는 기본 명칭으로 되어있었고 이제 메인 Activity의 이름을 바꾸려합니다.(기본값 MainActivity)

![img]({{ site.baseurl }}/images/android_unity/ccode_mainactivity_namechange_before.png)

Import한 모듈 내부의 MainActivity를 선택하고 오른쪽 버튼을 눌러 Refact -> Rename을 클릭합니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_mainactivity_namechange_start.png)

아래와 같이 이름을 수정하고 Preview를 통해 변경될 사항들을 확인하여 봅니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_mainactivity_namechange_change.png)

Preview를 눌렀다면 아래와 같은 메세지가 뜨며 Import된 모듈 내부에서만 수정되는지 확인하고 Do Refactor 버튼을 눌러줍니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_mainactivity_namechange_change2.png)

그럼 아래와 같이 java파일명과 class이름, AndroidManifest에 설정된 Activity 이름 등이 수정됩니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_mainactivity_namechange_change_result.png)



2. MainActivity와 연결하기 전 작업들

아래부터 Import 했던 프로젝트의 이름을 원활한 설명을 위해 CustomCode라고 명칭하겠습니다.

위에서 Activity의 이름을 바꾼 방법과 같이 src - main - res - layout - activity_main.xml을 activity_ccode.xml로 변경하겠습니다.

(MainActivity.java에 있는 activity_main이란 명칭도 바뀔 수 있으니 변경 후 확인바랍니다. onCreate의 setContentView(R.layout.activity_main) 부분)

![img]({{ site.baseurl }}/images/android_unity/ccode_layout_namechange_after.png)

CustomCode의 AndroidManifest.xml에서 아래와 같이 해당 부분을 수정합니다. 이제 이 프로젝트는 메인이 아니기 때문에 수정해줍니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_manifest_modify.png)

이제 CustomCode에서 build.gradle을 수정하도록 하겠습니다. 이 프로젝트를 어플리케이션이 아닌 라이브러리로 인식시키려 합니다.

아래와 같이 com.android.application을 com.android.library로 변경해주시고 applicationId부분을 지우거나 주석처리합니다.

![img]({{ site.baseurl }}/images/android_unity/ccode_build_gradle_modify.png)



3. 메인인 Activity에서 CustomCodeActivity 실행하기

간편하게 메인에서 버튼을 클릭하여 CustomCodeActivity를 실행하도록 하겠습니다.

우선 메인 프로젝트(app 모듈)의 build.gradle에서 밑에 dependencies부분에 implementation project(':CustomCode')내용을 추가합니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_add_customcode_project.png)

메인 프로젝트에서 아래와 같이 버튼을 생성해줍니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_makeccbutton.png)

MainActivty.java에 아래와 같이 코드를 추가합니다. 불러올 Activity의 import경로는 추가 프로젝트가 제대로 연결 되었다면 코드상에서 추가한 Activity의 이름을 쓰고 Alt+Enter로 쉽게 추가할 수 있습니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_load_customcode_activity.png)

이후 기기에서 어플을 실행하고 버튼을 누르면 작동하게 됩니다. 작동영상은 이 포스트 마지막에 있습니다.



3. 작동이 확인되면 그 다음 유니티 new gradle 옵션으로 빌드 후 export

4. 막상 적용하고 실행하면 어플 자체에서는 오류가 안나지만 유니티를 실행하면 디바이스 오류
 
5. 유니티는 안드로이드에서 armeabi-v7a과 x86만 지원함으로 armeabi-v8이 아닌 armeabi-v7a로 지정해야함(opencv java sdk가 x86을 지원하는지 여부 확인)
