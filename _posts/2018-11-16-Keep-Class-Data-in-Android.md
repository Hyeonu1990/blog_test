---
layout: post
title: 안드로이드에서 NDK로 C++ Class 데이터 유지 및 Class 내 String값 
---

안드로이드에서 C++ 클래스의 포인터주소값을 Java 내에 long 형식으로 할당해두면 가비지 컬렉터에 의해 클래스 데이터가 사라지는걸 방지 할 수 

액티비티 자바 파일에서 처리할 부분, long 형식으로 주소값을 받아온다.
```java
public class MainActivity extends AppCompatActivity
{
  ~
  long classptr;
  public native long GetClassPtr();
  ~
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    ~
    classptr = GetClassPtr();
    ~
  }
  ~
}
```

C++ 코드에서 처리할 부분, 클래스를 생성하고 주소값을 return
```c++
extern "C"
JNIEXPORT jlong JNICALL
Java_'프로젝트 패키지 이름'_'액티비티 클래스 이름'_GetClassPtr(JNIEnv *env, jobject instance) {

    CustomClass* custom = new CustomClass();
    custom->Init();
    return (long)(custom);

}
```
-----------------------

long 형식으로 받아온 주소값을 활용하여 String값을 . 아래는 액티비티 클래스 코드
```java
public class MainActivity extends AppCompatActivity
{
  ~
  long classptr;
  public native long GetClassPtr();
  public String long GetStr_C(long ptr);
  String str;
  ~
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    ~
    classptr = GetClassPtr();
    str = GetStr_C(classptr);
    ~
  }
  ~
}
```

C++코드 부분. CustomClass에 String값을 return하는 GetStr함수가 있다고 가정
```c++
extern "C"
JNIEXPORT jlong JNICALL
Java_'프로젝트 패키지 이름'_'액티비티 클래스 이름'_GetClassPtr(JNIEnv *env, jobject instance) {

    CustomClass* custom = new CustomClass();
    custom->Init();
    return (long)(custom);

}

extern "C"
JNIEXPORT jlong JNICALL
Java_'프로젝트 패키지 이름'_'액티비티 클래스 이름'_GetStr_C(JNIEnv *env, jobject instance, jlong ptr) {

    CustomClass* custom = (CustomClass*)ptr;
    String Cstr = custom->GetStr();
    
    return env->NewStringUTF(Cstr.c_str());
}
```
