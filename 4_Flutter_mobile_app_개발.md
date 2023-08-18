## DartPad를 이용한 Hello World 프로그램 개발

~~~dart
//Material 안드로이드 운영체제 전용 그래픽 라이브러리
import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center(
      child: Text(
        'Hello, World11',
        //글자 방향 left to right
        textDirection: TextDirection.ltr,
        style: TextStyle(
          fontSize:32,
          color:Colors.white,
        ),
      ),
    ),
  );
}
~~~

## Hello World 프로그램 2

개발 과정에서 반드시 적용해야 하는 Stateless Widget과 MaterialApp을 알아본다.

~~~dart

import 'package:flutter/material.dart';

main() => runApp(MyApp());

class MyApp extends StatelessWidget{
    //StatelessWidget 클래스를 extends하는 경우에는 반드시 build() 메서드를 오버라이드 해야한다.
    //build()는 운영체제가 앱을 실행하면서 화면에 출력해야 하는 내용들을 준비하고, 화면에 나타나도록 하는 핵심 기능을 한다.
    @override
    Widget build(BuildContext context){
        return MaterialApp(
            title: 'App Title',
            home: Scaffold(
                appBar: AppBar(
                    title: const Text('AppBar Title'),
                ),
                body: const Center(
                    child: Text(
                        'Hello, World!',
                        textDirection: TextDirection.ltr,
                        style: TextStyle(
                            fontSize: 32,
                            color: Colors.black,
                        ),
                    ),
                ),
            ),
        );
    }
}
~~~

StatelessWidget과 StatefulWidget은 가장 대표적인 위젯이다.     
프로그램이 사용자로부터 정보를 받고, 이를 토대로 화면 구성이나 콘텐츠 내용이 바뀌어야 한다면 StatefulWidgt, 고정된 내용을 사용자에게 일방적으로 보여주기만 한다면 StatlessWidget을 사용한다.      

MaterialApp은 Material Design을 사용하는 애플리케이션에서 공통적으로 요구되는 다수의 위젯들을 감싸주는 편리한 위젯이다.      

Scaffold는 Material Design으로 만든 모바일 엡이 화면에 어떻게 나타날지를 표현하는 클래스다.


## Hello World 프로그램 3

~~~dart
import 'package:flutter/material.dart';

main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  final groupAggregated = Container(
    padding: const EdgeInsets.all(20),
    child: Column(
      children: [
        const Text(
          'Shop Name',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
        Center(
          child: Image.network(
              'https://storage.googleapis.com/cms-storage-bucket/780e0e64d323aad2cdd5.png',
//      'https://storage.googleapis.com/cms-storage-bucket/c823e53b3a1a7b0d36a9.png',
//      'https://storage.googleapis.com/cms-storage-bucket/4cdf1c5482cd30174cfe.png',
              width: 300,
              height: 300),
        ),
        Container(
          padding: const EdgeInsets.all(20),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  Icon(Icons.star, color: Colors.green[500]),
                  Icon(Icons.star, color: Colors.green[500]),
                  const Icon(Icons.star, color: Colors.black),
                ],
              ),
              const Text(
                '170 Reviews',
                style: TextStyle(
                  color: Colors.black,
                  fontWeight: FontWeight.w800,
                  fontFamily: 'Roboto',
                  letterSpacing: 0.5,
                  fontSize: 20,
                ),
              ),
            ],
          ),
        ),
        DefaultTextStyle.merge(
          style: const TextStyle(
            color: Colors.black,
            fontWeight: FontWeight.w800,
            fontFamily: 'Roboto',
            letterSpacing: 0.5,
            fontSize: 18,
          ),
          child: Container(
            padding: const EdgeInsets.all(20),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                Column(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    Icon(Icons.kitchen, color: Colors.green[500]),
                    const Text('kitchen:'),
                  ],
                ),
                Column(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    Icon(Icons.timer, color: Colors.green[500]),
                    const Text('timer:'),
                  ],
                ),
                Column(
                  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                  children: [
                    Icon(Icons.restaurant, color: Colors.green[500]),
                    const Text('restaurant:'),
                  ],
                ),
              ],
            ),
          ),
        ),
      ],
    ),
  );

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'App Title',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('AppBar Title'),
        ),
        body:
            Column(mainAxisAlignment: MainAxisAlignment.spaceEvenly, children: [
          groupAggregated,
        ]),
      ),
    );
  }
}
~~~

## Flutter 공식 Counter 프로그램

~~~dart
// Copyright (c) 2019, the Dart project authors.  Please see the AUTHORS file
// for details. All rights reserved. Use of this source code is governed by a
// BSD-style license that can be found in the LICENSE file.

import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.white),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  final String title;

  const MyHomePage({
    Key? key,
    required this.title,
  }) : super(key: key);

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
~~~

StatefulWidget은 프로그램의 변경 가능한 상태(state)를 가지는 State 위젯을 만들어서 사용한다.       
상태(state)는 위젯이 프로그램 속에서 존재하는(life-cycle) 동안 변경될 수 있고 위젯이 화면 출력 작업을 하는 build() 메서드에 의해서 동기적으로 읽을 수 있는 정보이다.        
프로그램 실행 동안 바뀌는 값은 StatefulWidget 자체에 저장하는 것이 아니고, StatefulWidget이 만드는 State 위젯에 저장하는 것이다.     
State 위젯을 생성(create)하는 것이 StatefulWidget의 역할이다. 변경되는 정보 자체는 StatefulWidget이 만드는 State 위젯의 내용이다.      
이를 위해서 statefulWidget의 createState() 메서드를 오버라이드해야 한다.

## Stateless Widget 활용

~~~dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  final Map info = {
    'appTitle': 'StatelessWidget Demo',
    'appBarTitle': 'Flutter official Site',
    'titleImageLink':
    'https://storage.googleapis.com/cms-storage-bucket/'
    '2fl18a9971e4ca6ad737.png',
    'titleSectionHeader': 'Flutter on Mobile',
    'titleSectionBody': 'https://flutter.dev/multi-platform/mobile',
    'titleSectionScore':100,
    'textSection':'Bring your app idea to more users from day one by~~~' 
  };
  @override
  Widget build(BuildContext context){
    final titleImage = _buildTitleImage(info['titleImageLink']);
    Widget textSection = _buildTextSection(info['textSection']);
    Widget buttonSection = _buildButtonSection(Theme.of(context).primaryColor);
    Widget titleSection = _buildTitleSection(info['titleSectionHeader'],info['titleSectionBody'],info['titleSectionScore']);

    return MaterialApp(
      title: info['appTitle'],
      home: Scaffold(
        appBar: AppBar(
          title: Text(info['appBarTitle']),
        ),
        body: ListView(
          children:[
            titleImage,
            titleSection,
            buttonSection,
            textSection,
          ],
        ),
      ),
    );
  }
}
Image _buildTitleImage(String imageName){
  return Image.network(
    imageName,
    width:600,
    height:240,
    fit: BoxFit.cover,
  );
}
Container _buildTitleSection(String name, String addr, int count){
  return Container(
    padding: const EdgeInsets.all(32),
    child: Row(
      children: [
        Expanded(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children:[
              Container(
                padding: const EdgeInsets.only(bottom:8),
                child: Text(
                  name,
                  style: const TextStyle(
                    fontWeight: FontWeight.bold
                  ),
                ),
              ),
              Text(
                addr,
                style: TextStyle(
                  color: Colors.grey[500],
                ),
              ),
            ],
          ),
        ),
        Icon(
          Icons.star,
          color: Colors.red[500],
        ),
        Text('$count')
      ],
    ),
  );
}
Widget _buildButtonSection(Color color){
  return Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children:[
      _buildButtonColumn(color,Icons.assistant_navigation,'Visit'),
      _buildButtonColumn(color,Icons.add_alert_sharp,'Alarm'),
      _buildButtonColumn(color,Icons.share,'Share'),
    ],
  );
}
Column _buildButtonColumn(Color color,IconData icon, String label){
  return Column(
    mainAxisSize: MainAxisSize.min,
    mainAxisAlignment: MainAxisAlignment.center,
    children:[
      Icon(icon, color:color),
      Container(
        margin: const EdgeInsets.only(top:8),
        child:Text(
          label,
          style:TextStyle(
            fontSize: 13,
            fontWeight: FontWeight.w400,
            color:color,
          ),
        ),
      ),
    ],
  );
}
Container _buildTextSection(String section){
  return Container(
    padding: const EdgeInsets.all(32),
    child:Text(
      section,
      softWrap:true,
      textAlign:TextAlign.justify,
      style: const TextStyle(height:1.5, fontSize: 15),
    ),
  );
}
~~~

## Stateful Widget 활용

~~~dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget{
  final Map info = {
    'appTitle': 'StatelessWidget Demo',
    'appBarTitle': 'Flutter official Site',
    'titleImageLink':
    'https://storage.googleapis.com/cms-storage-bucket/'
    '2fl18a9971e4ca6ad737.png',
    'titleSectionHeader': 'Flutter on Mobile',
    'titleSectionBody': 'https://flutter.dev/multi-platform/mobile',
    'titleSectionScore':100,
    'textSection':'Bring your app idea to more users from day one by~~~' 
  };
  @override
  Widget build(BuildContext context){
    final titleImage = _buildTitleImage(info['titleImageLink']);
    Widget textSection = _buildTextSection(info['textSection']);
    Widget buttonSection = _buildButtonSection(Theme.of(context).primaryColor);
    Widget titleSection = _buildTitleSection(info['titleSectionHeader'],info['titleSectionBody'],info['titleSectionScore']);

    return MaterialApp(
      title: info['appTitle'],
      home: Scaffold(
        appBar: AppBar(
          title: Text(info['appBarTitle']),
        ),
        body: ListView(
          children:[
            titleImage,
            titleSection,
            buttonSection,
            textSection,
          ],
        ),
      ),
    );
  }
}
Image _buildTitleImage(String imageName){
  return Image.network(
    imageName,
    width:600,
    height:240,
    fit: BoxFit.cover,
  );
}
Container _buildTitleSection(String name, String addr, int count){
  return Container(
    padding: const EdgeInsets.all(32),
    child: Row(
      children: [
        Expanded(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children:[
              Container(
                padding: const EdgeInsets.only(bottom:8),
                child: Text(
                  name,
                  style: const TextStyle(
                    fontWeight: FontWeight.bold
                  ),
                ),
              ),
              Text(
                addr,
                style: TextStyle(
                  color: Colors.grey[500],
                ),
              ),
            ],
          ),
        ),
        const Counter(),
      ],
    ),
  );
}
Widget _buildButtonSection(Color color){
  return Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children:[
      _buildButtonColumn(color,Icons.assistant_navigation,'Visit'),
      _buildButtonColumn(color,Icons.add_alert_sharp,'Alarm'),
      _buildButtonColumn(color,Icons.share,'Share'),
    ],
  );
}
Column _buildButtonColumn(Color color,IconData icon, String label){
  return Column(
    mainAxisSize: MainAxisSize.min,
    mainAxisAlignment: MainAxisAlignment.center,
    children:[
      Icon(icon, color:color),
      Container(
        margin: const EdgeInsets.only(top:8),
        child:Text(
          label,
          style:TextStyle(
            fontSize: 13,
            fontWeight: FontWeight.w400,
            color:color,
          ),
        ),
      ),
    ],
  );
}
Container _buildTextSection(String section){
  return Container(
    padding: const EdgeInsets.all(32),
    child:Text(
      section,
      softWrap:true,
      textAlign:TextAlign.justify,
      style: const TextStyle(height:1.5, fontSize: 15),
    ),
  );
}
class Counter extends StatefulWidget{
  const Counter({
    Key? key,
  }) : super(key:key);

  @override
  State<Counter> createState() => CounterState();
}
class CounterState extends State<Counter>{
  int _counter = 0;
  bool _boolStatus = false;
  Color _statusColor = Colors.black;

  void _buttonPressed(){
    setState((){
      if(_boolStatus == true){
        _boolStatus = false;
        _counter--;
        _statusColor = Colors.black;
      }else{
        _boolStatus = true;
        _counter++;
        _statusColor = Colors.red;
      }
    });
  }
  @override
  Widget build(BuildContext context){
    return Row(
      children:[
        IconButton(
          icon: const Icon(Icons.star),
          color:_statusColor,
          onpressed: _buttonPressed,
        ),
        Text('$_counter'),
      ],
    );
  }
}
~~~