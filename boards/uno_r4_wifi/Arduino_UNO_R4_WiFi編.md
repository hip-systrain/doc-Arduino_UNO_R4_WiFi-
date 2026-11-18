<!--
  手順書テンプレート(全ボード共通)
  - このファイルをコピーして、ボードごとのフォルダに保存してから書き始める。
  - 章の番号と名前は変えない。該当しない章は消さずに「本ボードでは作成しない(理由:〜)」と書く。
  - ボード独自の内容は、第9章以降に追加章として書く。
  - このコメントはプレビューには表示されない。消さずに残してよい。
-->

# 【Arduino UNO R4 WiFi】開発手順書

| 項目 | 内容 |
|---|---|
| 対象ボード | |
| 開発言語 | |
| IDE・ツール | (名称とバージョン) |
| SDK・ライブラリ | (名称とバージョン) |
| 開発PCのOS | (例:Windows 11 24H2) |
| 最終検証日 | (yyyy/mm/dd) |

## 改版履歴

| 版 | 日付 | 担当者 | 内容 |
|---|---|---|---|
| 0.1 | (yyyy/mm/dd) | | 新規作成 |

---

## 0. 概要

### 0.1 この手順書でできること
<!-- 読者がこの手順書を最後まで進めると何ができるようになるかを書く -->

### 0.2 必要な機材

| 品名 | 数量 | 備考 |
|---|---|---|
| | | |

### 0.3 前提知識・事前準備
<!-- 既存の Git/GitHub 運用・Redmine 運用を参考に、前提とする知識と準備を書く -->

---

## 1. 開発環境構築

### 1.1 Arduino IDEのインストール
Arduino IDEをインストールする手順を次に記載する。<br>

1. Arduino IDEのダウンロードページ(https://www.arduino.cc/en/software/)へアクセスする<br>

1. 「Windows Win 10 and newer, 64bits」を選択する<br>
<img src="./images/1-1_Tool_Install_01.png" width=40%>

1. 「DOWNLOAD」を押下する<br>
<img src="./images/1-1_Tool_Install_02.png" width=40%>

1. ダウンロードしたインストーラを実行する<br>
<img src="./images/1-1_Tool_Install_03.png" width=60%>

1. ライセンス条項を確認し、「同意する」を押下する<br>
<img src="./images/1-1_Tool_Install_04.png" width=40%>

1. インストールオプションの選択は変更せず、「次へ」を押下する<br>
<img src="./images/1-1_Tool_Install_05.png" width=40%>

1. インストール先フォルダは変更せず、「インストール」を押下する<br>
<img src="./images/1-1_Tool_Install_06.png" width=40%>

1. 「完了」を押下する<br>
<img src="./images/1-1_Tool_Install_07.png" width=40%>

### 1.2 デバイスドライバのインストール
デバイスドライバをインストールする手順を次に記載する。<br>

1. Arduino IDEを起動する<br>

1. Windowsセキュリティのポップアップが表示された場合、「許可」を押下する<br>
<img src="./images/1-2_Driver_Install_01.png" width=30%>

1. Adafruit Industries LLCポート(発行元:Adafruit Industries)のインストールが表示された場合、「インストール」を押下してインストールする<br>
<img src="./images/1-2_Driver_Install_02.png" width=40%>

1. Arduino USB Driver(発行元:Arduino srl)のインストールが表示された場合、「インストール」を押下してインストールする<br>
<img src="./images/1-2_Driver_Install_03.png" width=40%>

1. Arduino USB Driver(発行元:Arduino SA)のインストールが表示された場合、「インストール」を押下してインストールする<br>
<img src="./images/1-2_Driver_Install_04.png" width=40%>

1. Genuino USB Driver(発行元:Arduino LLC)のインストールが表示された場合、「インストール」を押下してインストールする<br>
<img src="./images/1-2_Driver_Install_05.png" width=40%>

1. ボードのアイコンを押下し、ボードマネージャを開く<br>
<img src="./images/1-2_Driver_Install_06.png" width=60%>
<img src="./images/1-2_Driver_Install_07.png" width=60%>

1. 「検索をフィルタ…」の入力欄に `"Arduino UNO R4 Boards"` と入力する<br>
<img src="./images/1-2_Driver_Install_08.png" width=60%>

1. フィルタで表示された「Arduino UNO R4 Boards」を「インストール」を押下してインストールする<br>
インストールが始まると右下にインストール状況が表示される
<img src="./images/1-2_Driver_Install_09.png" width=60%>

1. フィルタで表示された「Arduino UNO R4 Boards」を「インストール」を押下してインストールする<br>
インストールが始まると右下にインストール状況が表示される
<img src="./images/1-2_Driver_Install_09.png" width=60%>

1. インストール中にArduinoユニバーサルシリアルバスデバイス(発行元:Arduino SA)のインストールが表示されるので、「インストール」を押下してインストールする<br>
<img src="./images/1-2_Driver_Install_10.png" width=40%>

1. インストールが完了すると　`"Platform arduino:renesas_uno@1.6.0 installed"` と出力される<br>
<img src="./images/1-2_Driver_Install_11.png" width=60%>

### 1.3 Tera Termのインストール
Tera Termをインストールする手順を次に記載する。<br>

1. Tera termのダウンロードページ(https://github.com/TeraTermProject/teraterm/releases/)へアクセスする<br>

1. 「teraterm-5.7.1-x64.exe」を押下してインストーラをダウンロードする<br>
<img src="./images/1-3_Terminal_Install_01.png" width=50%>

1. ダウンロードしたインストーラを実行する<br>
<img src="./images/1-3_Terminal_Install_02.png" width=60%>

1. 「現在のユーザー用にインストール」を押下する<br>
<img src="./images/1-3_Terminal_Install_03.png" width=20%>

1. 言語の選択は変更せず、「OK」を押下する<br>
<img src="./images/1-3_Terminal_Install_04.png" width=20%>

1. 使用許諾誓約書を確認し、「同意」を押下する<br>
<img src="./images/1-3_Terminal_Install_05.png" width=40%>

1. 「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_06.png" width=40%>

1. インストール先は変更せず、「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_07.png" width=40%>

1. コンポーネントの選択は変更せず、「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_08.png" width=40%>

1. 言語の選択は変更せず、「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_09.png" width=40%>

1. スタートメニューフォルダの指定は変更せず、「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_10.png" width=40%>

1. 追加タスクの選択は変更せず、「次へ」を押下する<br>
<img src="./images/1-3_Terminal_Install_11.png" width=40%>

1. 「インストール」を押下する<br>
<img src="./images/1-3_Terminal_Install_12.png" width=40%>

### 1.4 インストールできたことの確認
#### 1.4.1 Arduino IDE と Tera Term
プログラムと機能の一覧にインストールしたツールがあることを確認事項とする。<br>

1. コントロールパネルを開く<br>
<img src="./images/1-4_Install_check_01.png" width=40%>

1. 「プログラム」を押下する<br>
<img src="./images/1-4_Install_check_02.png" width=40%>

1. 「プログラムと機能」を押下する<br>
<img src="./images/1-4_Install_check_03.png" width=40%>

1. 一覧に「Arduino IDE」があれば、インストールできていることを確認できる<br>
<img src="./images/1-4_Install_check_04.png" width=40%>

1. 一覧に「Tera term」があれば、インストールできていることを確認できる<br>
<img src="./images/1-4_Install_check_05.png" width=40%>

### 1.4.2 デバイスドライバ
デバイスマネージャーとArduino IDEで、Arduino UNO R4 WiFiを検出できることを確認事項とする。<br>

1. USBポートにArduino UNO R4 WiFiを接続する<br>

1. デバイスマネージャーを開く<br>
<img src="./images/1-4_Install_check_06.png" width=40%>

1. 「ポート(COMとLPT)」のツリーを開く<br>
<img src="./images/1-4_Install_check_07.png" width=40%>

1. 「USBシリアルデバイス」があれば、デバイスマネージャーで検出できていることを確認できる<br>
<img src="./images/1-4_Install_check_08.png" width=40%>

1. Arduino IDEを起動する<br>
<img src="./images/1-4_Install_check_09.png" width=60%>

1. 「ツール」を押下する<br>
<img src="./images/1-4_Install_check_10.png" width=60%>

1. 「ボード情報を取得」を押下する<br>
<img src="./images/1-4_Install_check_11.png" width=60%>

1. 「Arduino UNO R4 WiFi」のボード情報が表示できれば、Arduino IDEで検出できていることを確認できる<br>
<img src="./images/1-4_Install_check_12.png" width=20%>

## 2. シリアルターミナルの使い方

### 2.1 使用するツールと接続方法
### 2.2 通信設定(ポート、ボーレートなど)
### 2.3 動作確認

## 3. プロジェクト作成

### 3.1 新規プロジェクトの作成
### 3.2 プロジェクトの構成(フォルダ・ファイルの役割)

## 4. コーディング・ビルド・書込み(RTOSなし)

<!-- 各題材の節は「作るもの → 配線 → プログラム → ビルドと書込み → 動作確認」の順で書く -->

### 4.1 題材1:LED点滅
### 4.2 題材2:ボタン入力
### 4.3 題材3:UART出力
### 4.4 題材4:タイマ割込み

## 5. コーディング・ビルド・書込み(FreeRTOS)

### 5.1 FreeRTOSの導入・設定
### 5.2 題材5:複数タスク
### 5.3 題材6:タスク間通信

## 6. デバッグ

### 6.1 デバッガの接続(配線図・写真)
### 6.2 デバッグの開始
### 6.3 ブレークポイント・ステップ実行
### 6.4 変数・メモリ・レジスタの確認

## 7. トラブルシューティング

| 症状 | 原因 | 対処 |
|---|---|---|
| | | |

## 8. 参考資料

- 

<!-- ボード独自の追加章がある場合は、ここから「## 9. 〜」として追加する -->

<!--
  記述ルール(書きぶりを全章でそろえる)
  1. 見出し:章は「## 1. 開発環境構築」、節は「### 1.1 ツールのインストール」の形にする。
  2. 手順:番号付きリストで書き、1つの番号に1つの操作。文末は「〜する。」にそろえる。
  3. 確認:手順のあとに「> 確認:〜れば成功」と、成功したかどうかの判断方法を書く。
  4. 注意:まちがえやすい所は「> 注意:〜」と書く。
  5. 画像:画面操作にはスクリーンショットを付ける。images/ に置き、名前は担当番号から始める(例:p1_ext.png)。
  6. 図:処理の流れやタスクの関係は Mermaid で書く。配線は図か写真で、ピン番号と信号名を書く。
  7. プログラム:手順書の中にコードブロックで書き、日本語のコメントを付ける。
  8. バージョン:ツールと SDK のバージョンを書く。
  9. 用語:同じものは同じことばで書く。英数字は半角で、日本語との間に半角スペースを入れる。
  10. つまずいた点:第7章に必ず記録する。
-->
