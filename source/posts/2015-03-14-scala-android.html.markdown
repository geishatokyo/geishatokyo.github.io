---
title: Scala で作る Android ゲーム
date: 2015-03-14 18:20 JST
tags: geishastudy,scala,android
authors: yamauchi
---

Scala で Android ゲームを開発する方法を紹介します。
クロスプラットフォーム環境ではないため弊社製品では使っていませんが、
Android のみでアプリ開発をしたい人には役に立つと思います。

## 開発環境の構築

Scala で Android アプリを開発するには以下のものが必要です。

1. JDK (Java Development Kit) 7  
[Oracleのサイト](http://www.oracle.com/technetwork/java/javase/downloads/index.html) からインストーラをダウンロードする。  
JDK8だと動かないので注意。
2. SBT (Simple Build Tool)  
Windowsユーザは [公式サイト](http://www.scala-sbt.org/release/docs/Getting-Started/Setup.html) からインストーラをダウンロードする。  
それ以外のユーザは[sbt-extras](https://github.com/paulp/sbt-extras) を使う。
3. Android SDK  
Windows ユーザは [公式サイト](http://developer.android.com/sdk/index.html) からインストーラをダウンロードする。  
Mac ユーザは brew でインストールする。  
`brew install android-sdk`
4. Android emulator (または Android 実機)  
Android SDK 付属のエミュレータは重たいので、Android 実機 か [Genymotion](https://www.genymotion.com/) を使うのがオススメ。  
本記事で使っている Android のバージョンは Android 4.4.2 (android 19) です。
5. SDK library のインストール  
スタートメニューまたは `android` コマンドで SDK manager を起動します。  
SDK manager で android 19 (または使いたいバージョン) のライブラリ一式をインストールします。

本記事では統合開発環境(IDE)を使わずにテキストエディタで開発をする前提で書いています。

## Hello World

上記の開発環境をインストールしたものとします。
Android アプリを動かしてみましょう。

1. サンプルアプリのダウンロード  
`git clone git@github.com:geishatokyo/scalappybird.git`
2. Android の起動  
実機をPCにUSB接続する。もしくはエミュレータを起動する。
3. デバイスの割り当て  
`cd scalappybird`  
`sbt devices`  
デバイスが発見されていればOK。
4. パッケージの作成  
`sbt android:package`
5. アプリの実行  
`sbt android:run`

## アプリの開発

基本的には MaihThread.scala を編集するだけで開発を進められるように設計してあります。  
ソースコードを編集したら `sbt android:run` で動作を確認するのを繰り返します。

## 正常に動作しない時に見直すこと

* Android エミュレータ(または実機)が起動しているか  
Android エミュレータ(または実機)が起動していないと、デバイスとして認識されません。  
有効なデバイスが認識されない場合 `sbt android:package`, `sbt android:run` のコマンドが正常に実行できません。
* Android OS のバージョン  
Android のバージョンが設定ファイルと合っているかを確認します。  
Android 4.4.2 (android 19) 以外のバージョンの OS を使う場合には、
build.sbt の `platformTarget in Android := "android-19"` を適切なバージョンに書き換えましょう。  
書き換えたあとは `sbt clean` を一度実行して環境を更新しましょう。
* JDK のバージョン  
JDK8 には対応していない。JDK7 を使いましょう。
* Android SDK のインストール先  
ANDROID SDK への PATH に日本語文字列やスペースを含むと動かないことがあります。
日本語文字列やスペースを含まないディレクトリにインストールしなおしましょう。

## 勉強会情報

上記の内容で 2015-03-14 に [第1回ゲーム開発ハンズオンin仙台](http://geishatokyo.doorkeeper.jp/events/20899) を行いました。

