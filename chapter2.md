# 2.123D Circuitsを使ってみよう

123D Circuitsとはブラウザ上で電子工作が行えるWebサービスです。
<br>
Arduino本体やセンサー等が無くても回路を作成し、実行結果を画面でみることができます。

http://123d.circuits.io/

まずサインインしてみましょう。
<br>
既に123Dでアカウントをお持ちの方は「Sign in」、お持ちでない方は「Sign up」をクリックしてみましょう。
<br>
![](circuits0.jpg)


「Sign in」をクリックした場合の画面です。
<br>
facebookのアカウントがあれば登録は必要ありません
無い場合でもEmailとパスワードを設定すればすぐ使うことができます。
<br>
![](circuits01-2.jpg)

サインインするとブレッドボードというもののみ表示されています。
<br>
まずはArduinoを追加するため、画面右上の「Components」を選択します。
<br>
![](circuits02.jpg)


画面下に一覧が出てきますので、少し下にスクロールして「Arduino uno」を選択します。
<br>
![](circuits03.jpg)


画面上のどこでもいいのでもう一度クリックし、Arduino unoを配置します。
<br>
配置したオブジェクトはドラッグ&ドロップで移動することがきます。
![](circuits04.jpg)

Arduinoの下側に「5V」という箇所があるのでクリックします。
<br>
その後、右のブレッドボード（穴の空いた板）のFの左から7番目の位置をクリックすると赤い線が引かれます。
![](circuits05.jpg)

次に抵抗を配置します。
<br>
Componentsより「Resistor」を選択し、配置します。
![](circuits06.jpg)

置いた状態では抵抗が縦向きに配置されますので、画面左上の回転マークをクリックし、抵抗を回転させます。
![](circuits07.jpg)

抵抗の右側をHの左から7番目になるように配置します。
<br>
その後、ComponentsよりLEDを配置します。
![](circuits08.jpg)

LEDの右側をIの左から3番目の位置になるように配置します。
<br>
その後、ArduinoのGNDと、Jの左から2番目を線で繋ぎます。
<br>
繋ぎ終わりましたら、右上の「StartSimulation」を押して実行し、LEDが点灯することを確認しましょう。
![](circuits09.jpg)

プログラムを変更する場合は、「Code Editor」をクリックします。
<br>
変更が完了しましたら、プログラムの上にある「Upload&Run」を押すことで結果を確認することができます。
![](circuits10.jpg)
