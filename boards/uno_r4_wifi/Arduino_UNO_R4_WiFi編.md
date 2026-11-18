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

### 1.1 ツールのインストール
### 1.2 ドライバのインストール
### 1.3 インストールできたことの確認

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
この章では、FreeRTOSの導入方法、およびFreeRTOSを用いた題材について記述する。

### 5.1 FreeRTOSの導入・設定
UNO R4 WiFiにはFreeRTOSがすでに入っているため、追加でインストールは不要である。

### 5.2 題材5:複数タスク
この節では、複数タスクを実装する。<br>
ボードの動きとしては、LEDを一定の間隔で点滅させながら、ボタンを押した回数をカウントするものである。<br>
(4章で作成した、題材1:LED点滅、題材3:UART出力のプログラムを参照)
1. タクトスイッチをボードに接続する。(4.2 題材2のつなぎ方を参照)
2. プログラムを記述する。(以下のコードを参照)<br>
```
#include <Arduino_FreeRTOS.h>

void LedTask(void *pvParameters);
void ButtonTask(void *pvParameters);

int outPin = 13;
int inPin = 2;
long baud = 9600;

void setup(){
  Serial.begin(baud);
  pinMode(outPin, OUTPUT);
  pinMode(inPin, INPUT_PULLUP);

  xTaskCreate(
    LedTask,
    "LedTask",
    128,
    NULL,
    2,
    NULL
  );

  xTaskCreate(
    ButtonTask,
    "ButtonTask",
    128,
    NULL,
    1,
    NULL
  );

  vTaskStartScheduler();
}

void loop(){
  // 使用しない
}

void LedTask(void *pvParameters){
  while(1){
    digitalWrite(outPin, HIGH);
    vTaskDelay(pdMS_TO_TICKS(100));
    digitalWrite(outPin, LOW);
    vTaskDelay(pdMS_TO_TICKS(400));
  }
}

void ButtonTask(void *pvParameters){
  int val = 0;
  int count = 0;
  int state = 0;

  while(1){
    val = digitalRead(inPin);
    if(val == LOW){
      if(val != state){
        count = count + 1;
        Serial.print(count);
        Serial.println("回 ");
      }
    }
    
    state = val;
  }
}
```
3. 検証する。(4.1 題材1の検証の手順を参照)
4. 書込みをする。(4.1 題材1の検証の手順を参照)
5. LEDが一定の間隔で点滅しながら、ボタンを押した回数がシリアルモニタに表示されることを確認する。

### 5.3 題材6:タスク間通信
この節では、タスク間通信を実装する手順を記述する。<br>
ボードの動きとしては、一定間隔で点滅しているLEDを、ボタンを押している間のみ間隔を変更するものである。
1. タクトスイッチをボードに接続する。(4.2 題材2のつなぎ方を参照)
2. プログラムを記述する。(以下のコードを参照)<br>
```
#include <Arduino_FreeRTOS.h>   // FreeRTOSを使うために必要

QueueHandle_t buttonQueue;

void ButtonTask(void *pvParameters);
void LedTask(void *pvParameters);

int outPin = 13;
int inPin = 2;
long baud = 9600;
int queue_size = 1;

void setup(){
  Serial.begin(baud);   // 
  pinMode(outPin, OUTPUT);    // 
  pinMode(inPin, INPUT_PULLUP);   // 
  buttonQueue = xQueueCreate(queue_size, sizeof(int));    // 

  xTaskCreate(
    ButtonTask,
    "ButtonTask",
    128,
    NULL,
    1,
    NULL
  );

  xTaskCreate(
    LedTask,
    "LedTask",
    128,
    NULL,
    2,
    NULL
  );

  vTaskStartScheduler();
}

void loop(){
  // 使用しない
}

void ButtonTask(void *pvParameters){
  int val = 0;

  while(1){
    val = digitalRead(inPin);
    xQueueSend(buttonQueue, &val, 0);
  }
}

void LedTask(void *pvParameters){
  int button_state = 0;
  int loop_count = 0;
  while(1){
    xQueueReceive(buttonQueue, &button_state, portMAX_DELAY);
    if(loop_count == 0){
      if(button_state == HIGH){
        digitalWrite(outPin, HIGH);
        vTaskDelay(pdMS_TO_TICKS(500));
      }else{
        digitalWrite(outPin, HIGH);
        vTaskDelay(pdMS_TO_TICKS(100));
      }
      loop_count = 1;
    }else{
      if(button_state == HIGH){
        digitalWrite(outPin, LOW);
        vTaskDelay(pdMS_TO_TICKS(500));
      }else{
        digitalWrite(outPin, LOW);
        vTaskDelay(pdMS_TO_TICKS(100));
      }
      loop_count = 0;
    }
  }
}
```
3. 検証する。(4.1 題材1の検証の手順を参照)
4. 書込みをする。(4.1 題材1の検証の手順を参照)
5. LEDが一定間隔で点滅しながら、ボタンを押されいる間のみ間隔が狭くなっていることを確認する。

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
