これはTheolizerのドライバをビルドする際llvmに含まれるlibToolingの一部のライブラリが必要になります。
llvm公式にあるWindows用のプリビルド版にはlibToolingが含まれていません。
そこで、当プロジェクトは、Theolizerをビルドするために必要なlibToolingライブラリを用意します。

### １．ビルド概要
llvmのビルド用にcmakeスクリプトを用意しています。そのcmakeスクリプトにて以下の処理を行います。<br>

1. 指定バージョンのllvmソース・コード一式を[llvm公式](http://releases.llvm.org/download.html#4.0.0)からダウンロード
2. llvmをビルドし、内部フォルダへインストール(１環境辺り１時間程度かかります)
3. Theolizerが用いるlibToolingを抽出し、find_package(LLVM)用のcmakeスクリプトを置き換えます。
4. 圧縮

### ２．ビルド用スクリプトの設定とビルド
cmakeスクリプトはbuild_libToolingのllvm_buildフォルダにおいてます。Windowsでビルドする場合はwindows.cmakeを用いて下さい。（linux用のlinux.cmakeも置いていますが、現時点では必要ないため見てテストです。）<br>
設定内容は以下の通りです。(相対指定する時は、各cmakeスクリプトがあるフォルダが起点となります。）

|設定先|設定内容|
|------|--------|
|LLVM_VERSION|使用するllvmのバージョン番号|
|LLVM_BINARY|ビルド作業に用いるバイナリ・フォルダ|
|CC32|MingWまたはgccの32ビット版のbinフォルダのパス(既にパスが通っているなら指定不要)|
|CC64|MingWまたはgccの64ビット版のbinフォルダのパス(既にパスが通っているなら指定不要)|
|build_process()|ビルドする組み合わせを指定|

zz_full_all.batを実行することでllvmソース一式をダウンロードしてビルドし、以下のフォルダへ各種ファイルを生成します。

|アイテム|フォルダ|
|-------|--------|
|圧縮llvmソース|${LLVM_BINARY}${LLVM_VERSION}/*.tar.xz|
|解凍llvmソース|${LLVM_BINARY}${LLVM_VERSION}/source/|
|ビルド・フォルダ|${LLVM_BINARY}${LLVM_VERSION}/${COMPILER}x${BIT_NUM}/|
|インストール先|${LLVM_BINARY}${LLVM_VERSION}/install/${COMPILER}x${BIT_NUM}/|
|Theolizer用抽出先|${LLVM_BINARY}${LLVM_VERSION}/package/${COMPILER}x${BIT_NUM}|
|Theolizer用圧縮ファイル|${LLVM_BINARY}${LLVM_VERSION}/package/${COMPILER}x${BIT_NUM}.tar.bz2|

COMPILDERとBIT_NUMは、それぞれbuild_process()の第１、第２パラメータです。

---
© 2016 [Theoride Technology](http://theolizer.com/) All Rights Reserved.  
["Theolizer" is a registered trademark of Theoride Technology.](http://theolizer.com/info/theolizer%E3%81%8C%E5%95%86%E6%A8%99%E7%99%BB%E9%8C%B2%E3%81%95%E3%82%8C%E3%81%BE%E3%81%97%E3%81%9F/)
