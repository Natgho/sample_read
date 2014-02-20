uni_objects
======================
Rubyから4D DAMを利用するためのAPI(4D DAM→AI DAMに書き換える？)



動作環境
--------
+   CentOS release 5.8 (Final)  
    CentOS release 6.5 (Final)

+   4D DAM V10L1.4

+   gcc (GCC) 4.1.2 20080704 (Red Hat 4.1.2-52)  
    gcc (GCC) 4.4.7 20120313 (Red Hat 4.4.7-4) ※warning

+   ruby 1.9.3  
    ruby 2.0.0


以下、UniVerseディレクトリを`/usr/uv`として説明する


事前準備
--------
+   UniVerseインストール
+   4D DAMインストール
+   UVSYS,UVUSRの配置
+   NLSの設定
+   RubyGemsインストール
+   rubyインストール
+   bundlerインストール

+   `/usr/unishared/icsdk/intcall.h`の以下3行をコメントアウトする  

        // extern  void *calloc();  
        // extern  void *malloc();  
        // extern  void *realloc();


インストール手順
----------------
1. gemパッケージをインストールする  
   `gem install uni_objects`

2. インストール先へ移動する  
   `cd GEM PATHS/gems/uni_objects/`

3. `uni_objects/ext/uni_objects/extconf.rb`の
   `ictcall_header_path`を環境に合わせて更新する

4. 以下のコマンドを実行して拡張ライブラリをコンパイルする  

        bundle install  
        rake compile

5. `uni_objects/lib/uni_objects/uni_verse.rb`の以下4行を環境に合わせて更新する

        @@server = 'localhost'
        @@user = 'user'
        @@password = 'passwd'
        @@account = '/usr/uv/UVUSR'


テスト実行手順
--------------
1. `uni_objects/test/test_helper.rb`の以下4行を環境に合わせて更新する  

        ENV['server'] = 'localhost'  
        ENV['userid'] = 'user'  
        ENV['passwd'] = 'passwd'  
        ENV['account'] = '/usr/uv/UVUSR/'

2. `test/data/`のファイルを`ENV['account']`で指定した
   UVUSRアカウント配下のBPフォルダへコピーする

3. `rake test`


webアプリAPIテスト実行手順
--------------------------
1. サンプルアプリフォルダへ移動する
   `cd uni_objects/sample/cataloghouse/`

2. `cataloghouse/test/unit/helper/test_helper.rb`の以下4行を環境に合わせて更新する  

        ENV['server'] = 'localhost'  
        ENV['userid'] = 'user'  
        ENV['passwd'] = 'passwd'  
        ENV['account'] = '/usr/uv/UVUSR/'

3. `rake test`

**エラーが発生する場合**  
UniVerse Shellで`LIST VOC "USERS"`を実行し、下記のデータが存在するが確認する  
存在する場合は削除する

    NAME..........    TYPE    DESC..........................
    
    USERS             K       Keyword - Display STATUS
                              information about the USERS
                              logged into the system


サンプルアプリ実行手順
----------------------
1. `uni_objects/sample/create_sample_data.rb`の接続情報を環境に合わせて更新する

2. create_sample_data.rbを実行する

3. `uni_objects/sample/cataloghouse`に移動し、`rails server`を起動する

