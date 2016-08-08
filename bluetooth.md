# Bluetoothによるシリアル通信

10メートル程度までの距離の機器間で使用できる2.4GHz帯の電波による無線通信です。

スマートフォンやキーボード、マウスなどのパソコンの周辺機器等の音声や文字等の比較的簡単な通信に使用されます。


###Bluetoothモジュール
http://akizukidenshi.com/catalog/g/gM-07612/


## 1.ArduinoからPCへの文字送信

まずはArduinoからPCへカウントアップしていく数値を送信してみましょう。

###スケッチ

```c
#include <SoftwareSerial.h>

int bluetoothRx = 12;  // RX-I of bluetooth
int bluetoothTx = 13;  // TX-O of bluetooth

int count = 0; //送信用カウンタ

SoftwareSerial mySerial(bluetoothRx, bluetoothTx); // RX, TX

void setup()  
{
  // Bluetooth用のシリアルオープン
  mySerial.begin(115200);
}

void loop()
{
    // Bluetoothへ送信
    mySerial.println(count);

    count++;

    delay(1000);
}
```

###PCからBluetoothへの接続(Windows)
※ここではwindows7での接続方法を記入します。<br>
その他の場合はペアリング方法が少し異なります。

1. シリアル通信が行えるターミナルソフトをダウンロードします。

2. ArduinoにBluetoothを接続した状態でPCと接続します。

3. コントロールパネルよりデバイスとプリンタを選択します。

4. 画面上のメニューにあるデバイスの追加を選択し、表示されたデバイスから対象のBluetoothを選択し接続します。<br>
※接続するBluetoothは、Bluetoothのチップに貼られているラベルの「MAC ID」の下四桁が設定されているものになります。

5. コントロールパネルよりデバイスマネージャーを選択し、ポートからBluetoothの接続名を確認します。（接続名はCOM◯◯）

6. ターミナルソフトを起動し、シリアルから上記の接続名を選択します。（2つある場合は番号の小さい方を選択してください）<br>
※ここで接続が成功すればBluetoothモジュールのLEDが青で点灯します。

7. 転送レートをプログラムに合わせて変更する

###PCからBluetoothへの接続(Mac)
1. ArduinoにBluetoothを接続した状態でPCと接続します。
<br>
Bluetoothモジュールはこの時点では未接続のため、赤点滅している状態です。

2. Macのメニューよりシステム環境設定を開き、「Bluetooth」を選択

3. 対象のBluetoothモジュールを選択し、ペアリングを行います。
<br>
接続できない場合は数回ためしてみることで繋がる場合があります。

4. ターミナルを起動し、下記のコマンドを実行して接続先の確認します。
<br>
※「/dev/tty.RNBT」で始まるものがBrickになります。
```
sudo ls /dev/tty.*
```

5. ターミナルにて下記のコマンドを実行し、Screenを起動します。 
<br>
※「XXXXX」の箇所は上記で確認したものを設定します。
<br>また、115200で設定している箇所は転送レートになるのでプログラムに合わせて変更します。
```
sudo screen /dev/tty.XXXXX 115200
```
接続が成功すると、BluetoothモジュールのLEDが青に点灯します。
<br>
※ここで接続できない場合、一度ペアリングの状態を解除し、再度ペアリングから確認してください。




## 2.PCからArduinoへの文字送信
次にPCで入力した文字をArduinoに送信するプログラムを書いてみましょう。

###スケッチ
```c
#include <SoftwareSerial.h>

int bluetoothRx = 10;  // RX-I of bluetooth
int bluetoothTx = 11;  // TX-O of bluetooth

SoftwareSerial mySerial(bluetoothRx, bluetoothTx); // RX, TX

void setup()  
{

  // Arduino側のシリアルオープン
  Serial.begin(115200);

  // Bluetooth用のシリアルオープン
  mySerial.begin(115200);

  Serial.println("send start!");

}

void loop() // run over and over
{
  // Bluetoothから受け取ったデータがある場合
  if (mySerial.available()){
    // データ取得
    char c = mySerial.read();
    // 取得データをログに表示
    Serial.println(c);
  }
}
```

書き込みが終わったら、ArduinoIDEでシリアルモニタを開き、ターミナル（またはターミナルソフト）を表示させた状態で、キーボードから文字を入力して確認します。


## 3.PCとArduino間のデータ送受信
次にお互いが受け取った文字を表示するプログラムを書いてみましょう。

###スケッチ
```c
#include <SoftwareSerial.h>

int bluetoothRx = 12;  // RX-I of bluetooth
int bluetoothTx = 13;  // TX-O of bluetooth

SoftwareSerial mySerial(bluetoothRx, bluetoothTx); // RX, TX

void setup()  
{
  // Arduino側のシリアルオープン
  Serial.begin(115200);

  // Bluetooth用のシリアルオープン
  mySerial.begin(115200);

  Serial.println("start!");
}

void loop()
{
  // Bluetoothから受け取ったデータがある場合
  if (mySerial.available()){
    char c = mySerial.read();
    Serial.write(c);    
  }
  
  // シリアルモニタから入力したデータがある場合
  if (Serial.available()){
    mySerial.write(Serial.read());
  }
}
```
書き込みが終わったら、ArduinoIDEのシリアルモニタの上部にある入力用の項目から入力して送信ボタン(またはEnterキー)、ターミナル（またはターミナルソフト）を表示させた状態で、キーボードから文字入力をそれぞれ試してみましょう。


## 4.PCからLED操作
最後にPCからArduinoに接続されたLEDを制御してみたいと思います。

###スケッチ
```c
#include <SoftwareSerial.h>

#define ledPin A0

int bluetoothRx = 10;  // RX-I of bluetooth
int bluetoothTx = 11;  // TX-O of bluetooth

SoftwareSerial mySerial(bluetoothRx, bluetoothTx); // RX, TX

void setup()  
{

  // LEDpin
  pinMode(ledPin,OUTPUT);
  
  // Arduino側のシリアルオープン
  Serial.begin(115200);

  // Bluetooth用のシリアルオープン
  mySerial.begin(115200);

  Serial.println("start!");
}

void loop()
{
  // Bluetoothから受け取ったデータがある場合
  if (mySerial.available()){
    char c = mySerial.read();
    if (c == '1'){
      digitalWrite(ledPin,HIGH);
    }else if(c == '0'){
      
    }
    Serial.write(c);
  }
  
  // シリアルモニタから入力したデータがある場合
  if (Serial.available()){
    mySerial.write(Serial.read());
  }
}```