## dart 언어의 9가지 특징

1. dart는 main() 함수로 시작한다.
2. 다트는 어디에서나 변수를 선언하고 사용할 수 있다.
3. 다트에서는 모든 변수가 객체이다. 그리고 모든 객체는 Object 클래스를 상속받는다.
4. 다트는 자료형이 엄격한 언어이다.
5. 다트는 제네릭 타입을 이용해 개발할 수 있다.
6. 다트는 public, protected 같은 키워드가 없다.
   외부로 노출하고 싶지 않다면 변수 이름 앞에 언더스코어(_)를 이용해 표시할 수 있다.
8. 변수나 함수의 시작은 언더스코어 또는 문자열로 시작한다.
9. 다트는 삼항 연산자를 사용할 수 있다.
10. Null Safety를 지원한다.

~~~dart
var visibility = isPublic ? 'public' : 'private';

String playerName(String name) => name ?? 'Guest';
//name이 null이면 Guest를 반환한다.
~~~

## Null Safety

변수를 선언할 때 사용하는 것으로, 자료형 다음에 ?를 붙이면 Null이 가능하고 붙이지 않으면 Null이 불가능하다.       
그리고 식 다음에 !를 붙이면 Null이 아님을 직접 표시한다.
~~~dart
int? couldReturnNullButDoesnt() =? -3;
//null을 넣을 수 있음

void main(){
    int? couldBeNullButInsnt = 1;   //null로 변경 가능
    List<int?> listThatCouldHoldNulls = [2, null, 4];   //null 포함 가능
    List<int>? nullsList;   //List 자체가 null일 수 있음
    int a = couldBeNullButInst; //null을 넣으면 오류
    int b = listThatCouldHoldNulls.first;   //?가 없으므로 오류
    int b = listThatCouldHoldNulls.first!;   //null이 아님을 직접 표시
    int c = couldBeNullButDoesnt().abs();   //null일 수도 있으므로 abs()에서 오류
    int c = couldBeNullButDoesnt()!.abs();  //null이 아님을 직접 표시
}
~~~

## 비동기 처리 방식

비동기asynchronous란 언제 끝날지 모르는 작업을 기다리지 않고 다음 작업을 처리하게 하는 것을 의미한다.

### 작동 방식
다트는 async와 await 키워드를 이용해 비동기 처리를 구현한다.

1. 함수 이름 뒤, 본문이 시작하는 중괄호 앞에 async 키워드를 붙여 비동기로 만든다.
2. 비동기 함수 안에서 언제 끝날지 모르는 작업 앞에 await 키워드를 붙인다.
3. 2번 작업을 마친 결과를 받기 위해 비동기 함수 이름 앞에 Future(값이 여러개면 Stream) 클래스를 지정한다.

~~~dart
void main(){
    checkVersion();
    print('end process');
}
Future checkVersion() async{
    var version = await lookUpVersion();
    print(version);
}
int lookUpVersion(){
    return 12;
}
~~~

비동기 함수가 반환하는 값을 처리하려면 then() 함수를 이용한다.

~~~dart
void main() async{
    await getVersionName().then((value) => {
        print(value);
    });
    print('end process');
}
Future<String> getVersionName() async{
    var versionName = await lookUpVersionName();
    return versionName;
}
String lookUpVersionName(){
    return 'Android Q';
}
~~~

### 다트와 스레드

다트는 하나의 스레드thread로 동작하는 프로그래밍 언어이다.

~~~dart
void main(){
    printOne();
    printTwo();
    printThree();
}
void printOne(){
    print('One');
}
void printThree(){
    print('Three');
}
void printTwo() async {
    Future.delayed(Duration(seconds:1), (){
        print('Future');
    });
    //await Future.delayed(Duration(seconds:2){
    //    print('Future');
    //});
    print('Two');
}
//One Two Three Future
////One Three Future Two
~~~

Future.delayed() 함수는 Duration 기간 동안 기다린 후에 진행하라는 의미이다.
await 키워드를 붙인다면 이후 코드의 실행이 멈추고      
main() 함수의 나머지 코드를 모두 실행하고 await가 붙은 코드부터 차례대로 실행한다.

## JSON 데이터

JSON을 사용하려면 소스에 convert라는 라이브러리를 포함해야 한다.

~~~dart
import 'dart:convert';

void main(){
    var jsonString = '''
        [
            {"score":40},
            {"score":80}
        ]
    ''';
    var scores = jsonDecode(jsonString);
    print(scores is List);  //true
    var firstScore = scores[0];
    print(firstScore is Map);   //true
    print(fistScore['score'] == 40) //true
}
~~~

jsonDecode() 함수는 JSON 형태의 데이터를 dynamic 형식의 리스트로 변환해서 반환해 준다. 

~~~dart
import 'dart:convert';

void main(){
    var scores = [
        {'score':40},
        {'score':80},
        {'score':100, 'overtime':true, 'special_guest':null}
    ];

    var jsonText = jsonEncode(scores);
    print(jsonText == 
        '[{"score":40},{"score":80},'
        '{"score":100, "overTime":true,'
        '"special_guest":null}]');  //true
}
~~~

### 스트림 통신

데이터를 순서대로 주고받을 때 스트림stream을 이용한다.

~~~dart
import 'dart:async';

Future<int> sumStream(Stream<int> stream) async{
    var sum = 0;
    await for (var value in stream){
        print('sumStream: $value');
        sum += value;
    }
    return sum;
}
Stream<int> countStream(int to) async*{
    for(int i =1; i<=to; i++){
        print('countStream: $i');
        yield i;
    }
}
main() async{
    var stream = countStream(10);
    var sum = await sumStream(stream);
    print(sum);
}
~~~

위 countStream 함수는 async*와 yield 키워드를 이용해 비동기 함수로 만들었다.     
async* 명령어는 앞으로 yield를 이용해 지속적으로 데이터를 전달하겠다는 의미이다.      
위 코드에서 yield는 int형 i를 반환하는데,       
return은 한 번 반환하면 함수가 끝나지만 yield는 반환 후에도 계속 함수를 유지한다.      
then() 함수를 이용해 스트림 코드를 작성할 수도 있다.     
스트림을 통해 데이터를 사용하면 데이터는 사라지기 때문에 하나씩 실행해야 된다.

~~~dart
void main(){
    var stream = Stream.fromIterable([1,2,3,4,5]);

    stream.first.then((value) => print('$value'));  //1
    stream.last.then((value) => print('$value'));   //5
    stream.isEmpty.then((value) => print('$value'));//false
    stream.length.then((value) => print('$value')); //5
}
~~~

