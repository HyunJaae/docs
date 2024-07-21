---
description: Android App 생성 시 코드 구조에 대한 내용입니다.
---

# Android 기초

### <mark style="color:purple;">프로젝트 생성하기</mark>

Android Studio 를 OS 에 맞게 다운로드 받고 실행을 하면 아래와 같은 화면이 뜹니다.

<figure><img src="../../.gitbook/assets/스크린샷 2023-11-06 오전 12.00.34.png" alt=""><figcaption><p>MacBook Pro(Apple silicon)</p></figcaption></figure>

New Project 를 선택하고 Phone and Tablet 템플릿에서 Empty Activity 를 선택합니다.

<figure><img src="../../.gitbook/assets/스크린샷 2023-11-06 오전 12.02.51.png" alt=""><figcaption><p>Empty Activity</p></figcaption></figure>

그리고 원하는 프로젝트 이름과 앱을 실행할 수 있는 Android 의 최소 버전을 선택합니다.&#x20;

<figure><img src="../../.gitbook/assets/스크린샷 2023-11-06 오전 12.04.12.png" alt=""><figcaption><p>Create Project</p></figcaption></figure>

### <mark style="color:purple;">코드 구조</mark>

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.greetingcard.ui.theme.GreetingCardTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            GreetingCardTheme {
                // A surface container using the 'background' color from the theme
                Surface(modifier = Modifier.fillMaxSize(), color = MaterialTheme.colorScheme.background) {
                    Greeting("Android")
                }
            }
        }
    }
}

@Composable
fun Greeting(name: String, modifier: Modifier = Modifier) {
    Surface(color = Color.Cyan) {
        Text(
            text = "Hi, my name is $name!",
            modifier = modifier.padding(24.dp)
        )
    }
}

@Preview(showBackground = true)
@Composable
fun GreetingPreview() {
    GreetingCardTheme {
        Greeting("HyeonJae")
    }
}
```

* `onCreate(savedInstanceState: Bundle?)` 함수는 Kotlin 에서 `main()` 함수와 같은 역할이며 앱 실행 시 진입점이 되는 함수입니다.
* `onCreate()` 함수 내부에 있는 `setContent()` 함수는 구성 가능한 함수(Composable Function)를 통해 레이아웃을 정의하는 데 사용됩니다.
* `@Composable` 이 표시된 함수는 `setContent()` 함수 또는 다른 구성 가능한 함수에서 호출할 수 있습니다.

{% hint style="info" %}
<mark style="color:purple;">`@Composable`</mark>은 Jetpack Compose 에서 이 함수가 UI를 생성하는 데 사용된다고 Kotlin 컴파일러에 알립니다.
{% endhint %}

{% hint style="info" %}
<mark style="color:blue;">Jetpack Compose</mark>는 네이티브 UI를 빌드하기 위한 <mark style="color:blue;">Android의 최신 권장 도구 키트</mark>로 UI 개발을 간소화하고 가속화합니다.
{% endhint %}

`Greeting()` 함수는 구성 가능한 함수입니다. 구성 가능한 함수는 몇 가지 입력을 받아서 화면에 표시되는 내용을 생성합니다.

{% hint style="info" %}
구성 가능한 함수(Composable Function) 의 특징

1. 함수 이름은 대문자로 시작됩니다.
2. 함수 앞에 `@Composable` 주석을 추가합니다.
3. 아무것도 반환할 수 없습니다.
{% endhint %}

`DefaultPreview()` 함수는 Android Studio 에서 전체 앱을 빌드하지 않고도 앱이 어떻게 표시 되는지 확인할 수 있습니다. 미리보기 함수가 되려면 `@Preview` 를 추가해야 합니다.

`@Preview` 주석이 `showBackground` 라는 매개변수를 사용하는데 `true` 로 설정할 경우, 앱 미리보기에 배경이 추가됩니다.

배경 색상을 변경하고 싶다면 `Text` 를 <mark style="color:purple;">`Surface`</mark>로 둘러싸야 합니다. <mark style="color:purple;">`Surface`</mark>는 배경 색상이나 테두리와 같은 모양을 변경할 수 있는 UI 섹션을 나타내는 컨테이너입니다. <mark style="color:purple;">`Surface`</mark>의 매개변수로 `Color` 의 원하는 옵션을 전달하면 배경 색상을 변경할 수 있습니다.

이제 패딩을 추가해보겠습니다. <mark style="color:blue;">`Modifier`</mark>는 Composable 함수를 장식하는 데 사용됩니다. <mark style="color:blue;">`modifier.padding()`</mark> 함수를 사용하여 텍스트 주위에 공백을 추가했습니다.

{% hint style="info" %}
dp(독립형 픽셀) 단위는 다음 과정에서 알아 봅니다.
{% endhint %}

