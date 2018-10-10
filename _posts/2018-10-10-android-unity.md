//~작성중~---//~작성중~
layout: post
title: 안드로이드에서 유니티, 다른 안드로이드 프로젝트(OpenCV적용) 불러오기
---

1. 빈 프로젝트에 기존 안드로이드 프로젝트 Import

2. MainActivity와 연결하기 전 

3. 작동이 확인되면 그 다음 유니티 new gradle 옵션으로 빌드 후 export

4. 막상 적용하고 실행하면 어플 자체에서는 오류가 안나지만 유니티를 실행하면 디바이스 오류
 
5. 유니티는 안드로이드에서 armeabi-v7a과 x86만 지원함으로 armeabi-v8이 아닌 armeabi-v7a로 지정해야함(opencv java sdk가 x86을 지원하는지 여부 확인)
