#include <M5Stack.h>
#include "utility/MPU9250.h"

MPU9250 IMU;

bool leftFlag = false; //左に動いたことを保持するフラグ
bool rightFlag = false; //右に動いたことを保持するフラグ

int maxPicSize = 3; //SDカード内に入れた画像の枚数
int th = 80;
int count = 1;

void setup()
{
  M5.begin();
  Wire.begin();
  M5.Lcd.setBrightness(100);
  M5.Lcd.drawJpgFile(SD, "/pictures/p0.jpg");
  byte c = IMU.readByte(MPU9250_ADDRESS, WHO_AM_I_MPU9250);
  IMU.calibrateMPU9250(IMU.gyroBias, IMU.accelBias);
}

void loop()
{
  if (IMU.readByte(MPU9250_ADDRESS, INT_STATUS) & 0x01){
    IMU.readGyroData(IMU.gyroCount);  // Read the x/y/z adc values
    IMU.getGres();
    IMU.gz = (float)IMU.gyroCount[2]*IMU.gRes;
  } 
  IMU.delt_t = millis() - IMU.count;
  if (IMU.delt_t > 100){
    int accel = (int)(IMU.gz);
    if(accel < -th){
      if(leftFlag){
        //左に振る→右に振るの後の処理
         M5.Lcd.fillScreen(BLACK);
         int num = count++ % maxPicSize;
         String s = "/pictures/p"+String(num)+".jpg";
         M5.Lcd.drawJpgFile(SD, s.c_str());
      }else{
        rightFlag = true;
      }
    }else if(accel > th){
      if(rightFlag){
        //右に振る→左に振るの後の処理

      }else{
        leftFlag = true;
      }
    }else{
      leftFlag = false;
      rightFlag = false;
    }
    IMU.count = millis();
  } 
}
