# teraterm-auto-solarized-macro

Automatic login and solarized color of teraterm

## 1. 概要

検証環境と商用環境を取り違える予防として、SSHログイン先ごとにTereTermの設定ファイルを切り替えるTeratermマクロを作成した。  
また、パスワードファイルから読み出しを行い、自動ログインも行う機能を実装した。  
パスワードは暗号化した上で、パスワードファイルに保存するよう、補助ツールも作成した。  

接続先ホスト選択  
![キャプチャ1.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/d6de9b78-460b-3d9f-eac0-e83693375d81.png)  
補助ツールでパスワードを暗号化保存  
![キャプチャ8.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/2ff2aad1-d4bc-007e-2fa5-183f5f726c1d.png)  
接続先に応じてiniファイルを切り替え  
![キャプチャ5.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/962d382c-aa53-b28a-d009-fdbef1472ce6.png)  

### 1-1. 機能

#### auto_solarized.ttl（本体）
* SSH自動ログイン機能（ベーシック認証）  
設定ファイルに記載されているパスワードは暗号化
* ホスト一覧プルダウンからのホスト選択  
ホスト一覧にないものは通常のTeraterm起動
* SSHログイン先ごとに設定ファイルの切り替え  
初期設定状態で３環境（商用, 検証, 開発）を用意  
　1. 商用：通常のteratermテーマ  
　2. 検証：solarized darkテーマ  
　3. 開発：solarized lightテーマ  

#### setpw.ttl（補助ツール）
* パスワードファイルの生成
* パスワードの暗号化

#### hosts.txt（ログイン情報設定ファイル）
* SSHログイン先ホスト情報の記載  
　1. IPアドレス  
　2. ホスト名  
　3. 環境  
　4. ログイン識別子  

### 1-2. マクロ作成の背景

会社で商用環境と検証環境を取り違えて作業を行い、自責障害となったという事例があった。  
（商用環境と検証環境のターミナルを横に並べて作業していたため、誤って作業したらしい、、、）  
チーム内で予防策を考えることになり、まずは簡単にできる環境ごとに背景色を変えることになった。  

### 1-3. マクロの実行環境

* Windows 
* Teraterm バージョン4.7.8以降

## 2. 使い方

### 2-1. 導入

1.Teratermインストール  
  ここでは割愛。以下を参照。  
  [TeraTermの最新版（4.82）をWindows7にインストールしてみました！](http://www.j-oosk.com/teraterm/install/1204/)  
2.ttlファイルをTeratermマクロ実行ファイル（ttmacro.exe）と関連付け  
  ここでは割愛。以下を参照。  
  [Tera Term－TTLファイルの関連付け（Windows7編）](http://www.j-oosk.com/teraterm/configuration/516/)  
3.本プログラムをダウンロード  
  [auto-solarized-macro(zip)](https://github.com/yusukew62/teraterm-auto-solarized-macro/archive/master.zip)  
4.本プログラムの配置  
  解凍してTeratermマクロ（ttmacro.exe）と同じフォルダにファイルを展開する  
  今回の追加対象  

```text
C/Program Files (x86)/teraterm
├── ttermpro.exe
├── ttpmacro.exe
├── auto_solarized.ttl (★)
├── hosts.txt (☆)
├── password.txt (☆)
├── setpw.ttl (●)
├── prd.ini (▲)
├── stg.ini (▲)
├── dev.ini (▲)
└── etc...

--------------------------------------
★：マクロ必須ファイル（マクロ本体）
☆：マクロ必須ファイル（設定ファイル）
●：マクロ補助ツール
▲：Teraterm iniファイル（必要に応じて用意）
--------------------------------------
```

### 2-2. 初期設定

* setpw.ttlにログインユーザ名とパスワードを記載する  
DATA_NUMにユーザ・パスワードの組み合わせの合計登録件数を設定（ここでは3としている）※画面キャプチャを差し替える  
ユーザ名は USER_LIST[連番]に設定  
パスワードは PW_LIST[連番]に設定  
![キャプチャ7.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/4b6df24e-10d5-1031-5857-ed737bd5d5d4.png)  
* setpw.ttlを実行する  
* password.txtが作成/更新されたことを確認する  
![キャプチャ8.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/2ff2aad1-d4bc-007e-2fa5-183f5f726c1d.png)  
* hosts.txtにSSH接続先情報を書く  
![キャプチャ6.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/6076ff4f-b255-3ea2-2b15-a4bb3dea28d5.png)  

### 2-3. 実行

1. auto_solarized.ttlを実行する  
2. 接続先ホストを選択する  
![キャプチャ1.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/d6de9b78-460b-3d9f-eac0-e83693375d81.png)  
**リストに登録されているホストへ接続した場合**  
リストからホスト名を選択し「OK」ボタンを押下する  
SSH接続され、hosts.txtで定義した環境ごとに異なるinitファイルを読んだことがわかる  
例）  
商用(prd): prd.ini  
![キャプチャ3.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/cc1ab34b-aa3a-5039-234c-12fc447d22f2.png)  
検証(stg): stg.ini  
![キャプチャ4.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/19e15624-fb61-9ab4-be1e-00e48952fb70.png)  
開発(dev): dev.ini  
![キャプチャ5.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/962d382c-aa53-b28a-d009-fdbef1472ce6.png)  
**登録されていないホストへ接続したい場合**  
「other」を選択すると通常のTeratermログイン画面が表示される  
![キャプチャ2.PNG](https://qiita-image-store.s3.amazonaws.com/0/43280/9c48600f-2456-b3d5-c6fd-fb7202e47b1d.png)  

## 3. マクロの仕様について

### 3-1. auto_solarized.ttlのロジック

鋭意作成中  

### 3-2. setpw.ttlのロジック

鋭意作成中  

## 4. 苦労したところ

teratermマクロでは以下の点に苦労した。  

* foreach がない  
* 文字列分割や比較処理後に結果が格納格納される一時変数が決まったグローバル変数に格納されるため、その後の処理で用いるときに、都度変数に入れるなどする必要があり冗長  
* 関数に引数を渡せない  
