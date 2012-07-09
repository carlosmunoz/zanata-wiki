# Diff of Publican Roundtrip Test

    $ diff src/test/resources/test-input/ja-JP/Accounts_And_Subscriptions.po target/test-output/ja-JP/Accounts_And_Subscriptions.po  > ~/Desktop/roundtrip_diff
    1,15d0
    < # translation of ja.po to Japanese
    < # Noriko Mizumoto <noriko@redhat.com>, 2006.
    < # Hyu_gabaru Ryu_ichi <hyu_gabaru@yahoo.co.jp>, 2007, 2008, 2009.
    < msgid ""
    < msgstr ""
    < "Project-Id-Version: ja\n"
    < "Report-Msgid-Bugs-To: http://bugs.kde.org\n"
    < "POT-Creation-Date: 2009-08-10 03:19+0000\n"
    < "PO-Revision-Date: 2009-08-01 20:14+0900\n"
    < "Last-Translator: Hyu_gabaru Ryu_ichi <hyu_gabaru@yahoo.co.jp>\n"
    < "Language-Team: Japanese <doc-i18n-list@redhat.com>\n"
    < "MIME-Version: 1.0\n"
    < "Content-Type: text/plain; charset=UTF-8\n"
    < "Content-Transfer-Encoding: 8bit\n"
    < 
    17d1
    < #: Accounts_And_Subscriptions.xml:6
    23d6
    < #: Accounts_And_Subscriptions.xml:7
    32,36c15,18
    < "Fedora の翻訳者になるには、この章に記載されているようにしてアカウントと登録を"
    < "求めて署名しなければなりません。質問があれば fedora-trans-list にポストする"
    < "か、<systemitem class=\"fqdomainname\">irc.freenode.org</systemitem> の "
    < "<systemitem>#fedora-l10n</systemitem> チャネルの Internet Relay Chat いわゆ"
    < "る <acronym>IRC</acronym> 経由で助けを求めてください。"
    ---
    > "Fedora の翻訳者になるには、この章に記載されているようにしてアカウントと登録を求めて署名しなければなりません。質問があれば fedora-"
    > "trans-list にポストするか、<systemitem class=\"fqdomainname\">irc.freenode.org</"
    > "systemitem> の <systemitem>#fedora-l10n</systemitem> チャネルの Internet Relay "
    > "Chat いわゆる <acronym>IRC</acronym> 経由で助けを求めてください。"
    39d20
    < #: Accounts_And_Subscriptions.xml:11
    45d25
    < #: Accounts_And_Subscriptions.xml:14
    49c29
    < "list\"></ulink> and subscribe to the main translation mailing list."
    ---
    > "list\" /> and subscribe to the main translation mailing list."
    51,52d30
    < "<ulink url=\"http://listman.redhat.com/mailman/listinfo/fedora-trans-list"
    < "\"></ulink> へ行き、メインの翻訳メーリングリストに登録します。"
    55d32
    < #: Accounts_And_Subscriptions.xml:19
    60,62c37
    < msgstr ""
    < "あなたの登録を確認するためのリンクを含んだ確認メールが届くのを待ちます。登録"
    < "を確認するリンクをクリックします。"
    ---
    > msgstr "あなたの登録を確認するためのリンクを含んだ確認メールが届くのを待ちます。登録を確認するリンクをクリックします。"
    65d39
    < #: Accounts_And_Subscriptions.xml:24
    68,70c42,44
    < "Check <ulink url=\"http://fedoraproject.org/wiki/L10N/Teams\"></ulink> to "
    < "see if there is a special mailing list for your language. If so, subscribe "
    < "to that list too."
    ---
    > "Check <ulink url=\"http://fedoraproject.org/wiki/L10N/Teams\" /> to see if "
    > "there is a special mailing list for your language. If so, subscribe to that "
    > "list too."
    72,74d45
    < "あなたの言語用の特別なメーリングリストがあるかどうか <ulink url=\"http://"
    < "fedoraproject.org/wiki/L10N/Teams\"></ulink> を見てチェックします。もしあれ"
    < "ば、そのメーリングリストにも登録します。"
    77d47
    < #: Accounts_And_Subscriptions.xml:33
    83d52
    < #: Accounts_And_Subscriptions.xml:34
    87c56
    < "that case, proceed to <xref linkend=\"st-change-permissions\"/> in the "
    ---
    > "that case, proceed to <xref linkend=\"st-change-permissions\" /> in the "
    91,94d59
    < "もしあなたが既に既存の SSH キーを持っているのならば、Fedora の作業用にそれを"
    < "使えます。その場合、以下の手続きの <xref linkend=\"st-change-permissions\"/> "
    < "に進みます。もしあなたが SSH キーを持っていなければ、以下の最初のステップに進"
    < "みます:"
    97d61
    < #: Accounts_And_Subscriptions.xml:39
    102,107d65
    < #. Tag: screen
    < #: Accounts_And_Subscriptions.xml:43
    < #, no-c-format
    < msgid "<command>ssh-keygen -t rsa</command>"
    < msgstr "<command>ssh-keygen -t rsa</command>"
    < 
    109d66
    < #: Accounts_And_Subscriptions.xml:44
    114,116c71
    < msgstr ""
    < "デフォルトの場所 (<filename>~/.ssh/id_rsa</filename>) を容認し、 パスフレーズ"
    < "を入力します。"
    ---
    > msgstr "デフォルトの場所 (<filename>~/.ssh/id_rsa</filename>) を容認し、 パスフレーズを入力します。"
    119d73
    < #: Accounts_And_Subscriptions.xml:48
    125d78
    < #: Accounts_And_Subscriptions.xml:49
    130,132c83
    < msgstr ""
    < "翻訳を送信するにはパスフレーズが必要になります。パスフレーズを忘れてしまうと"
    < "回復できません。"
    ---
    > msgstr "翻訳を送信するにはパスフレーズが必要になります。パスフレーズを忘れてしまうと回復できません。"
    135,142d85
    < #: Accounts_And_Subscriptions.xml:55
    < #, no-c-format
    < msgid "Change permissions to your key and <filename>.ssh</filename> directory:"
    < msgstr ""
    < "キーと <filename>.ssh</filename> ディレクトリーのパーミッションを変更します:"
    < 
    < #. Tag: screen
    < #: Accounts_And_Subscriptions.xml:59
    145,149c88,89
    < "<command>chmod 700 ~/.ssh</command> <command>chmod 600 ~/.ssh/id_rsa</"
    < "command> <command>chmod 644 ~/.ssh/id_rsa.pub</command>"
    < msgstr ""
    < "<command>chmod 700 ~/.ssh</command> <command>chmod 600 ~/.ssh/id_rsa</"
    < "command> <command>chmod 644 ~/.ssh/id_rsa.pub</command>"
    ---
    > "Change permissions to your key and <filename>.ssh</filename> directory:"
    > msgstr "キーと <filename>.ssh</filename> ディレクトリーのパーミッションを変更します:"
    152d91
    < #: Accounts_And_Subscriptions.xml:62
    157c96
    < "Accounts_and_Subscriptions-Applying_for_an_Account\"/>."
    ---
    > "Accounts_and_Subscriptions-Applying_for_an_Account\" />."
    159,162d97
    < "このパブリックキー (~/.ssh/id_rsa.pub) を <xref linkend=\"sect-"
    < "Translation_Quick_Start_Guide-Accounts_and_Subscriptions-"
    < "Applying_for_an_Account\"/> に書かれている Fedora アカウントの作成で使いま"
    < "す。"
    165d99
    < #: Accounts_And_Subscriptions.xml:71
    171d104
    < #: Accounts_And_Subscriptions.xml:74
    176,181d108
    < #. Tag: screen
    < #: Accounts_And_Subscriptions.xml:78
    < #, no-c-format
    < msgid "<command>gpg --gen-key</command>"
    < msgstr "<command>gpg --gen-key</command>"
    < 
    183d109
    < #: Accounts_And_Subscriptions.xml:79
    188,190c114
    < msgstr ""
    < "処理の間、一連のプロンプトがあなたを導きます。たいていの場合、省略値で十分で"
    < "す。よいパスワードを選択するのを忘れないようにしてください。"
    ---
    > msgstr "処理の間、一連のプロンプトがあなたを導きます。たいていの場合、省略値で十分です。よいパスワードを選択するのを忘れないようにしてください。"
    193d116
    < #: Accounts_And_Subscriptions.xml:83
    199d121
    < #: Accounts_And_Subscriptions.xml:84
    205d126
    < #: Accounts_And_Subscriptions.xml:89
    213d133
    < #: Accounts_And_Subscriptions.xml:94
    219d138
    < #: Accounts_And_Subscriptions.xml:99
    225d143
    < #: Accounts_And_Subscriptions.xml:107
    233,236c151,153
    < "結果のテキストの中で \"pub 1024D/1B2AFA1C\" に似た文の中であなたのキー ID を"
    < "探してください。あなたのキー ID はスラッシュ (<literal>/</literal>) の後の 8 "
    < "文字の '単語' です。前の例の GPG キー ID は <literal>1B2AFA1C</literal> で"
    < "す。あなたのキー ID を書き留めてください。"
    ---
    > "結果のテキストの中で \"pub 1024D/1B2AFA1C\" に似た文の中であなたのキー ID を探してください。あなたのキー ID はスラッシュ "
    > "(<literal>/</literal>) の後の 8 文字の '単語' です。前の例の GPG キー ID は <literal>1B2AFA1C</"
    > "literal> です。あなたのキー ID を書き留めてください。"
    239d155
    < #: Accounts_And_Subscriptions.xml:112
    244,256c160
    < msgstr ""
    < "他の人が見つけられるよう、以下のコマンドであなたの公開キーを公開サーバーに書"
    < "き出してください。キー ID は置き換えてください:"
    < 
    < #. Tag: screen
    < #: Accounts_And_Subscriptions.xml:116
    < #, no-c-format
    < msgid ""
    < "<command>gpg --keyserver subkeys.pgp.net --send-keys <replaceable>GPGKEYID</"
    < "replaceable></command>"
    < msgstr ""
    < "<command>gpg --keyserver subkeys.pgp.net --send-keys <replaceable>GPGKEYID</"
    < "replaceable></command>"
    ---
    > msgstr "他の人が見つけられるよう、以下のコマンドであなたの公開キーを公開サーバーに書き出してください。キー ID は置き換えてください:"
    259d162
    < #: Accounts_And_Subscriptions.xml:119
    264c167
    < "Accounts_and_Subscriptions-Applying_for_an_Account\"/>."
    ---
    > "Accounts_and_Subscriptions-Applying_for_an_Account\" />."
    266,268d168
    < "GPG キー ID は、<xref linkend=\"sect-Translation_Quick_Start_Guide-"
    < "Accounts_and_Subscriptions-Applying_for_an_Account\"/> に書かれている、あなた"
    < "用の Fedora アカウントの作成に使用されます。"
    271d170
    < #: Accounts_And_Subscriptions.xml:128
    277d175
    < #: Accounts_And_Subscriptions.xml:131
    281,282c179
    < "fedoraproject.org/accounts\"></ulink> and select <guilabel>New Account</"
    < "guilabel>."
    ---
    > "fedoraproject.org/accounts\" /> and select <guilabel>New Account</guilabel>."
    284,286d180
    < "Fedora アカウントに署名するには、まず<ulink url=\"http://admin.fedoraproject."
    < "org/accounts/\"></ulink> を訪れ、<guilabel>New account</guilabel> を選択しま"
    < "す。"
    289d182
    < #: Accounts_And_Subscriptions.xml:136
    296,298c189,191
    < "<guilabel>Username</guilabel> と、<guilabel>Full Name</guilabel>、"
    < "<guilabel>Email</guilabel> を埋め、<guibutton>Sign up!</guibutton> をクリック"
    < "します。パスワードがメールされます。"
    ---
    > "<guilabel>Username</guilabel> と、<guilabel>Full Name</"
    > "guilabel>、<guilabel>Email</guilabel> を埋め、<guibutton>Sign up!</guibutton> "
    > "をクリックします。パスワードがメールされます。"
    301d193
    < #: Accounts_And_Subscriptions.xml:141
    304,306c196,198
    < "Go back to <ulink url=\"http://admin.fedoraproject.org/accounts\"></ulink> "
    < "and log in with your password. The welcome page will be displayed, and it "
    < "reminds you that CLA is not completed and that an SSH key is not submitted."
    ---
    > "Go back to <ulink url=\"http://admin.fedoraproject.org/accounts\" /> and log "
    > "in with your password. The welcome page will be displayed, and it reminds "
    > "you that CLA is not completed and that an SSH key is not submitted."
    308,310d199
    < "<ulink url=\"http://admin.fedoraproject.org/accounts/\"></ulink> に戻り、あな"
    < "たのパスワードでログインします。歓迎のページが表示され、CLA は完了しておら"
    < "ず、SSH キーが送られていないことを知らせています。"
    313d201
    < #: Accounts_And_Subscriptions.xml:146
    318c206
    < "edit\"></ulink>."
    ---
    > "edit\" />."
    320,322d207
    < "パブリック SSH キーと、GPG キー ID を送るには、<guilabel>My Account</"
    < "guilabel> をクリックするか、<ulink url=\" https://admin.fedoraproject.org/"
    < "accounts/user/edit\"></ulink> へ行きます。"
    325d209
    < #: Accounts_And_Subscriptions.xml:151
    333,336c217,219
    < "Edit Account (user_name) ページでは、あなたの GPG キー ID を <guilabel>GPG "
    < "Key ID:</guilabel> 域に入れてください。パブリック SSH キーについては、"
    < "<guilabel>Public SSH Key:</guilabel> 域の次にある <guilabel>Browse...</"
    < "guilabel> ボタンをクリックし、あなたのパブリック SSH キーを指定してください。"
    ---
    > "Edit Account (user_name) ページでは、あなたの GPG キー ID を <guilabel>GPG Key ID:</"
    > "guilabel> 域に入れてください。パブリック SSH キーについては、<guilabel>Public SSH Key:</guilabel> "
    > "域の次にある <guilabel>Browse...</guilabel> ボタンをクリックし、あなたのパブリック SSH キーを指定してください。"
    339d221
    < #: Accounts_And_Subscriptions.xml:156
    346c228
    < "PrivacyPolicy\"></ulink>."
    ---
    > "PrivacyPolicy\" />."
    348,352d229
    < "CLA にサインするには、<guilabel>Telephone Number</guilabel> と "
    < "<guilabel>Postal Address</guilabel> 域を埋める必要があります。これらの情報は "
    < "admin グループを除いてアクセスできません。<ulink url=\"http://fedoraproject."
    < "org/wiki/Legal/PrivacyPolicy\"></ulink> にある Fedora Privacy Policy を参照し"
    < "てください。"
    355d231
    < #: Accounts_And_Subscriptions.xml:161
    363d238
    < #: Accounts_And_Subscriptions.xml:166
    368,370c243
    < msgstr ""
    < "ではあなたの情報を保存するためにこのページの末尾にある <guilabel>Save!</"
    < "guilabel> をクリックしてください。"
    ---
    > msgstr "ではあなたの情報を保存するためにこのページの末尾にある <guilabel>Save!</guilabel> をクリックしてください。"
    373d245
    < #: Accounts_And_Subscriptions.xml:174
    379d250
    < #: Accounts_And_Subscriptions.xml:175
    384,386c255
    < msgstr ""
    < "あなたは貢献者のライセンス同意 <acronym>CLA</acronym> を仕上げなければなりま"
    < "せん"
    ---
    > msgstr "あなたは貢献者のライセンス同意 <acronym>CLA</acronym> を仕上げなければなりません"
    389d257
    < #: Accounts_And_Subscriptions.xml:180
    392,394c260,262
    < "Visit <ulink url=\"http://admin.fedoraproject.org/accounts\"></ulink> and "
    < "login your account using your user name and password obtained from the "
    < "previous process."
    ---
    > "Visit <ulink url=\"http://admin.fedoraproject.org/accounts\" /> and login "
    > "your account using your user name and password obtained from the previous "
    > "process."
    396,397d263
    < "<ulink url=\"http://admin.fedoraproject.org/accounts/\"></ulink> を訪れ、前の"
    < "手続きで得たあなたのユーザー名とパスワードを使ってログインしてください。"
    400d265
    < #: Accounts_And_Subscriptions.xml:185
    404c269
    < "<ulink url=\"http://admin.fedoraproject.org/accounts/user/edit\"></ulink>."
    ---
    > "<ulink url=\"http://admin.fedoraproject.org/accounts/user/edit\" />."
    406,408d270
    < "歓迎のページでは、<guilabel>complete the CLA</guilabel> をクリックするか、"
    < "<ulink url=\" http://admin.fedoraproject.org/accounts/user/edit\"></ulink> へ"
    < "行ってください。"
    411d272
    < #: Accounts_And_Subscriptions.xml:190
    420,422c281,282
    < "電話番号と郵便番号の情報をまだ提供していなければ、Edit Account (user_name) "
    < "ページが現れます。そうでなければ Fedora Contributor License Agreement ページ"
    < "が表示されます。同意書をよく読んでいいと思えば <guibutton>I agree</"
    ---
    > "電話番号と郵便番号の情報をまだ提供していなければ、Edit Account (user_name) ページが現れます。そうでなければ Fedora "
    > "Contributor License Agreement ページが表示されます。同意書をよく読んでいいと思えば <guibutton>I agree</"
    426d285
    < #: Accounts_And_Subscriptions.xml:195
    432,433c291,292
    < "user-view ページが現れ、<guilabel>CLA:</guilabel> 域が <guilabel>CLA Done</"
    < "guilabel> と表示されます。"
    ---
    > "user-view ページが現れ、<guilabel>CLA:</guilabel> 域が <guilabel>CLA Done</guilabel> "
    > "と表示されます。"
    436d294
    < #: Accounts_And_Subscriptions.xml:204
    442d299
    < #: Accounts_And_Subscriptions.xml:207
    446,447c303,304
    < "Username\"></ulink>. This is very useful for Fedora contributors to get to "
    < "know and contact each other."
    ---
    > "Username\" />. This is very useful for Fedora contributors to get to know "
    > "and contact each other."
    449,451d305
    < "wiki 上での編集権を得たら、<ulink url=\"http://fedoraproject.org/wiki/User:"
    < "Username\"></ulink> で個人ページを作成してください。これは Fedora の貢献者に"
    < "とってお互いに知り合い、連絡をとるのに非常に便利です。"
    454d307
    < #: Accounts_And_Subscriptions.xml:212
    460,468c313,316
    < "SelfIntroduction\"></ulink>. Please remember to include your FAS user name "
    < "and your language. With this information, sponsor can identify you for "
    < "'cvsl10n' group joining approval."
    < msgstr ""
    < "<ulink url=\"http://fedoraproject.org/wiki/L10N/SelfIntroduction\"></ulink> "
    < "の指示に従って簡単な自己紹介を <systemitem>fedora-trans-list</systemitem> "
    < "メーリングリストと、ローカルチームのリストへ投稿します。あなたの FAS ユーザー"
    < "名と言語を含めることを忘れないでください。この情報でスポンサーは 'cvsl10n' グ"
    < "ループ参加承認の際にあなたを識別できます。"
    ---
    > "SelfIntroduction\" />. Please remember to include your FAS user name and "
    > "your language. With this information, sponsor can identify you for 'cvsl10n' "
    > "group joining approval."
    > msgstr ""
    471d318
    < #: Accounts_And_Subscriptions.xml:221
    477d323
    < #: Accounts_And_Subscriptions.xml:224
    483c329
    < "fedoraproject.org/accounts/user/view/user_name\"></ulink>."
    ---
    > "fedoraproject.org/accounts/user/view/user_name\" />."
    485,488d330
    < "user-view ページで左側のバーにある <guilabel>Apply For a new Group</"
    < "guilabel> をクリックします。このステップを後で実行したいのならば、あなたの "
    < "user-view ページは <ulink url=\"http://admin.fedoraproject.org/accounts/user/"
    < "view/user_name\"></ulink> からたどることができます。"
    491d332
    < #: Accounts_And_Subscriptions.xml:229
    496,498c337
    < msgstr ""
    < "アルファベットの <guilabel>C</guilabel> をクリックすると、左側のグループが "
    < "'c' から始まります。"
    ---
    > msgstr "アルファベットの <guilabel>C</guilabel> をクリックすると、左側のグループが 'c' から始まります。"
    501d339
    < #: Accounts_And_Subscriptions.xml:234
    507,508c345,346
    < "一覧からグループ名 <guilabel>cvsl10n</guilabel> を探し、<guilabel>Apply</"
    < "guilabel> をクリックします。"
    ---
    > "一覧からグループ名 <guilabel>cvsl10n</guilabel> を探し、<guilabel>Apply</guilabel> "
    > "をクリックします。"
    511d348
    < #: Accounts_And_Subscriptions.xml:239
    516c353
    < "Accounts_and_Subscriptions-Introducing_Yourself\"/>. Then your language "
    ---
    > "Accounts_and_Subscriptions-Introducing_Yourself\" />. Then your language "
    520,524d356
    < "全ての保証人と管理者にあなたの申請が通知されます。<xref linkend=\"sect-"
    < "Translation_Quick_Start_Guide-Accounts_and_Subscriptions-Introducing_Yourself"
    < "\"/> に従って自己紹介してください。そうするとあなたの言語の保証人があなたの保"
    < "証人を申し出ます。これには 1 時間から数日かかります。保証人が決まると資格通知"
    < "がメールされます。"
    527d358
    < #: Accounts_And_Subscriptions.xml:246
    535,538c366,367
    < "残りのステップは、アクセスのテストと、近いうちに必要となるかもしれない "
    < "Fedora の基盤の全てへの権利を与えることです。言語の保守を行う人と新しい言語を"
    < "始める人はこれに従ってください。翻訳者にとっては任意ですが、できるだけ従うよ"
    < "うにしてください。"
    ---
    > "残りのステップは、アクセスのテストと、近いうちに必要となるかもしれない Fedora "
    > "の基盤の全てへの権利を与えることです。言語の保守を行う人と新しい言語を始める人はこれに従ってください。翻訳者にとっては任意ですが、できるだけ従うようにしてください。"
    541d369
    < #: Accounts_And_Subscriptions.xml:253
    547d374
    < #: Accounts_And_Subscriptions.xml:254
    550,551c377,378
    < "Visit <ulink url=\"http://bugzilla.redhat.com/bugzilla/createaccount.cgi\"></"
    < "ulink> to create a Bugzilla account."
    ---
    > "Visit <ulink url=\"http://bugzilla.redhat.com/bugzilla/createaccount.cgi\" /"
    > "> to create a Bugzilla account."
    553,554d379
    < "<ulink url=\"http://bugzilla.redhat.com/bugzilla/createaccount.cgi/\"></"
    < "ulink> を訪れ、Bugzilla のアカウントを作成します。"
    557d381
    < #: Accounts_And_Subscriptions.xml:260
    563d386
    < #: Accounts_And_Subscriptions.xml:261
    571,574c394,396
    < "気楽にあなたの成果を楽しんでください。あなたは Fedora コミュニティーの一員と"
    < "して完全に認識され、デジタル署名されたドキュメントやメールができ、成果を投稿"
    < "し、wiki にコンテンツを出し、バグを投稿し、我々のグループで議論し、他の "
    < "Fedora チームに参加できます。"
    ---
    > "気楽にあなたの成果を楽しんでください。あなたは Fedora "
    > "コミュニティーの一員として完全に認識され、デジタル署名されたドキュメントやメールができ、成果を投稿し、wiki "
    > "にコンテンツを出し、バグを投稿し、我々のグループで議論し、他の Fedora チームに参加できます。"