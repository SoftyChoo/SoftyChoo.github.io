---
layout: post
title: kotlin
image: /assets/img/kotlin_img/kotlin.png
accent_image: 
  background: url('/assets/img/change_img/book.jpg') center/cover
  overlay: false
accent_color: '#fff'
theme_color: '#fff'
description: >
  kotlin
invert_sidebar: true
categories :
 - devlog	
 - kotlin






---

# [Android/Kotlin] [TIL] Single-expression function & SingleTon

단일표현식함수와 싱글톤에 대해서 알아보자

* toc
{:toc}




## 01. Single-expression function

- 람다식을 이용해 메소드를 간결하게 정의할 수 있다.
- JAVA8과 동일하게 Kotlin도 람다식을 지원한다.
- 하나의 메소드를 간결하게 표현할 수 있는 방법이다.

##### Kotlin의 람다식 구조

```kotlin
{매개변수1, 매개변수2... -> 
		코드
}
```

### 예시코드

- 람다식으로 표현한 3개의 숫자의 평균을 리턴해주는 함수

  ```kotlin
  fun add(num1:Int, num2:Int, num3:Int) = (num1+num2+num3)/3
  ```

- 메소드를 선언하지 않고 로직을 저장하는 함수

  ```kotlin
  var add = {num1: Int, num2: Int, num3: Int -> (num1+num2+num3) / 3}
  println("평균값은 ${add(10,20,30)}입니다")
  ```



## 02. 싱글톤(Singleton)

- 메모리 전역에서 유일한 객체임을 보장할 수 있다.

##### 장점

- 전역적으로 활용할 수 있어서 다른 클래스들에서 쉽게 접근할 수 있다.
- 만약 데이터가 전역에서 공통적으로 사용하는 정보라면 메모리를 더욱 효율적으로 활용 가능하다.
- 객체 자원간의 충돌을 방지할 수 있다.

##### 설명

- 일반적으로 객체는 자원을 필요한 만큼 생성할 수 있다.
- 각각의 객체는 상이한 위치정보를 가지고 있어 객체마다 고유한 저장하는 값을 가진다.
- 메모리 전역에서 유일함을 보장하기 위해 싱글톤을 사용하고 위치정보가 고정적이다.
- 프로그램이 실행되는 시점에 메모리에 바로 로드하여 위치를 잡는다.

##### ++

- 프로그램에서 키보드 객체를 무한하게 제작하려 한다면 입력 순서가 꼬일 수 있다.
- 키보드 객체는 싱글턴으로 제작해서 사용하고 싶을 때 객체를 얻어오는 방법으로 사용한다.

### 예시 코드

- 객체를 생성하지 않고 클래스 정보에 접근가능하다.

  -  [생성자 호출 X]

    ```kotlin
    fun main() {
        Bird.fly("참새")
    }
    
    object Bird {
        fun fly(name:String) {
            println("${name}가 날아요~")
        }
    }
    ```

  - [생성자 호출 O]

    ```kotlin
    fun main() {
        // trash와 같이 생성자에 매개변수 전달 가능
        var singletonObject1 = MySingletonClass.getInstance(trash = 1)
        singletonObject1.setNum(5)
        println("num값은: ${singletonObject1.getNum()}")
    
        // singletonObject2에서 num을 10으로 대입
        var singletonObject2 = MySingletonClass.getInstance(trash = 1)
        singletonObject2.setNum(10)
    
        // singletonObject1의 num이 10으로 출력됨
        // singletonObject1과 singletonObject2는 같은 객체를 공유하기 때문
        println("num값은: ${singletonObject1.getNum()}")
    
    }
    
    class MySingletonClass private constructor() {
        private var num:Int = 0
    
        companion object {
            @Volatile private var instance: MySingletonClass? = null
            private var trash = 0
    
            fun getInstance(trash: Int): MySingletonClass {
                this.trash = trash
                // 외부에서 요청왔을때 instance가 null인지 검증
                if(instance == null) {
    		            // synchronized로 외부 쓰레드의 접근을 막음
    								// 쓰레드는 다음챕터에서 소개합니다!
    		            // 쓰레드간의 객체상태 혼돈을 막기위해 사용한다고 이해해주세요
                    synchronized(this) {
                        instance = MySingletonClass()
                    }
                }
                return instance!!
                
    //            엘비스연산자와 뒷장에서배울 scope function을 이용하면
    //            아래와같이 더욱 직관적인 코드 작성이 가능합니다
    //            return instance ?: synchronized(this) {
    //                // also는 호출한 객체를 it으로 넘김
    //                // instance가 null이라면 새로 생성하고 아니면 무시함
    //                instance ?: MySigletonClass().also {
    //                    instance = it
    //                }
    //            }
            }
        }
        fun setNum(num: Int) {
            this.num = num
        }
    
        fun getNum(): Int{
            return this.num
        }
    }
    ```

    
