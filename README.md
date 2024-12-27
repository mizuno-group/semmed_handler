# SemMed handler

## 概要

このリポジトリでは、SemMed のデータの解析や、解析時に用いたコードをまとめています。

## データの概要

### 処理したデータ

[SemMedDB についてのサイト](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMedDB_download.html) からダウンロードしました。  
形式は csv ファイルを .gz 形式で圧縮したものです。

今回処理したテーブルは以下です：

* SENTENCE
* PREDICATION
* PREDICATION_AUX

### 各テーブルについて

カラムの詳細については、[SemMedDB のテーブルについての説明ページ](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/dbinfo.html) をご覧ください。

#### sentence

概要：title または abst を sentence で分けたもの  
行数：263,160,519  
論文数：

|カラム名|型|詳細 or 備考|
|---|---|---|
|SENTENCE_ID|int||
|PMID|int||
|TYPE|str|title or abst|
|NUMBER|int|title or abst 内で何番目か|
|SENT_START_INDEX|int||
|SENTENCE|str||
|SENT_END_INDEX|int||
|SECTION_HEADER|---|空欄|
|NORMALIZED_SECTION_HEADER|---|空欄|

#### predication

概要：sentence の subject、object についての関係性を判定したもの  
行数：130,480,194  
sentence 数：

|カラム名|型|詳細 or 備考|
|---|---|---|
|PREDICATION_ID|int||
|SENTENCE_ID|int||
|PMID|int||
|PREDICATE|int||
|SUBJECT_CUI|str||
|SUBJECT_NAME|str||
|SUBJECT_SEMTYPE|str||
|SUBJECT_NOVELTY|int||
|OBJECT_CUI|str||
|OBJECT_NAME|str||
|OBJECT_SEMTYPE|str||
|OBJECT_NOVELTY|int||

#### predication_aux

概要：predication についての追加情報。subject、object の開始位置や確信度などを含む。
行数：

|カラム名|型|詳細 or 備考|
|---|---|---|
||||

## コードの概要

### 主なスクリプト

* 241227_process_semmed.ipynb
    * ダウンロードしたデータの解凍、分割
    * 必要な列のみ DB に変換
    * 対応する sentence と predication を結合

## 関連資料

* [SemMedDB についてのサイト](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMedDB_download.html)
* [SemMedDB についての論文](https://lhncbc.nlm.nih.gov/ii/tools/SemRep_SemMedDB_SKR/SemMed.html)
