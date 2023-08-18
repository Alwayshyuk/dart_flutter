# Dart 시작

## Hello, World

~~~dart
void main(){
    print('Hello, World');
}
~~~

모든 Dart 프로그램은 반드시 하나의 main 함수를 포함하고 있어야 한다. main 함수는 가장 먼저 실행이 되는 작업이다. 또한, main함수를 종료하는 행위는 프로그램을 종료하는 행위이다.      

## 기초적인 숫자와 문자 다루기

~~~dart
void main(){
    int intTemp = 1;
    double dbTemp = 2.2;
    num numIntTemp = 3;
    num numDbTemp = 4.4;
    String strTempp = "Hello";
    print(intTemp);
    print(dbTemp);
    print(numIntTemp);
    print(numDbTemp);
    print(strTempp);

    var varInt = 1;
    var varDouble = 2.2;
    var varString = "Hello";
    print("$varInt $varDouble $varString");
    //1 2.2 Hello
    print("RESULT [ $varInt $varDouble '$varString' ] ");
    //RESULT [ 1 2.2 'Hello']

    dynamic dynInt = 1;
    dynamic dynDouble = 2.2;
    dynamic dynString = "Hello";
    print("$dynInt $dynDouble $dynString");
    //1 2.2 Hello

    const double cMathPi = 3.141592;
    const cChangeRate = 1.3;
    print("$cMathPi $cChangeRate");
    //3.141592 1.3

    final String fFirstFruit = "Apple";
    final fSecondFruit = "Mango";
    final String fThirdFruit;
    fThirdFruit = "Banana";
    print("$fFirstFruit $fSecondFruit $fThirdFruit");
    //Apple Mango Banana
}
~~~

작업 중에 값이 바뀌는 경우는 변수(variable), 바뀌지 않은 채 유지하는 경우 상수(constant)라고 부른다.           
|문법|의미|주요특징|
|:---:|:---:|:---:|
|int|정수|-9,007,199,254,740,992 ~ 9,007,199,254,740,992|
|double|실수|최대 1.7976931348623157e+308|
|num|숫자|정수 혹은 실수|
|String|문자열|글자, 단어 혹은 문장|
|var|변수|정수, 실수, 문자열 등을 저장하며, 값의 변경 가능(한번 값을 저장하고 나면, 같은 타입의 값을 저장해야 함)|
|dynamic|변수|정수,실수,문자열 등을 저장하며 값의 변경 가능(저장하는 값의 타입에 제한 없음)|
|constant|상수|처음 만드는 시점에 값을 설정하며, 값의 변경 불가능|

> final 문법을 통해 만든 상수는, 초기에 어떤 값을 가지지 않는 상태로 만들어지고 나서 나중에 상수에 값을 저장할 수 있다.

## 문자 다루기
~~~dart
void main(){
    var str1 = 'Single Quotes';
    var str2 = "Double Quotes";
    var str3 = '"Double Quotes" in Single Quotes';
    var str4 = "'Single Quotes' in Double Quotes";
    var str5 = '\'Escape Delimeter\' in Single Quotes';
    print("[1] \n$str1 \n$str2 \n$str3 \n$str4 \n$str5");

    var longStr1 = 'My'
        'name'
        'is'
        'Apple';
        print(longStr1);
    var longStr2 = '''
        My name is Apple
        Dart is lovely
        ''';
        print(longStr2);
    var longStr3 = "My"
        "name"
        "is"
        "Mango";
        print(longStr3);
    var longStr4 = """
        My name is Mango
        Flutter is lovely
        """;
    print(longStr4);

    var varInt = 1;
    const double cMathPi = 3.141592;
    String tempStr1, tempStr2;
    tempStr1 = varInt.toString();
    tempStr2 = cMathPi.toStringAsFixed(2);
    print("$tempStr1 $tempStr2");
    //1 3.14
}
~~~

## 조건문

~~~dart
void main() {
  var number1 = 1;
  var number2 = 1;
  var number3 = 2;

  print("[ 1] $number1 == $number2 : ${number1 == number2}");
  print("[ 2] $number1 == $number3 : ${number1 == number3}");
  print("[ 3] $number1 != $number2 : ${number1 != number2}");
  print("[ 4] $number1 != $number3 : ${number1 != number3}");
  print("[ 5] $number1 >= $number2 : ${number1 >= number2}");
  print("[ 6] $number1 <= $number2 : ${number1 <= number2}");
  print("[ 7] $number1 >  $number2 : ${number1 > number2}");
  print("[ 8] $number1 <  $number2 : ${number1 < number2}");
  print("[ 9] $number1 >= $number3 : ${number1 >= number3}");
  print("[10] $number1 <= $number3 : ${number1 <= number3}");
  print("[11] $number1 >  $number3 : ${number1 > number3}");
  print("[12] $number1 <  $number3 : ${number1 < number3}");

  var flag1 = ((number1 == number2) || (number1 == number3));
  bool flag2 = ((number1 == number2) && (number1 == number3));

  print("[13] ($number1 == $number2) OR  ($number1 == $number3) : $flag1");
  print("[14] ($number1 == $number2) AND ($number1 == $number3) : $flag2");
  print("[15] NOT ($number1 == $number2) : ${!(number1 == number2)}");

  if (number1 == number2) {
    print("[16] (a) number1[$number1] equal to number2[$number2].");
  } else if (number1 == number3) {
    print("[16] (b) number1[$number1] equal to number3[$number3].");
  } else {
    print("[16] (c) number1[$number1] not equal to number2[$number2] and number3[$number3].");
  }

  var switchStatus = 'OFF';

  switch (switchStatus) {
    case 'OFF':
      print("[17] (a) switch is OFF.");
      break;
    case 'ON':
      print("[17] (b) switch is ON.");
      break;
    default:
      print("[17] (c) switch status is not correct.");
      break;
  }

  switch (switchStatus) {
    case 'off':
    case 'OFF':
      print("[18] (a) switch is OFF.");
      break;
    case 'on':
    case 'ON':
      print("[18] (b) switch is ON.");
      break;
    default:
      print("[18] (c) switch status is not correct.");
      break;
  }

  var programTermination = 'NORMAL';

  assert(programTermination == 'NORMAL');
  print("[19] program terminated in normal.");
  //[19] program terminated in normal.
}
~~~

assert는 프로그램이 오류 상태여서 수행을 중단해야 하는지를 판단하는 경우에 주로 사용한다.       
프로그램에서 예상하지 못한 상황을 만나는 경우에 대한 처리를 프로그래밍 하는 문법이다.

## 반복문

~~~dart
void main(){
    print("[1] 'for' statement.\n");
    var number = 1;
    var count = 1;
    for(count = 1; count <=3; count++){
        print("$number x $count = ${number * count}");
    }

    print("\n[2] 'while' statement.\n");
    number = 1;
    count = 1;
    while(count<=3){
        print("$number x $count = ${number*count}");
        count++;
    }

    print("\n[3] 'do-while' statement.\n");
    number = 1;
    count = 1;
    do{
        print("$number x $count = ${number*count}");
        count++;
    }while(count<=3);
    //while구문과 for구문은 조건에 따라서 반복 작업이 한번도 실행되지 않을 수 있으나 do-while 구문은 반복 작업이 최소한 한번은 수행된다.

    print("\n[4] nested loop statement.\n");
    for(count = number = 1; number<=3; number++){
        while(count<=3){
            print("$number x $count = ${number*count}");
            count++;
        }
        count = 1;
    }

    print("\n[5] nested conditional statement.\n");
    for(count = number = 1; number<=3; number++){
        if((number % 9) == 1){
            while(count<=3){
                print("$number x $count = ${number*count}");
                count++;
            }
            count = 1;
        }
    }

    print("\n[6] 'continue' and 'break' statement.\n");
    for(count = number = 1; number<=9; number++){
        if(number>4){
            print("[6.1] break - $number");
            break;
        }else if((number % 9) != 1){
            print("[6.2] continue - $number");
            continue;
        } else {
            print("[6.3] calculate - $number");
            while(count <= 3){
                print("$number x $count = ${number * count}");
                count++;
            }
            count = 1;
        }
    }
}
~~~

## 함수를 이용한 반복 작업

~~~dart
// Type.1 Normal Function
int getMax(var argv1, var argv2) {
  if (argv1 >= argv2) {
    return argv1;
  } else {
    return argv2;
  }
}

// Type.2 Shorthand Syntax Function
int getSum(var argv1, var argv2) => argv1 + argv2;

// Type.3 Named Parameters
int getMaxNamed({var argv1, var argv2}) {
  if (argv1 >= argv2) {
    return argv1;
  } else {
    return argv2;
  }
}

// Type.4 Optional and Default Parameters in Normal Function
//개발자가 입력 파라미터를 주지 않으면 함수에서 미리 정한 초기값으로 설정한다는 표현인 대괄호를 사용
int getMaxDefault(var argv1, [var argv2 = 1]) {
  if (argv1 >= argv2) {
    return argv1;
  } else {
    return argv2;
  }
}

// Type.5 Optional and Default Parameters in Named Parameters
int getMaxNamedDefault({var argv1, var argv2 = 1}) {
  // [WARN] Protected code for local variables check
  // print(tmp1, tmp2, tmp3);

  if (argv1 >= argv2) {
    return argv1;
  } else {
    return argv2;
  }
}

void main() {
  var tmp1 = 3;
  var tmp2 = 2;
  var tmp3 = 0;

  tmp3 = getMax(tmp1, tmp2);
  print("getMax(3,2) ==> $tmp3");

  tmp3 = getSum(tmp1, tmp2);
  print("getSum(3,2) ==> $tmp3");

  tmp3 = getMaxNamed(argv1: tmp1, argv2: tmp2);
  print("getMaxNamed(argv1: 3, argv2: 2) ==> $tmp3");

  tmp3 = getMaxDefault(tmp1);
  print("getMaxDefault(3) ==> $tmp3");

  tmp3 = getMaxNamedDefault(argv1: tmp1);
  print("getMaxNamedDefault(argv1: 3) ==> $tmp3");

  // [WARN] Protected code for local variables check
  // print(argv1, argv2);
}
~~~

## Class를 이용한 객체지향 프로그래밍

~~~dart
void printMessage(var header, var message){
    print("[$header] $message")
}
void main(){
    int iInteger = 3;
    printMessage("1-1", iInteger.abs());
    printMessage("1-2", 4.abs());
    //정수를 입력해도 int로 자동 반영되어 메서드 적용이 가능하다.
    printMessage("1-3", iInteger is int);
    //1-3 true
    //is는 객체가 주어진 클래스 타입이면 true를 반환한다.
}
~~~
> http://api.dart.dev/

## List를 이용한 복수 데이터 처리

List 클래스는 복수의 값을 저장할 수 있으며, 개발자가 지정한 순서대로 일렬로 저장하는 데이터 타입이다. 저장되는 복수의 값은 동일하 데이터 타입일 수 있으며, 반대로 서로 다른 데이터 타입을 함께 섞어서 저장할 수도 있다.

~~~dart
void main(){
    List iList = [1,2,3,4,5];
    print("[1] iList is $iList");
    print("[2] ${iList.length}");
    //프로퍼티는 함수 형태를 갖지 않기에 입력 파라미터를 전달하는 소괄호가 없다.
    print("[3] ${iList.first}");
    print("[4] ${iList.last}");
    print("[5] ${iList.indexOf(3)}");
    iList.insert(2,99);
    iList.add(6);
    iList.addAll([7,8,9]);
    iList.sort((a,b) => a.compareTo(b));
    iList.clear();

    List mixedList = [1, 2.2, "Three"];
    print("[6] mixedList is $mixedList");
    print("[7] mixedList[0] is ${mixedList[0] is int}");
    print("[8] mixedList[1] is ${mixedList[1] is double}");
    print("[9] mixedList[2] is ${mixedList[2] is String}");

    var count = 0;
    for(var item in mixedList){
        print("[=>] mixedList[$count] is $item");
        count++;
    }

    List<int> intList = [1,2,3,4,5];
    print("[10] intList is List<int> ${intList is List<int>}");
    print("[11] mixedList is List<dynamic> ${mixedList is List<dynamic>}");
    //정수,실수, 문자열이 섞인 mixedList를 만들때 Dart 언어가 알아서 서로 다른 타입이 처리 가능한 List<dynamic>으로 만들었다.
}
~~~

## Set을 이용한 집합 데이터 처리

중복이 불가하고 순서가 의미가 없는 집합

~~~dart
void main(){
    Set setFill = {1,2};
    Set setEmpty = {};

    setFill.add(3);
    
    setEmpty.addAll([3,4,5]);

    print("[1] 3 in setFill ? : ${setFill.contains(3)}");
    print("[2] Union of setFill and setEmpty : ${setFill.union(setEmpty)}");//합집합
    print("[3] Intersection of setFill and setEmpty : ${setFill.intersection(setEmpty)}");//교집합
    print("[4] Difference of setFill and setEmpty : ${setFill.difference(setEmpty)}");//차집합

    setFill.remove(3);

    Set<int> exSet1 = {1,2,3};
    var exSet2 = <int>();

    print("[5] Type of setFill : ${setFill.runtimeType}");//Set<dynamic>
    print("[6] Type of exSet1 : ${exSet1.runtimeType}");//Set<int>
    print("[7] Type of exSet2 : ${exSet2.runtimeType}");//Set<int>
}
~~~

## Map을 이용한 사전 데이터 처리

key와 value가 1:1 대응하는 형태로 중복된 key는 가질 수 없다.

~~~dart
void main(){
    Map dbFruit = {"A001":"Apple", "A002":"Mango"};
    var dbEmpty = <dynamic, dynamic>();
    dbFruit["A003"] = "Banana";
    dbEmpty.addAll(dbFruit);
    dbFruit["A003"] = "Orange";
    print("Key'A002' in dbFruit ? ${dbFruit.containsKey("A002")}");
    print("Value 'Apple' in dbFruit ? ${dbFruit.containsKey("Apple")}");
    dbFruit.remove("A002");
    dbEmpty.clear();
}
~~~

## Dart 언어 기능

#### Unicode
~~~dart
void printStar(var item){
  print("\u{2605} $item \u{2605}");
}
void printStarWithDefault(String item, [String mark = '\u{1F499}']){
  print("$mark $item $mark");
}
void printStarWithNull(String item, [String? mark]){
  if(mark == null){
    print("\u{2605} $item \u{2605}");
  }else{
    print("$mark $item $mark");
  }
}

//개발자가 스스로 데이터 타입을 만든다는 의미의 문법
enum Color{red, green, blue}

void main(){
  print('\u{AC00}');
  Map dicEmoji = {
    'A':'\u{0041}',
    'a':'\u{0061}',
    'clap':'\u{1f44f}',
    'smile':'\u{1F642}',
    'star':'\u{2605}'
  };
  print("$dicEmoji");
  //{A: A, a: a, clap: 👏, smile: 🙂, star: ★}


  //Cascade
  List iList = [];
  iList
    ..addAll([2,1])
    //iList.addAll([2,1])
    ..add(0)
    //iList.add(0)
    ..sort((a,b)=>a.compareTo(b));
    //iList.sort((a,b)=>a.compareTo(b))
    print("$iList");

  //forEach
  //다수의 element를 갖는 객체가 있을 때, 객체 내부의 전체 혹은 일부 element들에 대해서 동일한 작업을 수행하는 경우 사용하는 문법
  iList.forEach(printStar);

  //Nested Function(중첩 함수)
  //함수 안에서 함수를 정의하는 문법
  void printSmile(var item){
    print("\u{1F642} $item \u{1F642}");
  }
  iList.forEach(printSmile);

  //Conditional Expression(조건적 표현)
  // A ? B : C
  var result = dicEmoji.isEmpty ? "dicEmoji is empty":"dicEmoji is not empty";
  print(result);

  //Bitwise Operators(비트 처리 연산자)
  //데이터를 bit 단위로 처리하는 연산자
  int bitOne = 1;//00000001
  int bitTwo = 2;//00000010
  print("${bitOne << 1} ${bitTwo >> 1} ${bitOne | bitTwo} ${bitOne & bitTwo}");

  //Hexa-Demical Presentation(16진법 표현)
  //0x로 시작한 후, 코드를 뒤에 쓰면 된다.
  num var1 = 0x01;//1
  num var2 = 0xFF;//255

  //Exponential Presentation(지수 표현)
  //수학과 유사하게 알파벳 e를 사용하여 표시하면 된다.
  num varE = 1.1e2;

  //String-to-Number Conversion(문자열을 숫자로 변환)
  num varI = int.parse('1');
  num varD = double.parse('1.1');

  //Enumerator(나열형 데이터)
  //개발자가 새로운 데이터 타입을 만들 수 있도록 지원하는 문법
  //새로운 데이터 타입이 갖는 값을 몇 가지의 값들로 제한되며, 그 외의 값은 다루지 못한다.
  print(Color.values);
  Color chColor = Color.red;
  print(chColor);

  //Null-Safty(Null에 대한 프로그램 안정성 보장)
  int iTemp = 3;
  int iNonNullableInt;
  //print(iNonNullableInt);
  //iTemp = iNonNullableInt;
  int? iNullableInt;
  iNullableInt = null;
  print(iNullableInt);
  //print(iNullableInt.abs());

  //iNonNullableInt = null;
  
  print(iNullableInt.runtimeType);//Null
  //iTemp = iNullableInt;

  printStarWithDefault('null-safety');
  printStarWithNull('null-safety');

  iNullableInt = null;
  print(iNullableInt ?? iTemp);
  //iNullableInt가 null 이 아니라면 iNullableInt고 null이면 iTemp다.

  iNullableInt = 1;
  print(iNullableInt ?? iTemp);//1

  iNonNullableInt = (iNullableInt ?? iTemp);
  print(iNonNullableInt)

  iNullableInt = null;
  print(iNullableInt?.abs());
  // ?. 객체가 null인 상태인 경우 호출이 불가능한 메서드와 프로퍼티에 접근하는 것을 막는 문법

  var map = {'key', 'value'};
  //print(map['key'].length);
  print(map['key']!.length);
  //[]! 는 map['key']가 반드시 String일 것이라는 확신을 표현하는 코드

}
~~~