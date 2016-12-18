# Bluetooth-thermometer
#include <Adafruit_NeoPixel.h>
#include <SHT2x.h>
#include <SoftwareSerial.h>
SoftwareSerial mySerial(4, 5); //RX,TX
#define my_Serial Serial
#include "I2Cdev.h"
#define PIXEL_PIN    A0
#define PIXEL_COUNT  6
Adafruit_NeoPixel strip = Adafruit_NeoPixel(PIXEL_COUNT, PIXEL_PIN, NEO_GRB + NEO_KHZ800);
#define temp1    28//#define定义常量
#define temp2    30
#define temp3    32

float sensor_tem;
void setup() {
  strip.begin();
String msg = "";
my_Serial.begin(9600);
}

void loop() {
  read();//调用自定义函数
  int t=(sensor_tem<temp1)+(sensor_tem<temp2)+(sensor_tem<temp3);
    switch(t)
    {
     case 3:
     colorSet(strip.Color(225, 255, 225));break;
     case 2:
     colorSet(strip.Color(0, 0, 255));break;
 
    case 1:
    colorSet(strip.Color(255, 255, 0));break;
 
    case 0:
    colorSet(strip.Color(255, 0, 0));break;
    }
    my_Serial.println("tem:");
    my_Serial.println(SHT2x.GetTemperature());
    my_Serial.println("\n");
}
void read()
{
  sensor_tem = SHT2x.GetTemperature();
  delay(100);
}
void colorSet(uint32_t c) {
  for (uint16_t i = 0; i < strip.numPixels(); i++) {
    strip.setPixelColor(i, c);//i:表示第几个灯开始，从0开始算第一个灯；c 表示灯的颜色
    strip.show();
  }
}
