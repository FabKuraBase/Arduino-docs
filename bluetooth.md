# Bluetoothによるシリアル通信



Bluetoothモジュール
<br>
http://akizukidenshi.com/catalog/g/gM-07612/


## 1.ArduinoからPCへの文字送信

まずはArduinoからPCへ文字を送信してみます。

###スケッチ

```c
#include <SoftwareSerial.h>

int bluetoothRx = 10;  // RX-I of bluetooth
int bluetoothTx = 11;  // TX-O of bluetooth

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

１．シリアル通信が行えるターミナルソフトをダウンロードします。

２．ArduinoにBluetoothを接続した状態でPCと接続します。
<br>

３．デバイス、プリンタの設定を選択します。


４．デバイスの追加にて対象のBluetoothと接続します。
<br>※接続するBluetoothは、Bluetoothのチップに貼られているラベルの「MAC ID」の下四桁が設定されているものになります。

５．コントロールパネルよりデバイスマネージャーを選択し、ポートからBluetoothの接続名を確認します。（接続名はCOM◯◯）

６．ターミナルソフトを起動し、シリアルから上記の接続名を選択します。（2つある場合は番号の小さい方を選択してください）
※ここで接続が成功すればBluetoothモジュールのLEDが緑で点灯します。

７．転送レートをプログラムに合わせて変更する

###PCからBluetoothへの接続(Mac)
１．ArduinoにBluetoothを接続した状態でPCと接続します。
<br>
Bluetoothモジュールはこの時点では未接続のため、赤点滅している状態です。

２．Macのメニューよりシステム環境設定を開き、「Bluetooth」を選択

３．対象のBluetoothモジュールを選択し、ペアリングを行います。
<br>
接続できない場合は数回ためしてみることで繋がる場合があります。

４．ターミナルを起動し、下記のコマンドを実行して接続先の確認する。
<br>
※「/dev/tty.RNBT」で始まるものがBrickになります。
```
sudo ls /dev/tty.*
```

５．ターミナルにて下記のコマンドを実行し、Screenを起動 
<br>
※「XXXXX」の箇所は上記で確認したものを設定します。
```
sudo screen /dev/tty.XXXXX 115200
```
接続が成功すると、BluetoothモジュールのLEDが緑色に点灯します。
<br>
※ここで接続できない場合、一度ペアリングの状態を解除し、再度ペアリングから確認してください。




## 2.PCからArduinoへの文字送信



