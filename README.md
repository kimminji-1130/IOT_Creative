# IOT_Creative
## 아두이노 코드
import processing.serial.; 
import processing.net.;

Serial p; 
Server s; 
Client c; 
void setup() {
s = new Server(this, 1234); 
p = new Serial(this, "COM5", 9600); // 포트번호 5번에 있는 아두이노와 연결
}
String msg;
void draw() {
c = s.available();
if (c!=null) { 
String m = c.readString(); 
if (m.indexOf("PUT")==0) { /
int n = m.indexOf("\r\n\r\n")+4;
m = m.substring(n);
m += '\n';
print(m); // 서버(프로세싱)화면에 출력
p.write(m); // 아두이노에 시리얼로 값을 보냄
}
else if (m.indexOf("GET")==0) { // 온도값
c.write("HTTP/1.1 200 OK\r\n"); 
c.write("Content-length: " + msg.length() + " \r\n\r\n");
c.write(msg); // 클라이언트(앱인벤터)에 값을 줍니다.
}
}
if (p.available()>0) { 
String m = p.readStringUntil('\n'); 
if (m!=null) {
msg = m;
print(m);
}
}
}
## 앱인벤터 코드

import processing.serial.; // 시리얼을 사용하겠다는 코드입니다
import processing.net.;

Serial p; // 시리얼로 아두이노와 연결합니다.
Server s; // 서버로 클라이언트(앱인벤터)와 연결합니다.
Client c; // 앱인벤터 입니다.
void setup() {
s = new Server(this, 1234); // 포트 1234로 서버를 만듭니다.
p = new Serial(this, "COM5", 9600); // 포트번호 5번에 있는 아두이노와 연결합니다
}
String msg;
void draw() {
c = s.available();
if (c!=null) { //클라이언트가 있을떄
String m = c.readString(); // 클라이언트에서 보낸 문자열을 받습니다.
if (m.indexOf("PUT")==0) { // 깜빡이는 주기입니다
int n = m.indexOf("\r\n\r\n")+4;
m = m.substring(n);
m += '\n';
print(m); // 서버(프로세싱)화면에 출력합니다
p.write(m); // 아두이노에 시리얼로 값을 보내줍니다.
}
else if (m.indexOf("GET")==0) { // 온도값입니다
c.write("HTTP/1.1 200 OK\r\n"); // POST 규격
c.write("Content-length: " + msg.length() + " \r\n\r\n"); // 데이터 길이
c.write(msg); // 클라이언트(앱인벤터)에 값을 줍니다.
}
}
if (p.available()>0) { // 시리얼이 통신이 될떄
String m = p.readStringUntil('\n'); // 엔터가 나올때까지 읽습니다
if (m!=null) {
msg = m;
print(m);
}
}
}

##앱인벤터 Blocks
프로세싱에서 온도를 받으면 lable1의 텍스트를 받은 온도값으로 넣는다. 두번째 블락은 서버로 textbox1의 내용을 보낸다. 
