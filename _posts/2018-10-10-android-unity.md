---
layout: post
title: 안드로이드에서 유니티, 다른 안드로이드 프로젝트(OpenCV적용) 불러오기
---

본 포스트는 어느 정도 안드로이드 스튜디오의 기초적인 경험이 있다고 생각하고 진행해 나갑니다.

유니티 부분도 큐브 하나 띄운 간단한 프로젝트라도 빌드했던 경험이 있다는 전제 하에 진행합니다. 유니티는 그냥 아무거나 만들어논 프로젝트로 이용하시면 됩니다만 기본적으로 유니티상에서 바로 안드로이드 빌드 후 정상작동하는 프로젝트여야 합니다.

OpenCV가 적용된 안드로이드 프로젝트는 http://webnautes.tistory.com/1054 여기를 참고하며 만들었습니다.

위 사이트 예제와 크게 다른점은 CMakeLists.txt에서 경로를 지정하는 부분이니 안드로이드 프로젝트 깃허브를 참조하바랍니다. https://github.com/Hyeonu1990/CustomCode/tree/master/CustomCode_Android

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



3. 메인 Activity에서 CustomCodeActivity 실행하기

간편하게 메인에서 버튼을 클릭하여 CustomCodeActivity를 실행하도록 하겠습니다.

우선 메인 프로젝트(app 모듈)의 build.gradle에서 밑에 dependencies부분에 implementation project(':CustomCode')내용을 추가합니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_add_customcode_project.png)

메인 프로젝트에서 아래와 같이 버튼을 생성해줍니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_makeccbutton.png)

MainActivty.java에 아래와 같이 코드를 추가합니다. 불러올 Activity의 import경로는 추가 프로젝트가 제대로 연결 되었다면 코드상에서 추가한 Activity의 이름을 쓰고 Alt+Enter로 쉽게 추가할 수 있습니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_load_customcode_activity.png)

이후 기기에서 어플을 실행하고 버튼을 누르면 작동하게 됩니다. 작동영상은 이 포스트 마지막에 있습니다.



4. 유니티에서 new gradle 옵션으로 빌드 후 export

본 포스트에서는 Vuforia를 활용한 간단한 AR샘플을 제작하여 사용하지만 그냥 큐브 하나 띄어놓은 단순한 프로젝트여도 아래 내용을 따라하는데 지장없습니다.

이미 완성된 프로젝트가 있다는 전제 하에 시작합니다. 유니티 메뉴에서 File - Build Settings를 누르면 나오는 창에서 아래와 같이 설정해 주시고 Export합니다.

![img]({{ site.baseurl }}/images/android_unity/build_settings.png)

아래는 Export된 내용입니다.

![img]({{ site.baseurl }}/images/android_unity/unity_export_result.png)

이제 해당 프로젝트를 추가하여봅시다. 다른 안드로이드 프로젝트를 불러온것과 같이 안드로이드에서 File - New - Import Module로 아까 Export했던 유니티 프로젝트를 불러옵니다. 아래는 그 결과입니다.

![img]({{ site.baseurl }}/images/android_unity/android_unity_import.png)

혹시 아래와 같은 오류가 떠도 일단 무시합니다.

![img]({{ site.baseurl }}/images/android_unity/android_unity_import_error.png)

Import된 유니티 모듈 내에 build.gradle에서 밑의 이미지의 오른쪽 코드에 맞게 변경합니다.

![img]({{ site.baseurl }}/images/android_unity/android_unity_build_gradle_modify.png)

그림에는 안나와있지만 build.gradle 위쪽에 buildscript에서 dependencies 부분에 gradle빌드버전 설정하는 부분이 있습니다.(아마 com.android.tools.build:gradle:2.1.0'로 설정되있을겁니다.) 전체 프로젝트가 같은 버전이여야 합니다. 아마 스튜디오 상에서 전구버튼으로 자동으로 바꿀 수도 있습니다.

그리고 유니티 모듈 내에 AndroidManifest.xml에서 밑의 이미지의 아래쪽 코드에 맞게 변경합니다

![img]({{ site.baseurl }}/images/android_unity/android_unity_manifest_modify.png)

일반적인 유니티 프로젝트라면 여기서 오류는 사라졌을 겁니다. 하지만 현재 샘플에 들어간 Vuforia라는 AR SDK때문에 추가 작업이 필요합니다. 해당사항이 아니신 분들은 메인 프로젝트와 유니티 프로젝트 연결하는 부분까지 쭉 내리시면 됩니다.

Vuforia 7버전 부터 유니티 내부에 들어갔으며 안드로이드 프로젝트로 Export 시 Vuforia 부분이 VuforiaWrapper.aar 파일로 들어가게 되었습니다. 혹시 저처럼 VuforiaWrapper에 관해서 계속 빌드 오류가 발생한다면 이 방법을 따라해보시기 바랍니다.

VuforiaWrapper.aar를 압축프로그램으로 실행 후 

VuforiaWrapper 내부에 있던 jni -> armeabi-v7a에 들어있는 3개의 파일을

유니티프로젝트 폴더 -> src -> main -> jniLibs -> armeabi-v7a 폴더에 집어넣습니다.

VuforiaWrapper 내부 libs 폴더에 들어있는 2개의 파일을

유니티프로젝트 폴더 -> libs 폴더에 집어넣습니다.

그리고 유니티프로젝트 폴더에 build.gradle에서

```text
dependencies {
	compile fileTree(dir: 'libs', include: ['*.jar'])
	compile(name: 'VuforiaWrapper', ext:'aar')
}
```

이 부분에서 VuforiaWrapper관련 부분을 주석처리하거나 제거합니다.

```text
dependencies {
	compile fileTree(dir: 'libs', include: ['*.jar'])
	//compile(name: 'VuforiaWrapper', ext:'aar')
}
```

일반 유니티 프로젝트와 뷰포리아 프로젝트 공통으로 compile에 대해서 경고메세지가 뜰텐데 complie을 implementation로 변경하면 해결됩니다. 차후 compile 명령어를 지원하지 않기 때문에 뜨는 경고메세지입니다.

마지막으로 유니티프로젝트의 AndroidManifest.xml과 VuforiaWrapper 내부의 AndroidManifest.xml와 비교하여

"<user-permission ~ " 이라고 시작하는 부분 중에 겹치지 않는, VuforiaWrapper 내부의 AndroidManifest.xml에서만 존재하는 권한 설정 부분을

유니티프로젝트의 AndroidManifest.xml에 추가해줍니다.



5. 메인프로젝트로 유니티 액티비티 띄우기

메인프로젝트 UI에서 유니티 액티비티를 실행할 버튼을 만들어줍니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_makeunitybutton.png)

메인프로젝트(app)의 build.gradel에서 implementation project(':유니티프로젝트이름') 을 추가합니다

![img]({{ site.baseurl }}/images/android_unity/mainactivty_add_unity_project.png)

MainActivity.java에서 onCreate 함수에 유니티 액티비티를 실행해줄 부분을 추가합니다.

![img]({{ site.baseurl }}/images/android_unity/mainactivty_load_unity_activity.png)



6. 빌드 후 발생 할 수 있는 문제점 및 작동 영상

![img]({{ site.baseurl }}/images/android_unity/unity_arm64-v8a_error.png)

혹시나 유니티버튼을 누르고 이런 메세지가 뜬다면 아마도 NDK빌드설정 문제입니다.
저도 갤럭시탭S2에서는 저런 메세지가 떴는데 보급형 스마트폰에서는 정상적으로 실행이 되서 원인을 찾게 되었습니다.

최신기기들은 기본값 arm64-v8a 형식으로 빌드되기 때문에 유니티가 지원하는 armeabi-v7a 형식에 맞지 않아 저런 경고 메세지가 뜹니다. 즉, 이 문제를 해결하기 위해서는 armeabi-v7a 형식으로 빌드되도록 지정해야합니다.

메인 프로젝트의 build.gradle에 아래 이미지와 같이 빨간색 부분을 추가하면 해결됩니다. 이 방법으로 특정 환경에서만 작동하도록 빌드되기 때문에 여러 형식이 섞여있는 OpenCV 프로젝트도 이 방법으로 빌드되는 APK용량을 축소 할 수 있습니다.

![img]({{ site.baseurl }}/images/android_unity/unity_arm64-v8a_error_solution.png)

구동영상

준비중...

{% youtube http://youtu.be/_q8NwdK-o00 %}
