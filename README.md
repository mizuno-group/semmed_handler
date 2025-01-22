# SemMed handler

## 概要

このリポジトリには、SemMed のデータの解析や、解析時に用いたコードをまとめています。

## データの概要

### 処理したデータ

[SemMedDB についてのサイト](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMedDB_download.html) からダウンロードしました。  
バージョンは 43 で、形式は csv ファイルを .gz 形式で圧縮したものです。

今回処理したテーブルは以下です：

* SENTENCE
    * 概要：sentence ごとに情報がまとまっているテーブル
    * 行数：263,160,519 行
* PREDICATION
    * 概要：subject 、object の関係性を定義しているテーブル
    * 行数：130,480,195 行
* PREDICATION_AUX
    * 概要：predication の補足情報が載っているテーブル
    * 行数：130,480,181 行

各テーブルの詳細については、[SemMedDB のテーブルについての説明ページ](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/dbinfo.html) をご覧ください。

### 処理後のデータ

SENTENCE を 50,000 行ごとに区切り、対応するデータをまとめました。  
処理後のデータのテーブルの構成は以下のとおりです：  

* id（ pmid と sent_id をまとめたテーブル）
* sentence（ sentence の情報をまとめたテーブル）
* pred_sent（ sent_id と pred_id をまとめたテーブル）
* predication（ predication の情報をまとめたテーブル）
* pred_aux（ pred_id と pred_aux_id をまとめたテーブル）
* aux（ predication aux の情報をまとめたテーブル）

各 DB ファイルは 5,264 個あり、各ファイルのサイズは概ね 12 MB です。

#### id

|カラム名|型|詳細 or 備考|
|---|---|---|
|sent_id|int|sentence ごとに一意に割り振られた id|
|pmid|int||

#### sentence

|カラム名|型|詳細 or 備考|
|---|---|---|
|sent_id|int|sentence ごとに一意に割り振られた id|
|type|str|title or abst|
|num|int|title or abst 内で何番目の文か|
|start|int||
|end|int||
|sentence|str||

SECTION_HEADER と NORMALIZED_SECTION_HEADER は欠損値だったので不使用

#### pred_sent

|カラム名|型|詳細 or 備考|
|---|---|---|
|pred_id|int||
|sent_id|int||

#### predication

|カラム名|型|詳細 or 備考|
|---|---|---|
|pred_id|int||
|predicate|int||
|sub_name|str|マッピングされたもの|
|sub_ty|str||
|obj_name|str|マッピングされたもの|
|obj_ty|str||

#### pred_aux

|カラム名|型|詳細 or 備考|
|---|---|---|
|pred_aux_id|int||
|pred_id|int||

#### aux

|カラム名|型|詳細 or 備考|
|---|---|---|
|pred_aux_id|int||
|sub_text|str||
|sub_dist|int|主語と述語の距離|
|sub_maxdist|int|述語から主語までの間で候補となる名詞句の数|
|sub_start|int||
|sub_end|int||
|sub_score|int|マッピングの信頼度|
|ind_ty|str|述語の品詞|
|pred_start|int||
|pred_end|int||
|obj_text|str||
|obj_dist|int||
|obj_maxdist|int||
|obj_start|int||
|obj_end|int||
|obj_score|int||

## コードの概要

### 主なスクリプト

* 241227_prepare_semmed.ipynb
    * データベースを作るための準備
    * ダウンロードしたデータの解凍、分割、DB 化、index 作成
* 250108_process_semmed_norm.ipynb
    * sent_id、pred_id をもとにデータベース化
    * テーブルの内容は上記のとおり
* 250108_watch.ipynb
    * データの中身の確認など
* 250117_ext_cells.ipynb
    * 上記のデータベースから条件を満たすものについての情報を、テーブルの形式を崩さずにすべて取って来る

## 関連資料

* [SemMedDB についてのサイト](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMedDB_download.html)
* [SemMedDB についての論文](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMed.html)
