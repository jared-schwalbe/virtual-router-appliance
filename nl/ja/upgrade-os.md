---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# OS のアップグレード
VRA オペレーティング・システムのアップグレードは、サポート・チームによってアップロードされるローカル ISO ファイル上で、コマンド ``add system image`` を使用して実行できます。 使用可能な Vyatta アップグレード・バージョンのリストは、IBM サポート・チケット・システムを使用して取得できます。

アップグレード・プロセスを開始するには、IBM サポート・チケット・システムを使用してチケットをオープンし、アップグレードと新規 ISO イメージのシステムへのアップロードを要求します。 IBM サポートから、ISO ファイルがアップロードされた場所を示す E メールが届きます。 下の例では、ファイルはディレクトリー ``tmp`` にあります。

**注:** 以下に示したアップグレード・プロセスは、単一 VRA の場合です。 VRA を高可用性モードで使用している場合は、両方のシステムで同じアップグレード・コマンドを実行する必要があります。 さらに、最初に `BACKUP` マシンをアップグレードし、それが正しく機能していることを確認することをお勧めします。 次に、`MASTER` マシンにアクセスし、`reset vrrp` コマンドを使用してフェイルオーバーします。 最後に、`BACKUP` が制御を取った時点で、元の `MASTER` をアップグレードします。

VRA をアップグレードするには、以下の手順を実行します。

1. コマンド ``add system image <Local ISO File>`` を実行します。
2. **Enter** キーを押して ISO イメージのデフォルト名を受け入れるか、ユーザー独自のものを入力します。
3. 現在の構成ディレクトリーと構成ファイルを保存するかどうかを選択します。
4. 現在の構成から SSH ホスト鍵を保存するかどうかを選択します。
5. **Enter** キーを押してデフォルトのシステム・コンソールを受け入れるか、ユーザー独自のものを入力します。
6. **Enter** キーを押してデフォルトのコンソール速度を受け入れるか、ユーザー独自のものを入力します。
7. コマンド `reboot` を入力し、`Yes` を入力してマシンをリブートします。
8. VRA を高可用性モードで使用している場合は、2 番目のマシンでステップ 1 からステップ 7 までを再実行します。

以下の例は、アップグレード・プロセスを示しています。

```
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Brocade vRouter イメージ・インストーラーにようこそ。
ISO イメージ上のファイルの MD5 チェックサムをチェック中...OK。
このイメージにどのような名前を付けますか? [5.2R5S4.08091424]: 5.2R5S4
このイメージは次の名前になります: 5.2R5S4
現在の構成ディレクトリーと構成ファイルを
保存しますか? (はい/いいえ) [はい]:
現在の構成を保存中...
現在の構成から SSH ホスト鍵を
保存しますか? (はい/いいえ) [はい]:
SSH 鍵を保存中...
squashfs イメージをコピー中...
カーネルおよび initrd イメージをコピー中...
保存した構成を構成区画にコピー中。
保存した SSH ホスト鍵をコピー中。
必要なシステム・コンソールを入力してください [tty0]:
コンソール速度を入力してください [38400]:
ポスト・インストール・スクリプトを実行中...
完了。

vyatta@vra01:~$ show system image
システムには現在、次のイメージがインストールされています:

   1: 5.2R5S3.06301309 (実行イメージ)
   2: 5.2R5S4 (デフォルト・ブート)

vyatta@vra01:~$ reboot
リブートを続行しますか? (はい/いいえ) [いいえ] y
今すぐ、リブートのためにシステムは停止します。

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```