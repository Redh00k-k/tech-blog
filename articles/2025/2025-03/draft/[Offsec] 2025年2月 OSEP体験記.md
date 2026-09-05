# はじめに
2025年2月末にOSEPを取得できました。
本稿はその受験記録について記載します。OSEP取得を目指している方の参考になればと思います。

# OSEP(OffSec Experienced Penetration Tester)の概要
OSEPについてざっくりと説明すると、OSCPで取得したスキルをベースに、セキュリティ機構を回避しつつ、オンプレで構成された環境に対するペネトレーションテストを行う試験です。OSEPは、セキュリティ関連の資格で有名なOSCP（Offsec Security Certified Professional）の上位資格となります。

最近のOSCPではActive Directoryで構成されたネットワークに関する内容が出題されていますが、それをより複雑にしたある組織を模した環境に対するペネトレーションテストとなります。
(本稿を作成するにあたって調べてみましたが、OSCPの内容はActive Directory以外にクラウド関連など、昔に比べてコンテンツが充実しているみたいですね)

# 受験前にしたこと
## C#の学習
OSEPのためのトレーニングコンテンツ（PEN-300）では、C#を使って学習していきます。
私はこれまでC#をあまり触れたことがなかったので、以下のコンテンツで事前勉強しました。
- [OSEP Code Snippets](https://github.com/chvancooten/OSEP-Code-Snippets)
- [defcon27_csharp_workshop](https://github.com/mvelazc0/defcon27_csharp_workshop)

## HackTheBox
OSCPを取得してから時間が経っていたので、HackTheBoxでWindowsマシンをメインで遊んでいました。
以下のマシンは攻略したマシンの中でOSEPライクだと私が思うマシンです。
- [Forest](https://www.hackthebox.com/machines/forest)
- [Support](https://www.hackthebox.com/machines/Support)
- [Outdated](https://www.hackthebox.com/machines/Outdated)

なお、OSEPライクなマシンをまとめてくださっているページがありますので、受験を検討している方はこちらを参考にしてただければと思います。

https://0xdf.gitlab.io/cheatsheets/offsec#osep-pen-300


# PEN-300
OSCP同様に、OSEPはテキスト・Labs・Challengesがあります。
私は、テキストとLabsに関してはある程度把握している内容だったので基本流し読みしつつ、知らない箇所は重点的に読むといった方法を取りました。

Challegensですが、Labsが終わり次第すぐに取り掛かりました。受講当初は6個でしたが、途中で1個増えて合計7個となりました。
所感ですが、Challengesの1-3まではセキュリティ機構の回避方法などのテキストやLabsで紹介されている内容を演習する感じで比較的簡単です。ただChallengeの4以降はADの要素が組み合わさり、横展開などを行う必要があります。

Challenge7は試験内容と非常に似ており、合格するならばこちらを重点的に学習するのが良いと思います。

## よく使っていたツール

- [Bloodhound](https://github.com/SpecterOps/BloodHound)
    - Active Directory環境にあるオブジェクトやACLを分析して、可視化するツール
        - 私はいくつかのChallengesでは、このツールを使用せずに手動で収集・分析するという縛りプレイをやっていたのですが、かなり時間がかかりました
        - 試験でも手動でやると、とても時間が足りなくなるので、こちらを使用することをおすすめします
- [ligoro-ng](https://github.com/nicocha30/ligolo-ng)
    - AD環境内での調査や横展開を行うために、TunneringやPivotingするためのツール
        - テキストやLabsではmeterpreterとSOCKSを使った方法が紹介されていますが、個人的にはこちらのツールをおすすめします（SOCKSよりも低いレイヤーで通信を確立するため、ポートスキャンなどの調査速度が段違いに良くなります）
- [NetExec](https://github.com/Pennyw0rth/NetExec)
    - Active Direcotry環境のセキュリティを評価するためのツール
        - 取得したクレデンシャルが他のマシンでも再利用されていないかなどを、一括で調査するときなどに使っていました

- [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1)
    - Windowsドメイン以上のネットワーク上の状況を把握するためのツール
        - Bloodhoundと並行して使ってました
- [PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)
    - Windowsの権限昇格に繋がりそうな設定ミスを確認できるツール
        - シェルを取得したマシンでは、大体インポートしていました


その他として、オリジナルのShellcode-Runner(C#、dll、PowerShell)も使っていました。検知回避しつつ、シェルコードを実行できたので非常に助かりました。
（ソースコードの公開は問題になりそうなので、割愛します）



# 試験について
## 試験前
大きく以下のことを実施しました。
- レポートのテンプレを作成
    - 昔使ったOSCP用のテンプレを、OSEPように少し改修しました
    - 業務ではWordを使っているので、テンプレもWordを使いました

- タイムスケジュールの作成
    - 約2日間に渡って行われる実技試験なので、最後まで体力を維持し続ける必要があります。そのため、意識的に休憩を取るためにタイムスケジュールを作成しました
    - タイムスケジュールに則って試験をしていたおかげか、目や肩、腰を傷めることもなく、試験を乗り切れました

- 試験ガイドの確認
    - OSCPを受験していた当時ですが、急遽AD問題を追加するなど試験内容が変更したことを経験しているので、受験一週間前と前日に合格するにあたっての要件を確認しました
        - 合格条件の確認
            - OSEPでは合格するには100点以上を獲得するか、試験の最終目的であるsecret.txtを取得する必要があります
                - https://help.offsec.com/hc/en-us/articles/360050293792-OSEP-Exam-Guide#point-allocation
                - https://help.offsec.com/hc/en-us/articles/360049781352-OSEP-Exam-FAQ#h_01FSRPSC1STTS1GTKGHSGFDPYH
        - 試験で使用しても良いツールなどの再確認
            - Bloodhoundやmeterpreterなどの利用可能
                - https://help.offsec.com/hc/en-us/articles/360050293792-OSEP-Exam-Guide#exam-restrictions


## 試験(実技)
OSEPは、ネットワークの構成やホストの構成は試験概要にも記載されていないため、本稿での説明は割愛します。
実技試験はOSCP同様に、基本的には以下をひたすら繰り返していく感じです。異なる点は、4の横展開部分があることと、セキュリティ機構の回避が必要だということです。

1. シェル取得
2. 脆弱性や設定ミスの確認、クレデンシャルの取得
3. 権限昇格
4. 再度2.をやって、横展開

試験に対する所感ですが、PEN-300で提供されるChallenge問題の1-6よりも難しく、Challenge7と同等の難易度だと感じました。

私の場合は試験開始してから12h後には、自己採点で100点を取得できました。その後、ぐっすり8hほど睡眠を取り、試験開始から1日経過した頃にはsecret.txtを取得できました。
残り一日でスクリーンショットの取得や、手順を確認しつつレポートを書きました。

## 試験(レポート作成)
実技試験中に大体レポートを書き終わっているので、レポートを推敲して十分に睡眠を取った後に提出しました。
OSCPのときは眠気との戦いでしたが、今回はある程度レポート書き終わっていたので、いつもの週末と同じような過ごし方ができました。

レポートのページ数は90ページほどの大作ですが、稚拙なレポートです。10ページほどは自作したShellcode-Runnerのソースコードなので、実際は80ページかもしれないです。

大作だったためか、合格通知は他の体験記と比較よりも少し遅い4営業日後に届きました。


# 最後に
私の中ではOSEPの取得は大きな目標の1つであったため、取得できたことを嬉しく思っております。OSEP合格を踏まえて、どうやってセキュリティ機構を回避するべきかというRedTeam視点がより一層深まったと同時に、どうやってそれらを検知するのかというBlueTeamよりの視点も今まで以上に考えるきっかけになりました。

また、OSEPはOSCPよりも現実的であるとは言えますが、EDRが動作していない環境であったり、クラウド関連が一切ないなど、実際の環境とは少し乖離があると思っています。このあたりは業務やトレーニング環境を構築するなどして補う必要がありそうです。

今後はBlueTeamのほうに注力していきたいと思います。

末筆ではございますが、最後まで読んでいただきありがとうございました。
