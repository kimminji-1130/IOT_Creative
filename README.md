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
