---
title: NFC業界の基礎知識
date: 2011-10-14
tags: Uncategorized
authors: h_ninomiya
---
 
<p>初めまして、こんにちは。GTEインターン生の二宮と申します。今回は私、二宮が個人的に興味のあるFeliCa についてお話ししたいと思います。</p>
<p>流れとしては、まずはFeliCa周りの基礎知識を紹介し、それからFeliCaの識別番号を取得するような簡単なプログラムを組んでみます。記事を通じて、FeliCa / NFCを取り巻くドキドキ感を読者の方にお伝えできれば幸いです。</p>
<h2>1.FeliCaってなぁに？</h2>
<p>そもそも皆さん、FeliCa をご存知でしょうか。……おっしゃる通り、SuicaやらWAONやらに使われているソニーの技術方式ですね。この名前は英語で「至福」を意味するfelicityからきています。</p>
<p>どうしても「FeliCa = カード型」のイメージが拭えませんが(<em>*1</em>)FeliCa技術そのものは通信インタフェースとOSを指します。ですから、別にカード型である必要はなく、実際アジアではコイン型のFeliCaトークンが使われていたりもします。</p>
<p>また、おサイフケータイ機能に使われているモバイルFeliCaや、FeliCa Plugを使用した健康機器など(個人的にはあまり馴染みがありませんが……)もカード型じゃありませんよね。</p>
<p>また、FeliCaの基本はリーダ／ライタから給電された上で駆動するので比較的シンプルな事しかできないのですが、モバイルFeliCaやFeliCa Plugでは、これらを搭載したデバイスと連携していろいろ面白いことができるようですよ。</p>
<p>例として、モバイルFeliCaには「カードエミュレーション」「P2P」「リーダ・ライタ」の3つのモードがあります。「カードエミュレーション」以外のモードを意識されていない方も多いのではないでしょうか。私も先日のFeliCaソリューションセミナー2011において、受付証(FeliCa)をAndroid端末で認証された時は「おおっ！」ってなりました。このあたりの技術は今からお話しするNFCとの関連性含めて、今後熱くなっていくんじゃないのかなぁとか思う私なのです。</p>
<div><img src="/images/2011/10/mobileFelicaMode.png" alt="モバイルFeliCaのモード" title="mobileFelicaMode" /><p class="wp-caption-text">モバイルFeliCaのモード</p></div>
<h2>2.RFID やNFC とFeliCa の関係</h2>
<p>さて、FeliCaといえば、関連してRFIDだとかNFCだとかいうワードを思い浮かべる方もあるかもしれません。結論から言うと、FeliCa はRFID 技術の一例ですし、NFC の一種でもあります。</p>
<p>まず、RFIDの方からいきましょう。こちらはRadio Frequency IDentification、つまり無線個体識別のことですね。歴史的にはバーコードの系譜から分かれてきたみたいで、最初はバーコードの代替として使われていました。バーコードと違ってタグが隠れていても情報を読み取ることができて便利なのが良かったみたいですね。</p>
<p>そこから、タグの情報を書き換えられたり、持てる情報量が多かったりするRFIDタグの特性を利用して、商品情報そのものを書き込んだり、一つ一つの商品を識別したりするのに使われるようになりました。ですからRFIDというと割と物流方面で使われているイメージがありますね。</p>
<p>あとは、農場での家畜の管理や万引き防止で使われるEAS(Electronic Article Surveillance)(<em>*2</em>)の一部でもRFID技術が使われています。</p>
<p>そのRFID というくくりの内にカード型（人用）というジャンルがあり、さらにその中には通信距離に応じた3つのタイプ(密着型：～数mm、近接型：～10cm、近傍型：～70cm)がありました。世間一般で非接触ICカードと呼ばれるもの殆どは3つの内の近傍型(ISO/IEC 14443で標準化)に分類されます。もしかしたら、TypeA、TypeBという規格名を耳にしたことがあるかもしれませんが、これらはISO/IEC 14443内の区分けを指しているのです(<em>*3</em>)。</p>
<p>実はFeliCaもISO/IEC 14443のTypeCとして、国際規格化を目指していました。しかし、審議の時間切れのために却下されてしまいます。</p>
<p>どうしてもFeliCaを国際規格にしたい、とソニーが思ったのかは定かでありませんが、その後ソニーはフィリップスとFeliCaやMIFAREと下位互換性のある規格を共同開発し、標準化を目指します。そうして2003年末に国際規格として認められたのがISO/IEC 18092で、これが俗にいうNFC(Near Field Communication)です。NFCの規格は後に、TypeBやISO/IEC 15693（カード型RFIDの近傍型の規格）を含むように拡張されており、ISO/IEC 21481として標準化されています。ですから、現在NFCといえばカード型RFIDのうち、近接型、近傍型をカバーした上にFeliCaも取り扱える規格ということができます。</p>
<div><img src="/images/2011/10/nfc.png" alt="NFC内でのFeliCaの位置づけ" /><p class="wp-caption-text">NFC内でのFeliCaの位置づけ</p></div>
<p>モバイル国内独自機能の代表格、おサイフケータイ(モバイルFeliCa)のイメージが強いのでしょう、よく「FeliCa vs NFC」なんていう構図を見かけますが、正確にはFeliCaはNFCの一端を担っているため対立するものではありません。</p>
<p>参考：<a href="http://www.atmarkit.co.jp/frfid/special/5minic/01.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.atmarkit.co.jp']);">http://www.atmarkit.co.jp/frfid/special/5minic/01.html</a> / <a href="http://www.welcat.co.jp/knowledge/rfid/rfid003.php" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.welcat.co.jp']);">http://www.welcat.co.jp/knowledge/rfid/rfid003.php</a></p>
<h2>3.作成するプログラム</h2>
<p>やたらと前置きが長くなってしまいましたが、作成するプログラムはシンプルです。FeliCa一枚一枚に振られている識別番号(IDm)を取得してみます。FeliCaカードはリーダ・ライタからPollingコマンドが来たらその応答としてこのIDmを返すようになっています(<em>*4</em>)。その仕掛けを利用してカードに対してPollingコマンドだけを発信し、応答として得られたIDmを表示してみましょう。</p>
<p>開発ソフトはVisual C# 2010 Expressを使用します。SDKはSDK for FeliCa(<em>*5</em>)という本格的なものもありますが、こちらは入手が難しいため、有志が作成した<a href="http://felicalib.tmurakam.org/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://felicalib.tmurakam.org']);">felicalib</a>というものを利用します。今回はハードウェアとしてこのほかにリーダ・ライタ(<a href="http://www.amazon.co.jp/gp/product/B0047AHHD2/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.amazon.co.jp']);">このあたり</a>)が必要となりますので適宜準備のほどよろしくお願いします。</p>
<p>さて、プロジェクトをコンソールアプリケーションとして新規作成し、コードを以下のように書いてみます。</p>
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

using FelicaLib;

namespace FeliCaTest
{
    class Program
    {
        static void Main(string[] args)
        {
            Felica _felica;

            try
            {
                _felica = new Felica();
                _felica.Polling((int) 0xffff);

                Console.WriteLine("IDm : " + BitConverter(_felica.IDm()));
            }
            catch(Exception e)
            {
                Console.WriteLine("例外発生 - " + e.Message);
                return;
            }
        }
    }
}
```
<p>正直、説明するところがなくて困っています。そのくらいにシンプルですね。Pollingメソッドの引数は、システムコード(<em>*6</em>)という値を指しますが、深く気にする必要はありません。ここでは、どんなFeliCaでも反応するような値を入れています。</p>
<p>あ、felicalibのC#ライブラリを関連付けるのと、実行時にfelicalib.dllを探せる位置に置いておくことをお忘れなく。</p>
<div><img src="/images/2011/10/FelicaResult.png" alt="実行結果" title="FelicaResult" /><p class="wp-caption-text">実行結果</p></div>
<p>ここから工夫していく点としては、Pollingコマンドの発信部分をループにしてカード補足時のみイベントを発生させるようにモジュール化する、といったところでしょうか。</p>
<p>このようにFeliCaからIDmを読むこと自体は非常に簡単にできますが、データベースやネットワークと連携させれば、なかなか面白いことができると思います。<br />
是非お試しあれ。</p>
<h2>脚注</h2>
<ol>
<li>FeliCaの公式ページにも「ソニーの非接触ICカード技術方式」だなんて書いてありますしね " <a href="http://www.sony.co.jp/Products/felica/about/index.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.sony.co.jp']);">http://www.sony.co.jp/Products/felica/about/index.html</a></li>
<li>タグにIDがない、つまり個体識別していなかったら定義から外れるため、正確にはRFIDではありません。</li>
<li>TypeAはオランダのフィリップス社(現：NXPセミコンダクターズ)が開発したもので、MIFAREとも呼ばれます。世界で最も普及しており、日本ではtaspo(タスポ)などがこの規格を使っています。TypeBは、モトローラ社が開発を主導したもので、日本の運転免許証、パスポートに採用されています。</li>
<li>通信プロトコルやファイルシステム、コマンドシーケンスなどFeliCaの細かい技術仕様は公式サイトで入手することができます。 " <a href="http://www.sony.co.jp/Products/felica/business/tech-support/index.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.sony.co.jp']);">http://www.sony.co.jp/Products/felica/business/tech-support/index.html</a></li>
<li>「FeliCaもNFCなんだぜ！」ということをアピールするために、近々「SDK for NFC」に改称するとか何とか。</li>
<li>他のシステムコードの例としては、nanaco やWAON が使用している共通領域を表す0xfe00やSuica やPASMO が使用しているサイバネ領域を表す0x0003などがあります。</li>
</ol>