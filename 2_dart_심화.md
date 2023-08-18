## Class 만들기

~~~dart
class Integer{
    late int _value;
    //late:초기화 지연 표현

    //생성자constructor
    Integer([int givenValue = 0]){
        _value = givenValue;
    }
    //초기화 리스트 Initialization List
    //Integer([int givenValue = 0]) : _value = givenValue{}
    //Integer([int givenValue = 0]) : _value = givenValue;
    int get(){
        return _value;
    }
    void set(int givenValue){
        _value = givenValue;
    }
    String get asString => "$_value";
    //리턴 타입과 함수 이름 사이에 get,set이 들어가는 문법을 getter,setter문법이라고 한다.
    set value(int givenValue) => _value = givenValue;

    Integer operator +(Integer givenValue){
        return Integer(_value+givenValue.get());
    }
}
//유전의 법칙 sub-class, inheritance
//기존에 존재하는 Base 클래스 A에 추가적인 데이터와 메서드를 넣어서 새로운 Derived 클래스 B를 만드는 것이다.
//이때 클래스 B는 클래스 A의 모든 데이터와 메서드를 포함하며 여기에 개발자가 추가하고 싶은 데이터와 메서드만 새롭게 작성하면 된다.
class TimemachineInteger extends Integer{
    List<int> _timemachine = [];
    TimemachineInteger([int givenValue = 0]){
        _value = givenValue;
    }

    //기존에 정의한 것을 사용하지 않고, 새로운 것으로 덮어쓴다
    @override
    void set(int givenValue){
        _timemachine.add(_value);
        super.set(givenValue);
    }
    List getOld(){
        return _timemachine;
    }
    @override
    String get asString => "$_value, previously $_timemachine";
}
void main(){
    //생성자와 객체 생성
    var tmpInteger1 = Integer();
    var tmpInteger2 = Integer(3);

    print("[01]${tmpInteger1._value} and ${tmpInteger2._value}");
    //인스턴스 변수 값에 접근
    print("[02] ${tmpInteger1.runtimeType}");//Integer
    //인스턴스 변수의 값을 읽는 Get유형 메서드
    print("[03] ${tmpInteger1.get()} and ${tmpInteger2.get()}");
    //인스턴스 변수의 값을 쓰는 Set 유형 메서드
    tmpInteger1.set(9);
    //getter문법
    print("[04] ${tmpInteger1.asString}");
    //setter 문법
    tempInteger2.value = 2;

    //연산자 오버로딩 operator overloading
    var tmpInteger3 = tmpInteger1 + tmpInteger2;

    var tmpTmInteger1 = TimemachineInteger();
    var tmpTmInteger2 = TimemachineInteger(3);

    print("[05] ${tmpTmInteger1._value} and ${tmpTmInteger2._value}");
    print("[06] ${tmpTmInteger1.runtimeType}");//TimemachineInteger

    tmpTmInteger1.set(9);
    tmpTmInteger1.value = 2;
    tmpTmInteger1.set(10);
    print("[07]${tmpTmInteger1.getold()}");
    print("[07]${tmpTmInteger1.asString}");
}
~~~

## Class 만들기2

~~~dart
class Integer{
    late int _value;

    Integer([int givenValue = 0]) : _value = givenValue;

    int get(){
        return _value;
    } 
    void set(int givenValue){
        _value = givenValue;
    }
    String get asString => "$_value";

    set value(int givenValue) => _value = givenValue;

    Integer operator +(Integer givenValue){
        return Integer(_value+givenValue.get());
    }
}

//사실상 클래스인데 스스로 객체가 되어 사용되기 보다는 다른 클래스의 부속품으로 사용되는 클래스
//on은 보통 작성하지 않음
mixin ActivationFlag on TimemachineInteger {
    bool _flag = true;

    bool get activated => _flag;
    set activated(bool givenFlag) => (_flag = givenFlag);
}

class TimemachineInteger extends Integer with ActivationFlag{
    List<int> _timemachine = [];

    TimemachineInteger([int givenValue = 0]) : _value = givenValue;

    @override
    void set(int givenValue){
        if(activated == true){
            _timemachine.add(_value);
            print("set($givenValue) called => _timemachine is $_timemachine");
        }else{
            print("set($givenValue) called => current $_value is ignored");
        }
        super.set(givenValue);
    }

    List getOld(){
        return _timemachine;
    }
    @override
    String get asString => "$_value, previously $_timemachine";
}
void main(){
    var tmpTmInteger = TimeMachineInteger(0);
    tmpTmInteger.set(2);
    tmpTmInteger.set(4);
    tmpTmInteger.activated = false;
    tmpTmInteger.set(6);
    tmpTmInteger.activated = true;
    tmpTmInteger.set(8);
    print("${tmpTmInteger.asString}");
}
~~~

## Class 만들기3

~~~dart
//추상클래스 - 객체를 만들 수 없는 클래스
abstract class Rectangle{
    int cx = 0, cy = 0;
    void draw();
}
class Circle implements Rectangle{
    @override
    int cx = 0, cy =0;

    late int radius

    @override
    void draw(){
        print("> Circle.draw(): center($cx, $cy) with r[$radius]");
    }
    Circle([int givenRadius = 1]) : raidus = givenRadius;
}
class Square implements Rectangle{
    @override
    int cx = 0, cy = 0;

    late int height, width;

    @override
    void draw(){
        print("> Square.draw(): center($cx,$cy) with h[$height] and w[$width]");
    }

    Square([int givenHeight = 1, int givenWidth = 1])
        : height = givenHeight, width = givenWidth;
}
void main(){
    //var rc = Ractangle();
    var ci = Circle(10);
    var sq = Square(10,10);
    ci.draw();
    sq.draw();
}
~~~

## Class 만들기4

~~~dart
class Point_int{
    late int x, y;
    Point_int(int givenX, int givenY) : x = givenX, y = givenY;
}

class Point_double{
    late double x,y;
    Point_double(double givenX, double givenY) : x=givenX,y=givenY;
}

class Point_String{
    late String x,y;
    Point_String(String givenX, String givenY) : x=givenX,y=givenY;
}
//Generic은 클래스가 다루는 데이터 타입이 많은 경우에 개발을 편안하게 해 주는 목적으로 제공되는 기술이다.
class Point1<T>{
    late T x,y;
    Point(T givenX, T givenY) : x = givenX, y = givenY{
        objCount++;
    }
    static int objCount = 0;
    get cnt => objCount;
}

class Point2<T,Y> {
    late T x;
    late Y y;
    Point(T givenX, Y givenY) : x = givenX, y = givenY{
        objCount++;
    }
    static int objCount = 0;
    get cnt => objCount;
}
void main(){
    Point_int iPoint = Point_int(10,10);
    Point_double dPoint = Point_double(10.1,10.1);
    Point_String sPoint = Point_string('10.2','10.2');

    Point<int> ip1 = Point<int>(10,10);
    Point<int> ip2 = Point<int>(20,20);

    Point1<double> dp = Point<double>(10.1,10.1);
    Point1<String> sp = Point<String>('10.2','10.2');

    Point2<int, double> ip1 = Point<int, double>(10,10.1);
    Point2<int,int> ip2 = Point<int,int>(20,20);
    Point2<double,String> dp = Point<double,String>(10.1,'10.1');
    Point2<String,String> sp = Point<String,String>('10.2','10.2');
}
~~~


## 비동기 입출력 기능 활용

~~~dart
void main(){
    print("main(): started");
    Future.delayed(
        Duration(seconds: 3), () => print("main(): Hello, World! @ 3 seconds")
    );
    print("main(): completed");
}
~~~
비동기 작업의 결과를 기다려야 하는 경우, await 구문을 사용하여 비동기 작업을 실행한다.      
await 문법을 사용하는 함수는 반드시 async 구문을 적용한다.

~~~dart
void main() async{
    print("main(): started");
    await Future.delayed(
        Duration(seconds: 3), () => print("main(): Hello, World! @ 3 seconds")
    );
    print("main(): completed");
}

// 아래와 같은 방식으로도 사용 가능하다.
void main() async{
    print("main(): started");
    await Future.delayed(Duration(seconds: 3));
    print("main(): Hello, World! @ 3 seconds");
    print("main(): completed");
}
~~~
Dart 언어로 개발하다 보면, Future나 Stream으로 명시된 클래스의 객체를 리턴하는 경우가 있다.           
이들은 비동기 작업을 지원하기 위한 목적으로 만들어져 await는 Future 혹은 Stream을 리턴하는 함수나 메서드에 쓰인다.

~~~dart
void doBackgroundJob(int jobTime){
    if(jobTime > 0){
        print("doBackgroundJob(): $jobTime sec remained");
        Future.delayed(Duration(seconds:1), () =>
            doBackgroundJob(jobTime -1));
    }else{
        print("doBackgroundJob(): finished");
    }
}
void main(){
    print("main(): started");
    doBackgroundJob(5);
    print("main(): completed");
}
~~~
~~~dart
class ActivationFlag{
    late bool _flag;

    ActivationFlag(bool givenFlag) : _flag = givenFlag;
    bool get activated => _flag;
    set activated(bool givenFlag) => (_flag = givenFlag);
}

void doBackgroundJob(int jobTime, var jobEnd){
    if(jobTime > 0){
        print("doBackgroundJob(): $jobTime sec remained");
        Future.delayed(Duration(seconds:1), () =>
            doBackgroundJob(jobTime -1, jobEnd));
    }else{
        print("doBackgroundJob(): finished");
        jobEnd.activated = true;
    }
}

void main() async{
    var bgJobFinished = ActivationFlag(false);
    print("main() : started");
    doBackgroundJob(5, bgJobFinished);
    while(bgJobFinished.activated == false){
        await Future.delayed(Duration(seconds:1));
    }
    print("main(): completed");
}
~~~

~~~dart
//async 함수가 리턴 값을 갖는 경우, 리턴 값의 타입은 반드시 Future<>로 정의한다.
Future<String> serveCustomer() async{
    print("serveCustomer(): waiting for order");
    var customerOrder = await simulateCustomerOrder();
    print("serveCustomer(): order '$customerOrder' received");
    return customerOrder;
}

Future<String> simulateCustomerOrder(){
    return Future.delayed(Duration(seconds:2), () => 'Ice Coffee');
}

void main() async{
    print("main(): started");

    var customerOrder = serveCustomer();
    print("main(): serve '$customerOrder'");
    print("main(): completed");
}
~~~

serveCustomer() 함수도 비동기 함수이기에, 결과가 제대로 리턴될 때까지 함수를 호출한 쪽에서 기다려 주어야 한다.       
하지만 그렇게 하지 않고 함수 호출로 작업 수행을 요청하고 결과를 받기도 전에 main 함수의 다음 작업을 진행했다.     
따라서, 앞서 비동기 함수의 결과 값을 기다릴 때와 동일하게 serveCustomer() 함수를 호출하는 main 함수에서도 await/asyn 구문을 추가로 반영하면 된다.

~~~dart
Future<String> serveCustomer() async{
    print("serveCustomer(): waiting for order");
    var customerOrder = await simulateCustomerOrder();
    print("serveCustomer(): order '$customerOrder' received");
    return customerOrder;
}
Future<String> simulateCustomerOrder(){
    return Future.delayed(Duration(seconds: 2), () => 'Ice Coffee');
}
void main() async{
    print("main(): started");

    var customerOrder = await serveCustomer();
    print("main(): serve '$customerOrder'");
    print("main(): completed");
}
~~~

#### 정리

~~~dart
class ActivationFlag{
    late bool _flag;

    ActivationFlag(bool givenFlag) : _flag = givenFlag;
    bool get activated => _flag;
    set activated(bool givenFlag) => (_flag = givenFlag);
}

void doBackgroundJob(int jobTime, var jobEnd){
    if(jobTime > 0){
        print("doBackgroundJob(): $jobTime sec remained");
        Future.delayed(
            Duration(seconds:1), () => doBackgroundJob(jobTime-1, jobEnd)
        );
    }else{
        print("doBackgroundJob(): finished");
        jobEnd.activated = true;
    }
}

Future<String> serveCustomer() async{
    print("serveCustomer(): waiting for order");
    var customerOrder = await simulateCustomerOrder();
    print("serveCustomer(): order '$customerOrder' received");
    return Future.value(customerOrder);
}

Future<String> simulateCustomerOrder(){
    return Future.delayed(Duration(seconds:2), () => 'Ice Coffee');
}

void main() async{
    var bgJobFinished = ActivationFlag(false);

    print("main(): started");
    doBackgroundJob(5, bgJobFinished);

    var customerOrder = await serveCustomer();
    print("main(): serve '$customerOrder'");

    while(bgJobFinished.activated == false){
        await(Future.delayed(Duration(seconds:1)));
    }
    print("main(): completed");
}
~~~

## 예외 상황 처리를 통한 프로그램 안정성 강화


#### try/catch 구문
~~~dart
void main(){
    int iTemp1 = 1;
    int iTemp2 = 0;
    int iResult1 = 0;

    try{
        iResult1 = iTemp1 ~/ iTemp2;
    }catch(error){
        print(error);
    }
    print(iResult1);
}
~~~

#### on ErrorType 구문

> on xxx {}

xxx 에러가 발생하면 {}안의 작업을 수행한다.

~~~dart
void main(){
    int iTemp1 = 1;
    int iTemp2 = 0;
    int iResult1 = 0;

    try{
        iResult1 = iTemp1~/iTemp2;
    }on UnsupportedError{
        iTemp2 = 1;
        iResult1 = iTemp1 ~/ iTemp2;
    }
    print(iResult1);
}
~~~

#### 예측 불가능한 에러 처리

~~~dart
void main(){
    int iTemp1 = 1, iTemp2 = 0, iResult1 = 0;

    try{
        iResult1 = iTemp1 ~/ iTemp2;
    } on UnsupportedError{
        iTemp2 = 1;
        iResult1 = iTemp1 ~/ iTemp2;
    } catch(error){
        print(error);
    } finally{
        print(iResult1);
    }
}
~~~

#### 호출한 함수에서 발생한 에러 처리

~~~dart
void main(){
    int iTemp1 = 1, iTemp2 = 0, iResult1 = 0;

    try{
        iResult1 = calcSomething(iTemp1,iTemp2);
    } on UnsupportedError{
        iTemp2 = 1;
        iResult1 = iTemp1 ~/ iTemp2;
    } catch(error){
        print(error);
    } finally{
        print(iResult1);
    }
}
int calcSomething(int iLeft, int iRight){
    int iResult1 = 0;

    try{
        iResult1 = iLeft ~/ iRight;
    } catch(error){
        rethrow;
    }
    return iResult;
}
~~~

#### 프로그램 자체적으로 에러 생성

~~~dart
void main(){
    int iTemp1 = 1, iTemp2 = 0, iResult1 = 0;

    try{
        iResult1 = calcSomething(iTemp1,iTemp2);
    } on NegativeDivisiorException{
        iTemp2 = 1;
        print("NegativeDivisiorException");
        iResult1 = iTemp1 ~/ iTemp2;
    } catch(error){
        print("un-known error");
    } finally{
        print(iResult1);
    }
}

int calcSomething(int iLeft, int iRight){
    int iResult1 = 0;

    if(iRight == 0){
        throw NegativeDivisorException():
    }else{
        iResult = iLeft ~/ iRight;
    }
    return iResult;
}
class NegativeDivisorException implements Exception{
    @override
    String toString(){
        return "Zero divisor is not allowed";
    }
}
~~~

## 키보드 입력과 화면 출력

Dart 언어에서 키보드를 통하 입력을 하려면 'dart.io' 라는 Dart 언어의 입출력 특화 기능을 사용해야 한다.

~~~dart
import 'dart:io';

void main(){
    //출력 후 줄바꿈 안 됨
    stdout.write("[1] : ");
    var tmpInput = stdin.readLineSync();
    //출력 후 줄바꿈 됨
    stdout.writeIn("$tmpInput");
    while(tmpInput = != "exit"){
        stdout.write("[2] : ('exit' to stop)");
        tmpInput = stdin.readLineSync();
        stdout.writeIn("[2]$tmpInput");
    }
    stdout.write("[3] : (Single number)");
    tmpInput = stdin.readLineSync();
    stdout.writeIn("[3] : $tmpInput");

    var iList = <int>[];
    stdout.write("[4] : (Single number)");
    tmpInput = stdin.readLineSync();
    // ! : null 값을 가질 수 있는 변수에 강제로 null이 될 리가 없으니 그냥 처리하라는 의미
    iList.add(int.parse(tmpInput!));
    stdout.write("[4] Single number again : ");
    tmpInput = stdin.readLineSync();
    // ?? : tmpInput이 null이면 '1'을 쓰고 아니면 tmpInput을 쓰라는 의미
    iList.add(int.parse(tmpInput ?? '1'));
    stdout.writeIn("[4] ${iList[0]} x ${iList[1]} = ${iList[0] * iList[1]}");

    var sList = <String>[];
    stdout.write("[5] Two numbers(ex.3, 3) : ");
    tmpInput = stdin.readLineSync();
    sList = tmpInput!.split(',');
    var index = 0;
    for(var item in sList){
        iList[index] = int.parse(sList[index]);
        index++;
    }
    stdout.writeIn("[5] ${iList[0]} x ${iList[1]} = ${iList[0] * iList[1]}");
}
~~~

## 파일 입출력 기능 활용

~~~dart
import 'dart:io';
import 'dart:convert';

Future<String> readFileToString(String filename) async{
    var file = File(filename);
    String fileContent = await file.readAsString();
    return fileContent;
}

Future<List<String>> readFileToList(String filename) async{
    Stream<String> lines = File(filename)
        .openRead()
        .transform(utf8.decoder)
        .transform(LineSplitter());
    
    try{
        List<String> sList = [];
        await for(var line in lines){
            sList.add(line);
        }
        return sList;
    }catch(error){
        throw (error);
    }
}

void main() async{
    try{
        String fileContent = await readFileToString('src.txt');
        print(fileContent);
    }catch(error){
        print(error);
    }

    try{
        List<String> fileContent = await readFileToList('src.txt');
        for(var fileLine in fileContent){
            print(fileLine);
        }
    }catch(error){
        print(error);
    }

    List<String> fileContent = await readFileToList('src.txt');
    var sList = [];
    var ivar1 = 0;
    var ivar2 = 0;
    var count = 0;
    var dstSink = File('dst.txt').openWrite();

    dstSink.write('${DateTime.now()}');
    for(var fileLine in fileContent){
        sList = fileLine.split(',');
        ivar1 = int.parse(sList[0]);
        ivar2 = int.parse(sList[1]);
        print("ivar1, ivar2 || $ivar1, $ivar2");
        dstSink.write("ivar1, ivar2 || $ivar1, $ivar2\n");
        count++;
    }
    dstSink.write("$count");
    //파일에 대한 데이터 읽기와 쓰기 작업을 마쳤다면 반드시 파일을 닫아야 한다.
    dstSink.close();
}
~~~

## 표준 라이브러리 활용

~~~dart
import 'dart:io';
import 'dart:async';
import 'dart:math';

void main(){
    //dart:core
    List<String> fruits = ['Apple is Red', '  Banana  ', 'Mango'];
    print("${fruits.contains('Mango')}");
    print("${fruits[0].startsWith('Apple')}");
    print("${fruits[0].endsWith('Red')}");
    print("${fruits[0].indexOf('Red')}");
    print("${fruits[1].trim()}");

    //dart:io - Platform Class
    //Native환경에서 동작하는 경우에 필요한 기능을 Platform 클래스가 제공한다
    //Native 환경은 컴퓨터의 운영체제 위에서 프로그램이 바로 실행되는 것을 의미한다.
    //Platform 클래스를 사용하므로 이 소스 코드는 CLI 환경에서 수행되어야 한다.
    String os = Platform.operatingSystem;
    String path = platform.script.toFilePath();
    print("$os");
    print("$path");

    //dart:math - Math Library
    print("${max(2,4)}");
    print("${min(2,4)}");
    print("{$e}");
    print("{$pi}");

    //dart:async & dart:core
    //DateTime Class
    var t1 = DateTime.now();
    //Future Class
    //wait() - 여러 Future 작업들이 완료될 때까지 기다렸다가 결과를 수집하는 메서드
    //reduce() - 리스트 안의 값들을 코드를 활용하여 하나의 값으로 변환하는 메서드
    Future.wait([async1(), async2(), async3()]).then((List<int> nums){
        var t2 = DateTime.now();
        var sum = nums.reduce((curr, next) => curr + next);
        print("$sum ${t2.difference(t1)}");
    }).catchError(print);

    Future<int> async1() async{
        await Future.delayed(Duration(seconds:1));
        return 10;
    }
    Future<int> async2() async{
        await Future.delayed(Duration(seconds:1));
        await Future.delayed(Duration(seconds:1));
        return 20;
    }
    Future<int> async3() async{
        await Future.delayed(Duration(seconds:1));
        await Future.delayed(Duration(seconds:1));
        await Future.delayed(Duration(seconds:1));
        return 30;
    }
}
~~~
