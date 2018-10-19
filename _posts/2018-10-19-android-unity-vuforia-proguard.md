---
layout: post
title: 안드로이드에서 뷰포리아 적용된 유니티 프로젝트 프로가드 설정
---

Unity(Vuforia) in Android proguard setting

```text
# 아래는 유니티 Default로 있는 ProGuard규칙
-keep class bitter.jnibridge.* { *; }
-keep class com.unity3d.player.* { *; }
-keep class org.fmod.* { *; }
-keep class unity.samyoung.* {*;}

# 아래는 Vuforia를 위한 ProGuard 규칙
#-keep class com.android.library.** { *; } #만약 유니티 프로젝트를 라이브러리로써 임포트 시켰을 경우 주석 
-keep class com.vuforia.** { *; }
-keep interface com.vuforia.** { *; }
-keep class com.osterhoutgroup.** { *; }
-keep class com.ti.s3d.** { *; }
-dontwarn com.vuforia.ar.**

```
