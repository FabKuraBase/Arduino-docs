# 16.サーボモーター


## サーボモーター

![](servo1.jpg)

サーボモーターとは、位置や速度など制御するモーターで、ロボットの関節等に使用されます。

## 回路

回路を作成してみましょう。
今回は外部電源(電池)を使用するため、プラスマイナスや、ショートに注意して下さい。

![](servo2.jpg)



## スケッチ

スケッチしてみましょう。
電池を入れる前にスケッチ書き込みを行い、一旦PCから外してから電池をセットし、もう一度接続して確認しましょう。

```
#include <Servo.h>

int servo_pin = 3;
int vol_pin   = 0;

Servo myservo;

void setup() 
{ 
    myservo.attach(servo_pin);
} 

void loop() 
{ 
    // 可変抵抗のデータを取得
    int get_data = analogRead(vol_pin);
    // 取得した可変抵抗の電圧を位置情報に変換
    int move_pos = map(get_data, 0, 1023, 0, 180);
    // サーボモーターを動かす
    myservo.write(move_pos);
    delay(10);
} 
```

可変抵抗をドライバー等で操作し、サーボモーターの動きを確認してみましょう。
