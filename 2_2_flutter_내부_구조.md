## 플러터 프로젝트 폴더 구성

|폴더|내용|비고|
|:---:|:---:|:---:|
|android|안드로이드 프로젝트 관련 파일|안드로이드 스튜디오로 실행 가능|
|ios|iOS 프로젝트 관련 파일|엑스코드로 실행 가능(맥 전용)|
|lib|플러터 앱 개발을 위한 다트 파일|플러터 SDK 설치 필요|
|test|플러터 앱 개발 중 테스트 파일|테스트 편의성 제공|

pubspec.yaml 파일은 플러터에서 다양한 패키지와 이미지, 폰트 등을 사용할 수 있게 해준다.

|파일|내용|비고|
|:---:|:---:|:---:|
|pubspec.yaml|패키지,이미지,폰트 설정|직접 관리|
|README.md|프로젝트 소개|직접 관리|
|.gitignore|깃(git)에 커밋, 푸시 등 소스 코드를 업로드할 때 필요없는 파일 기록|직접 관리|
|.metadate|Flutter SDK 정보|자동 관리|
|.packages|Flutter SDK에 사용하는 기본 패키지 경로|자동 관리|
|프로젝트 명.iml|파일이 자동으로 생성될 때 만들어지는 폴더 위치|자동 관리|
|pubspec.lock|pubspec.yaml 파일에 적용된 패키지 위치|자동 관리|

## 플러터 메인 소스 파일 구성

### import 구문
import는 해당 소스 파일에서 사용하려는 패키지를 불러올 때 사용하는 구문이다.      
다른 다트 클래스나 pubspec.yaml 파일에서 내려받은 패키지를 불러올 때도 사용한다.
~~~dart
import 'package:flutter/material.dart';
~~~

### main() 함수
플러터 앱은 자바나 C언어로 작성된 프로그램처럼 main() 함수에서 시작한다.

~~~dart
void main(){
    runApp(MyApp());
}
~~~
main() 함수에서는 runApp() 함수를 호출한다.      
runApp() 함수는 binding.dart 클래스에 정의되어 있으며 플러터 앱을 시작하는 역할을 한다.      

### MyApp 클래스
아래 코드는 main() 함수에서 runApp() 함수로 플러터 앱을 실행할 때 화면에 표시할 위젯으로 전달한 MyApp 클래스를 정의한 부분이다.

~~~dart
class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context){
        return MaterialApp(
            title:'Flutter Demo',
            theme:ThemeData(
                primarySwatch:Colors.blue,
                visualDensity: VisualDensity.adaptivePlatformDensity,
            ),
            home: MyHomePage(title:'Flutter Demo Home Page'),
        );
    }
}
~~~
build()함수는 어떤 위젯을 만들 것인지를 정의한다.    
처음 runApp()을 이용해 클래스를 실행할 때는 MaterialApp() 함수를 반환해야 한다.     
MaterialApp() 함수에는 그림을 그리는 도구에 속하는 title, theme 그리고 home 등이 정의되어 있다.     

## 상태 연결에 따른 위젯 구분

플러터 앱을 구성하는 위젯은 stateless와 statefull 두 가지로 구분할 수 있다.        
이러한 구분은 상태 연결과 관련이 있다.         
내용을 갱신할 필요가 없는 위젯은 화면에 보이기 전에 모든 로딩을 마친다. 즉, 앱이 위젯의 상태를 감시하고 있을 필요가 없는 것이다.      
이처럼 상태를 연결할 필요가 없는 위젯을 StatelessWidget 위젯이라고 하며 StatelessWidget 클래스를 상속받아 만든다.        

계산기와 같이 버튼을 누를 때마다 화면 내용을 갱신해야하는 경우도 있다.     
이러한 경우는 앱이 위젯의 상태를 감시하다가 위젯이 특정 상태가 되면 알맞은 처리를 수행해야 한다.      
이처럼 상태가 연결된 동적인 위젯을 StatefulWidget이라고 하며 StatefulWidget 클래스를 상속받아 만든다.        

## StatefulWidget의 생명주기

StatelessWidget은 한 번 만들어지면 갱신할 수 없으므로 생명주기가 없다.      
즉, 다른 화면으로 넘어가면 모든 로직이 종료된다.         
그러나 StatefulWidget은 10단계로 구분하는 생명주기가 있다.         

1. State를 생성하는 createState() 함수      
StatefulWidget 클래스를 상속받는 클래스는 반드시 createState() 함수를 호출해야 한다.        
이 함수는 다른 생명주기 함수들이 포함된 State 클래스를 반환한다.
~~~dart
class MyHomePage extends StatefulWidget{
    @override
    _MyHomePageState createState() => new _MyHomePageState();
}
~~~

2. Widget을 화면에 장착하면 mounted == true      
createState() 함수가 호출되어 State가 생성되면 곧바로 mounted 속성이 true로 변경된다.       
mounted 속성이 true라는 것은 Widget을 제어할 수 있는 buildContext 클래스에 접근할 수 있다는 의미이다.      
buildContext가 활성화되어야 setState() 함수를 이용할 수 있다.        
다음처럼 setState() 함수를 호출하기 전에 mounted 속성을 점검 코드로 활용하면 더 안전하게 작성할 수 있다.
~~~dart
if(mounted){
    setState()
}
~~~

3. Widget을 초기화하는 initState() 함수      
initState() 함수는 Widget을 초기화할 때 한 번만 호출한다.      
주로 데이터 목록을 만들거나 처음 필요한 데이터를 주고받을 때 호출한다.         
initState() 함수를 호출할 때 내부에서 _getJsonDate() 함수를 호출해 서버에서 받아온 데이터를 화면에 출력하게 만들 수 있다.        

~~~dart
@override
initState(){
    super.initState();
    _getJsonDate();
}
~~~

4. 의존성이 변경되면 호출하는 didChangeDependencies() 함수     
Widget을 초기화하는 initState() 함수가 호출된 후에 바로 호출되는 함수가 didChangeDependencies()이다.      
이 함수는 데이터에 의존하는 Widget이라면 화면에 표시하기 전에 꼭 호출해야 한다.      
주로 상속받은 Widget을 사용할 때 피상속자가 변경되면 호출한다.

5. 화면에 표시하는 build() 함수      
build() 함수는 Widget을 반환한다. 즉, Widget을 화면에 렌더링한다.     
build() 함수에서 Widget을 만들고 반환하면 비로소 화면에 표시된다.     
~~~dart
@override
Widget build(BuildContext context){
    return MaterialApp(
        title:'Flutter Demo',
        theme:ThemeDate(
            primarySwatch:Colors.amber,
        ),
        home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
}
~~~

6. Widget을 갱신하는 didUpdateWidget() 함수     
부모 Widget이나 데이터가 변경되어 Widget을 갱신해야 할 때 호출한다.      
만약 iniState()에서 특정 이벤트에 의해 Widget이 변경되면 didUpdateWidget() 함수를 호출해 Widget을 갱신할 수 있다.      
initState() 함수는 Widget을 초기화할 때 한 번만 호출되므로 Widget이 변경되었을 때 호출하는 didUpadateWidget() 같은 함수가 필요하다.      
~~~dart
@override
void didUpdateWidget(Widget oldWidget){
    if(oldWidget.importantProperty != widget.importantProperty){
        _init();
    }
}
~~~

7. Widget의 State를 갱신하는 setState() 함수     
setState() 함수를 이용하면 데이터가 변경되었다는 것을 알려주고 변경된 데이터를 이용해 화면의 UI를 변경할 수 있도록 한다.      
~~~dart
void updateProfile(String name){
    setState(() => this.name = name);
}
~~~

8. Widget의 State 관리를 중지하는 deactivate() 함수    
deactivate() 함수는 State 객체가 플러터의 구성 트리로부터 제거될 때 호출된다.      
다만, State 객체가 제거됐다고 해서 해당 메모리까지 지워지지는 않는다.     
deactivate() 함수가 호출되더라고 dispose() 함수를 호출하기 전까지는 State 객체를 재사용할 수 있다.       

9. Widget의 State 관리를 완전히 끝내는 dispose() 함수    
State 객체를 영구적으로 소멸할 때 호출한다.      
이 함수를 호출한다는 것은 이제 해당 Widget을 종료한다는 뜻이다.     
Widget을 소멸할 때 꼭 호출해야 하는 함수라면 dispose() 함수 안에서 호출해야 한다.      
만약 deactivate() 함수 호출로 State 객체를 트리에서 제거한 후에 같은 State를 다시 다른 트리에 재사용할 경우 dispose() 함수가 호출되지 않을 수 있다.     

10. Widget을 화면에서 제거하면 mounted == false     
State 객체가 소멸하면 마지막으로 mounted 속성이 false로 되면서 생명주기가 끝난다.     
mounted 속성이 false가 되었다는 것은 이 State는 재사용할 수 없다는 의미다.     


|호출 순서|생명주기|내용|
|:---:|:---:|:---:|
|1|createState()|처음 스테이트풀을 시작할 때 호출|
|2|mounted==true|crateState() 함수가 호출되면 mounted는 true|
|3|initState()|State에서 제일 먼저 실행되는 함수. State 생성 후 한 번만 호출|
|4|didChangeDependencies()|initState() 호출 후에 호출되는 함수|
|5|build()|Widget을 렌더링하는 함수. Widget을 반환|
|6|didUpdateWidget()|Widget을 변경해야 할 때 호출하는 함수|
|7|setState()|데이터가 변경되었음을 알리는 함수. 변경된 데이터에 UI를 적용하기 위해 필요|
|8|deactivate()|State가 제거될 때 호출|
|9|dispose()|State가 완전히 제거되었을 때 호출|
|10|mounted == false|모든 프로세스가 종료된 후 mounted가 false로 됨|
