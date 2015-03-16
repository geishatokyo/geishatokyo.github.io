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
JDK8だとおそらく動かないので注意。
2. SBT (Simple Build Tool)  
Windowsユーザは [公式サイト](http://www.scala-sbt.org/release/docs/Getting-Started/Setup.html) からインストーラをダウンロードする。  
それ以外のユーザは[sbt-extras](https://github.com/paulp/sbt-extras) を使う。
3. Android SDK  
Windows ユーザは [公式サイト](http://developer.android.com/sdk/index.html) からインストーラをダウンロードする。  
Mac ユーザは brew でインストールする。  
`brew install android-sdk`
4. Android emulator (または Android 実機)  
Android SDK 付属のエミュレータは重たいので、Android 実機 か [Genymotion](https://www.genymotion.com/) を使うのがオススメ。

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

## 注意点

* JDK8 には対応していない。
JDK7 を使いましょう。
* ANDROID SDK への PATH にスペースを含むと動かない。
スペースを含まないディレクトリにインストールしなおしましょう。

## 宣伝

2015-03-14 に実施した勉強会 [第1回ゲーム開発ハンズオンin東北(仙台)](http://geishatokyo.doorkeeper.jp/events/20899) で上記のアプリ開発を行いました。

