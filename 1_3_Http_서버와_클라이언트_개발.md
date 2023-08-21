## Http 프로토콜 이해

~~~dart
import 'dart:io';

Future main() async{
    var ip = InternetAddress.loopbackIpv4;
    var port = 4040;

    var server = await HttpServer.bind(
        ip,port,
    );
}

await for (HttpRequest request in server) {
    try{
        print("\$ http response");
        print("${DateTime.now()}");
        if(request.uri.path == '/'){
            request.response
                ..statusCode == HttpStatus.ok
                ..write("Hello, World");
        } else if(request.uri.path.contains('/add')){
            print("\$ http response is result of 'add' operation");

            var varList = request.uri.path.split(',');
            var result = int.parse(varList[1]) + int.parse(varList[2]);
            request.response
                ..statusCode = HttpStatus.ok
                ..write("${varList[1] + ${varList[2]} = $result}");
        } else if(await File(request.uri.path.substring(1)).exists() == true){
            print("\$ http response ${request.uri.path} filel transfer");
            var file = File(request.uri.path.substring(1));
            var fileContent = await file.readAsString();

            request.response
                ..statusCode = HttpStatus.ok
                ..headers.contentType = ContentType('text','plain',charset:"utf-8")
                ..write(fileContent);
        }else{
            print("\$ unsupported uri");
            request.response
                ..statusCode = HttpStatus.notFount
                ..write("Unsupported Uri");
        }

        await request.response.close();
    }catch(err){

    }
}
~~~

## Http Client & Server 개발

메서드 이름|주요기능
|:--:|:--:|
|GET|서버에서 자원(파일 등)을 클라이언트로 전달하도록 요청|
|PUT|클라이언트가 서버에게 전달한 자원을 서버에 저장하도록 요청|
|POST|클라이언트가 서버에게 정보(글자,숫자 등)를 전달|
|DELETE|서버에 저장되어 있는 자원을 삭제하도록 클라이언트가 요청|

~~~dart
//서버 소스
import 'dart:io';
import 'dart:convert';

Future main() async{
    var server = await HttpServer.bind(
        InternetAddress.loopbackIPv4,4040,
    );
    printHttpServerActivated(server);

    await for(HttpRequest request in server){
        printHttpRequestInfo(request);
        try{
            switch(request.method){
                case 'GET':
                    httpGetHandler(request);
                    break;
                case 'PUT':
                    httpPutHandler(server.address.address, server.port, request);
                    break;
                case 'POST':
                    httpPostHandler(request);
                    break;
                case 'DELETE':
                    httpDeleteHandler(request);
                    break;
                default:
                    print("Unsupported http method");
            }
        } catch(err){

        }
    }
}

void printHttpServerActivated(HttpServer server){
    var ip = server.address.address;
    var port = server.port;
    print("${ip}:${port}");
}
void printHttpRequestInfo(HttpRequest request) async{
    var ip = request.connectionInfo!.remoteAddress.address;
    var port = request.connectionInfo!.remotePort;
    var method = request.method;
    var path = request.uri.path;
    print("$method $path $ip:$port");

    if(request.headers.contentLength != -1){
        print("${request.headers.contentType}");
        print("${request.headers.contentLength}");
    }
}

void httpGetHandler(HttpRequest request) async{
    if(request.uri.path == '/'){
        var content = "Hello, World";
        request.response
            ..headers.contentType = ContentType('text','plain',charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.ok
            ..write(content);
    } else if(request.uri.path.contains('/add')){
        var vars = request.uri.path.split('/');
        var result = int.parse(vars[1]) + int.parse(vars[2]);
        var content = "$result";
        request.response
            ..headers.contentType = ContentType('text','plain',charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.ok
            ..write(content);
    } else if(await File(request.uri.path.substring(1)).exists() == true){
        var file = File(request.uri.path.substring(1));
        var content = await file.readAsString();
        request.response
            ..headers.contentType = ContentType('text','plain'charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.ok
            ..write(content);
    }else{
        var content = "Unsupported URI";
        request.response
            ..headers.contentType = ContentType('text','plain',charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.notFount
            ..write(content);
    }
    await request.response.close();
}
void httpPutHandler(var addr, var port, HttpRequest request) async{
    var content = await utf8.decoder.bind(request).join();
    var file = await File(request.uri.path.substring(1)).openWrite();
    print("$content");
    file
        ..write(content)
        ..close();
    content = 'http://$addr:$port${request.uri.path} created';
    request.response
        ..headers.contentType = ContentType('text','plain',charset:"utf-8")
        ..headers.contentLength = content.length
        ..statusCode = HttpStatus.ok
        ..write(content);
    await request.response.close();
}
void httpPostHandler(HttpRequest request) async{
    var content = await utf8.decoder.bind(request).join();
    var product = content.split("=");
    print("$content");
    content = "${product[1]}";
    request.response
        ..headers.contentType = ContentType('text','plain',charset:"utf-8")
        ..headers.contentLength = content.length
        ..statusCode = HttpStatus.ok
        ..write(content);
    await request.response.close();
}
void httpDeleteHandler(HttpRequest request) async{
    var filename = request.uri.path.substring(1);
    if(await File(filename).exists() == true){
        var content = "$filename deleted";
        File(filename).deleteSync();
        request.response
            ..headers.contentType = ContentType('text','plain',charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.ok
            ..write(content);
    }  else{
        var content = "$filename not found";
        request.response
            ..headers.contentType = ContentType('text','plain',charset:"utf-8")
            ..headers.contentLength = content.length
            ..statusCode = HttpStatus.notFound
            ..write(content);
    }
    await request.response.close();
}
~~~

~~~dart
//클라이언트 소스

import 'dart:io';
import 'dart:convert';

Future main() async{
    var serverIp = InternetAddress.loopbackIPv4.host;
    var serverPort = 4040;
    var serverPath;

    var httpClient = HttpClient;
    var httpResponseContent;
    var httpContent;

    HttpClientRequest httpRequest;
    HttpClientResponse httpResponse;

    print("step1. GET /");
    serverPath = "";
    httpRequest = await httpClient.get(serverIp, serverPort, serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    print("step2. GET /add,3,6");
    serverPath = "/add,3,6";
    httpRequest = await httpClient.get(serverIp,serverPort,serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpReponseContent);

    print("step3. GET /sample.txt");
    serverPath = "/sample.txt";
    httpRequest = await httpClient.get(serverIp,serverPort,serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpReponseContent);

    print("step4. POST item=product#1234");
    httpContent = "item=product#1234";
    serverPath = "/";
    httpRequest = await httpClient.post(serverIp,serverPort,serverPath)
        ..headers.contentType = contentType('text','plain',charset:"utf8")
        ..headers.contentLength = httpContent.length
        ..write(httpContent);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent); 

    print("step5. PUT /timestamp.txt");
    httpContent = "${DateTime.now()}";
    serverPath = "/timestamp.txt";
    httpRequest = await httpClient.put(serverIp,serverPort,serverPath)
    ..headers.contentType = contentType('text','plain',charset:"utf8")
    ..headers.contentLength = httpContent.length
    ..write(httpContent);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    print("step6. DELETE /timeStamp.txt");
    serverPath = "/timeStamp.txt";
    httpRequest = await httpClient.delete(serverIp, serverPort, serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);


}

void printHttpContentInfo(var httpResponse, var httpResponseContent){
    print("${httpResponse.statusCode}");
    print("${httpResponse.headers.contentType}");
    print("${httpResponse.headers.contentLength}");
    print("$httpResponseContent");
}
~~~

## JSON 활용

~~~dart
//클라이언트 소스

import 'dart:io';
import 'dart:convert';

Future main() async {
    var serverIp = InternetAddress.loopbackIPv4.host;
    var serverPort = 4040;
    var serverPath;

    var httpClient = HttpClient();
    var httpResponseContent;

    HttpClientRequest = httpRequest;
    HttpClientResponse = httpResponse;

    print("POST JSON Format");
    Map jsonContent = {'Korea':'Seoul','Japan':'Tokyo','China':'Beijing'};
    var content = jsonEncode(jsonContent);
    serverPath = "/";
    httpRequest = await httpClient.get(serverIp,serverPort,serverPath)
        ..headers.contentType = contentType.json
        ..headers.contentLength = content.length
        ..write(content);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);
}
void printHttpContentInfo(var httpResponse, var httpResponseContent){
    print("${httpResponse.statusCode}");
    print("${httpResponse.headers.contentType}");
    print("${httpResponse.headers.contentLength}");
    print("$httpResponseContent");
}
~~~

~~~dart
//서버 소스

import 'dart:io';
import 'dart:convert';

Future main() async {
    var server = await HttpServer.bind(InternetAddress.loopbackIPv4,4040,);
    printHttpServerActivated(server);
    await for(HttpRequest request in server){
        printHttpRequestInfo(request);
        try{
            var content = await utf8.decoder.bind(request).join();
            var jsonData = jsonDecode(content) as Map;
            print("$jsonData");
            print("${jsonData['Korea']}");
            content = "POST JSON accepted";
            request.response
                ..headers.contentType = contentType('text','plain',charset:"utf-8")
                ..headers.contentLength = httpContent.length
                ..statusCode = HttpStatus.ok
                ..write(content);
                await request.response.close();
        }catch(err){

        }
    }
}
void printHttpServerActivated(HttpServer server){
    var ip = server.address.address;
    var port = server.port;
    print("$ip:$port");
}

void printHttpRequestInfo(HttpRequest request) async{
    var ip = request.connectionInfo!.remoteAddress.address;
    var port = request.connectionInfo!.remotePort;
    var method = request.method;
    var path = request.uri.path;
    print("$method $ip:$port");

    if(request.headers.contentLength != -1){
        print("${request.headers.contentType}");
        print("${request.headers.contentLength}");
    }
}
~~~

## REST API 기반 CRUD 개발

~~~dart
//서버 소스

import 'dart:io';
import 'dart:convert';

Future main() async {
    var db = <dynamic, dynamic>{};

    var server = await HttpServer.bind(
        InternetAddress.loopbackIPv4,4040,
    );
    printHttpServerActivated(server);

    await for(httpRequest request in server){
        if(request.uri.path.contains('/api') == true){
            printHttpRequestInfo(request);
            try{
                switch(request.method){
                    case 'POST':
                        createDB(db,request);
                        break;
                    case 'GET':
                        readDB(db,request);
                        break;
                    case 'PUT':
                        updateDB(db,request);
                        break;
                    case 'DELETE':
                        delete(db,request);
                        break;
                    default:
                    print("Unsupported http method");
                }
            }catch(err){

            }
        }else{
            printAndSendHttpResponse(
                db.request, "${request.method} {ERROR: Unsupported API}"
            );
        }

    }
}

void printHttpServerActivated(HttpServer server){
    var ip = server.address.address;
    var port = server.port;
    print("$ip:$port");
}
void printHttpRequestInfo(HttpRequest request) async{
    var ip = request.connectionInfo!.remoteAddress.address;
    var port = request.connectionInfo!.remotePort;
    var method = request.method;
    var path = request.uri.path;
    print("$method $path $ip:$port");

    if(request.headers.contentLength != -1){
        print("${request.headers.contentType}");
        print("${request.headers.contentLength}");
    }
}
void printAndSendHttpResponse(var db, var request, var content) async{
    print("$content $db");
    request.response
        ..headers.contentType = contentType('text','plain',charset:"utf-8")
        ..headers.contentLength = httpContent.length
        ..statusCode = HttpStatus.ok
        ..write(content);
    await request.response.close();
}
void createdDB(var db, var request) async{
    var content = await utf8.decoder.bind(request).join();
    var transaction = jsonDecode(content) as Map;
    var key,value;

    print("$content");

    transaction.forEach((k,v){
        key = k;
        value = v;
    });

    if(db.containsKey(key) == false){
        db[key] = value;
        content = "Success $transction";
    } else {
        content = "$key";
    }
    printAndSendHttpResponse(db,request,content);
}
void readDB(var db, var request) async{
    var key = request.uri.pathSegments.last;
    var content,transaction;

    if(db.containsKey(key) == true){
        transaction = {};
        transaction[key] = db[key];
        content = "Success $transaction";
    }else{
        content = "Fail $key";
    }
    printAndSendHttpResponse(db,request,content);
}
void updateDB(var db, var request) async {
    var content = await utf8.decoder.bind(request).join();
    var transaction = jsonDecode(content) as Map;
    var key, value;

    print("$content");

    transaction.forEach((k,v){
        key = k;
        value = v;
    });

    if(db.containsKey(key) == true){
        db[key] = value;
        content = "Success $transaction";
    }else{
        content = "Fail $key";
    }
    printAndSendHttpResponse(db,request,content);
}
void deleteDB(var db, var request) async{
    var key = request.uri.pathSegments.last;
    var content,key;

    if(db.contains(key) == true){
        value = db[key];
        db.remove(key);
        content = "Success $value";
    }else{
        content = "Fail $key";
    }
    printAndSendHttpResponse(db,request,content);
}
~~~

~~~dart
//클라이언트 소스

import 'dart:io';
import 'dart:convert';

Future main() async{
    var serverIp = InternetAddress.loopbackIPv4.host;
    var serverPort = 4040;
    var serverPath;

    var httpClient = HttpClient();
    var httpResponseContent;

    HttpClientRequest = httpRequest;
    HttpClientResponse = httpResponse;

    var content;
    var jsonContent = <dynamic,dynamic>{};

    print("step1. Create by POST");
    jsonContent = {'0001':'Seoul'};
    content = jsonEncode(jsonContent);
    serverPath = "/api/0001";
    httpRequest = await httpClient.post(serverIp,serverPort,serverPath)
        ..headers.contentType = contentType.json
        ..headers.cotentLength = content.length
        ..write(content);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    print("step2. Create by POST");
    jsonContent = {'0002', 'Busan'};
    content = jsonEncode(jsonContent);
    serverPath = "/api/0002";
    httpRequest = await httpClient.post(serverIp,serverPort,serverPath)
        ..headers.contentType = contentType.json
        ..headers.contentLength = content.length
        ..write(content);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    print("step3. Read by GET");
    serverPath = "/api/0001";
    httpRequest = await httpClient.get(serverIp,serverPort,serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    
    print("step4. Update by PUT");
    jsonContent = {'0001', 'Sungnam'};
    content = jsonEncode(jsonContent);
    serverPath = "/api/0001";
    httpRequest = await httpClient.put(serverIp,serverPort,serverPath)
        ..headers.contentType = ContentType.json
        ..headers.contentLength = content.length
        ..write(content);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse, httpResponseContent);

    print("step5. Delete by DELETE");
    serverPath = "/api/0001";
    httpRequest = await httpClient.delete(serverIp,serverPort,serverPath);
    httpResponse = await httpRequest.close();
    httpResponseContent = await utf8.decoder.bind(httpResponse).join();
    printHttpContentInfo(httpResponse,httpResponseContent);
}
