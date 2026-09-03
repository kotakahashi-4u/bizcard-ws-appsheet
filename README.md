# WorkspaceStudio × AppSheet による名刺管理システム
本プロジェクトは、Googleから提供されている `Workspace Studio` と `AppSheet` を用いた名刺管理システムを構築する手順書である。これを用いることで、Workspaceに閉じた世界での構築が可能であり、（Workspaceアカウントにおいては）エンタープライズセキュリティで保護された名刺管理システムが爆誕する。

## 利用するには
1. Workspace Studioが利用できる状態であること
2. AppSheetが利用できる状態であること
3. スマホにGoogleドライブアプリがインストールされていること（任意）  
   `※本システムにおいては、スマホのGoogleドライブアプリを用いたスキャンを前提としているが、別途スキャンアプリを用いている場合にはそれでも可`

## 準備
1. 以下リンクよりスプレッドシートのコピーを自身のドライブに保存する。  
   [スプレッドシートリンク](https://docs.google.com/spreadsheets/d/1rW5DPezaiUV1LRnrAcPQRzzHS_4dMAg_oRHLxMQW-qs/edit?usp=drive_link)  
   1. 上記よりスプレッドシートを開く
   2. メニューより [ファイル] > [コピーを作成] を押下する  
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/438fbd5e-db88-4359-995f-4f2208c4a30a" />
   3. 確認ダイアログが表示されるため、ファイル名や格納先を選択して、「コピーを作成」を押下する  
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/680266eb-7d84-4ec4-a8fc-65b1b8f52778" />
2. 名刺データの格納フォルダを作成する。本フォルダが後述する `Workspace Studio` におけるトリガーフォルダとなる。
3. 名刺データの取り込み済みフォルダを作成する。本フォルダはすでに処理済みの名刺データを格納する領域となる。

## 実装
### Workspace Studio
1. Workspace Studioにアクセスする。[Workspace Studio](https://studio.workspace.google.com/)
2. 左側メニューの「フローを新規作成する」を押下し、ワークフロー作成画面に遷移する。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/17203cca-6a95-4c02-ae83-4316c051734a" />
3. 以下の手順書に従って、WorkspaceStudio上にフローを作成する。  
   [WorkspaceStudio構築補助手順書](https://docs.google.com/document/d/1qCFVFs0elrgNmBaoJsovz6sFZ2uJAwIkz3yscGqEdRg/edit?usp=sharing)

### スプレッドシート（GAS）
1. `準備の 1` で作成したスプレッドシートを開く
2. メニューより [拡張機能] > [Apps Script] を押下する。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/02c88d1a-fca4-46f1-aab3-12466e0597f9" />
3. 左側メニュー[歯車マーク]を押下し、以下スクリプトプロパティを追加する。
   1. SHEET_NAME: `スプレッドシートのシート名（デフォルトであればシート1）`
   2. TARGET_FOLDER_ID: `準備の 2 で作成した格納フォルダのフォルダID`
   3. ARCHIVE_FOLDER_ID: `準備の 3 で作成した格納済みフォルダのフォルダID`  
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/b3543b57-e8b2-4109-9901-01df5ff07f91" />

> [!TIP]
> フォルダIDはGoogleドライブでフォルダを開いた際のURLから確認ができる。例えば、以下URLの場合においては、`folders/`以降の `1Qm-QQr99999999999999999999DnQAgO` がフォルダIDとなる。  
> https://drive.google.com/drive/folders/1Qm-QQr99999999999999999999DnQAgO
> 

4. 左側メニュー[エディタ]に戻り、`fillMissingIDs` を一度テスト実行する。これを行うことで本プログラムの承認権限を付与するダイアログが表示されるため、画面に従って承認を行う。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/9b995ff9-4a2a-42c0-a6fd-8325846374f4" />

### AppSheet
1. `準備の 1` で作成したスプレッドシートを開く
2. メニューより [拡張機能] > [AppSheet] > [アプリを作成] を押下する。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/a3cffca9-af44-4edc-947b-cd97620d82c9" />
3. しばらくすると以下のようなAppSheetを作成するための準備が整った画面が表示されるが、特に必要ないため、右上のバツボタンからダイアログを閉じる。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/0f7fcaef-dd62-46a0-ac6a-4c5f08efa26d" />
4. 左側の [Data] メニューを押下して、以下表に示すDataTableを設定する。なお、デフォルトでNo.26の [_ComputedName] までは作成されているが詳細な設定は別途必要であり、この台帳がすべてを担うため完全に再現すること。  
   <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/06a9ff1e-1cc1-4960-9fb8-8b0f081f0a70" />

| No. | NAME | TYPE | KEY? | LABEL? | FORMULA | SHOW? | EDITABLE? | REQUIRE? | INITIAL VALUE | DISPLAY NAME | DESCRIPTION | SEARCH? | SCAN? | NFC? | PII? |
|:---|:---|:---|:---:|:---:|:---|:---:|:---:|:---:|:---|:---|:---|:---:|:---:|:---:|:---:|
| 1 | _RowNumber | Number | ☐ | ☐ | | ☐ | ☐ | ☑ | | | Number of this row | ☐ | ☐ | ☐ | ☐ |
| 2 | name_raw | Name | ☐ | ☐ | | ☐ | ☑ | ☐ | | "会社名（スキャン表記）" | | ☐ | ☐ | ☐ | ☐ |
| 3 | name_normalized | Name | ☐ | ☐ | | ☑ | ☑ | ☐ | | "会社名" | | ☑ | ☐ | ☐ | ☐ |
| 4 | website | Url | ☐ | ☐ | | ☐ | ☑ | ☐ | | "Webサイト" | | ☐ | ☐ | ☐ | ☐ |
| 5 | full_name | Text | ☐ | ☑ | | ☑ | ☑ | ☐ | | "氏名" | | ☑ | ☐ | ☐ | ☑ |
| 6 | last_name | Name | ☐ | ☐ | | ☑ | ☑ | ☐ | | "姓" | | ☑ | ☐ | ☐ | ☑ |
| 7 | first_name | Name | ☐ | ☐ | | ☑ | ☑ | ☐ | | "名" | | ☑ | ☐ | ☐ | ☑ |
| 8 | name_kana | Name | ☐ | ☐ | | ☑ | ☑ | ☐ | | "氏名カナ" | | ☑ | ☐ | ☐ | ☑ |
| 9 | department | Text | ☐ | ☐ | | ☑ | ☑ | ☐ | | "部署" | | ☑ | ☐ | ☐ | ☐ |
| 10 | position | Text | ☐ | ☐ | | ☑ | ☑ | ☐ | | "役職" | | ☑ | ☐ | ☐ | ☐ |
| 11 | email | Email | ☐ | ☐ | | ☑ | ☑ | ☑ | | "メールアドレス" | | ☑ | ☐ | ☐ | ☑ |
| 12 | phone_mobile | Phone | ☐ | ☐ | | LEN(TRIM([phone_mobile])) > 0 | ☑ | ☐ | | "携帯電話" | | ☑ | ☐ | ☐ | ☑ |
| 13 | phone_work | Phone | ☐ | ☐ | | LEN(TRIM([phone_work])) > 0 | ☑ | ☐ | | "勤務先電話" | | ☑ | ☐ | ☐ | ☑ |
| 14 | phone_fax | Phone | ☐ | ☐ | | LEN(TRIM([phone_fax])) > 0 | ☑ | ☐ | | "FAX" | | ☑ | ☐ | ☐ | ☑ |
| 15 | phone_other | Phone | ☐ | ☐ | | LEN(TRIM([phone_other])) > 0 | ☑ | ☐ | | "その他連絡先" | | ☑ | ☐ | ☐ | ☑ |
| 16 | postal_code | Text | ☐ | ☐ | | ☑ | ☑ | ☐ | | "郵便番号" | | ☑ | ☐ | ☐ | ☐ |
| 17 | full_address | Address | ☐ | ☐ | | ☑ | ☑ | ☐ | | "住所" | | ☑ | ☐ | ☐ | ☑ |
| 18 | prefecture | Text | ☐ | ☐ | | ☑ | ☑ | ☐ | | "都道府県" | | ☑ | ☐ | ☐ | ☐ |
| 19 | estimated_industry | Text | ☐ | ☐ | | ☑ | ☑ | ☐ | | "業界区分（AI分析）" | | ☑ | ☐ | ☐ | ☐ |
| 20 | business_summary | LongText | ☐ | ☐ | | ☑ | ☑ | ☐ | | "事業概要（AI分析）" | | ☑ | ☐ | ☐ | ☐ |
| 21 | confidence_score | Decimal | ☐ | ☐ | | ☑ | ☑ | ☑ | | "AI読み取り信頼度" | | ☐ | ☐ | ☐ | ☐ |
| 22 | requires_manual_review | Yes/No | ☐ | ☐ | | ☑ | ☑ | ☑ | | "要確認フラグ" | | ☐ | ☐ | ☐ | ☐ |
| 23 | notes | LongText | ☐ | ☐ | | ☑ | ☑ | ☐ | | "特記事項" | | ☑ | ☐ | ☐ | ☑ |
| 24 | item_link | File | ☐ | ☐ | | ☑ | ☑ | ☑ | | | | ☐ | ☐ | ☐ | ☐ |
| 25 | ID | Text | ☑ | ☐ | | ☐ | ☐ | ☑ | UNIQUEID() | | | ☐ | ☐ | ☐ | ☐ |
| 26 | _ComputedName | Name | ☐ | ☐ | CONCATENATE([first_name]," ",[last_name]) | ☐ | ☐ | ☐ | | | | ☐ | ☐ | ☐ | ☑ |
| 27 | 名刺画像 | Image | ☐ | ☐ | CONCATENATE(<br/>&nbsp;&nbsp;"https://drive.google.com/thumbnail?sz=w1000&id=",<br/>&nbsp;&nbsp;INDEX(SPLIT([item_link], "/"), 6)<br/>) | ☑ | ☐ | ☐ | | | | ☐ | ☐ | ☐ | ☑ |
| 28 | 会社HP | Url | ☐ | ☐ | IF(<br/>&nbsp;&nbsp;RIGHT(<br/>&nbsp;&nbsp;&nbsp;&nbsp;IF(<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CONTAINS([website], "url?q="),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;INDEX(SPLIT(INDEX(SPLIT(SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", ""), "url?q="), 2), "&"), 1),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", "")<br/>&nbsp;&nbsp;&nbsp;&nbsp;),<br/>&nbsp;&nbsp;&nbsp;&nbsp;1<br/>&nbsp;&nbsp;) = "/",<br/>&nbsp;&nbsp;LEFT(<br/>&nbsp;&nbsp;&nbsp;&nbsp;IF(<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CONTAINS([website], "url?q="),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;INDEX(SPLIT(INDEX(SPLIT(SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", ""), "url?q="), 2), "&"), 1),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", "")<br/>&nbsp;&nbsp;&nbsp;&nbsp;),<br/>&nbsp;&nbsp;&nbsp;&nbsp;LEN(<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IF(<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CONTAINS([website], "url?q="),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;INDEX(SPLIT(INDEX(SPLIT(SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", ""), "url?q="), 2), "&"), 1),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", "")<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;)<br/>&nbsp;&nbsp;&nbsp;&nbsp;) - 1<br/>&nbsp;&nbsp;),<br/>&nbsp;&nbsp;IF(<br/>&nbsp;&nbsp;&nbsp;&nbsp;CONTAINS([website], "url?q="),<br/>&nbsp;&nbsp;&nbsp;&nbsp;INDEX(SPLIT(INDEX(SPLIT(SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", ""), "url?q="), 2), "&"), 1),<br/>&nbsp;&nbsp;&nbsp;&nbsp;SUBSTITUTE(INDEX(SPLIT([website], "]("), 2), ")", "")<br/>&nbsp;&nbsp;)<br/>) | ☑ | ☐ | ☐ | | "Webサイト" | | ☑ | ☐ | ☐ | ☐ |
| 29 | フリガナ頭文字 | Text | ☐ | ☐ | LEFT([name_kana], 1) | ☑ | ☐ | ☐ | | | | ☑ | ☐ | ☐ | ☑ |

> [!TIP]
> 1. カラムを追加する際は、データシート右上の「＋」ボタンを押下し、[Column name] と [App formula] を記入して追加する。  
>    <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/6aab54d5-f408-47ad-977f-aa71bb0f600e" />
> 2. [phone_mobile] や [phone_work] といった [SHOW?]列に対して数式を用いるには、各データ行の左端にある「鉛筆」マークを押下し、[Show?] に関数を設定して保存する。  
>    <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/94fcf6f4-e1b3-4d18-8635-a320e55b39c1" />

> [!IMPORTANT]
> **PII? (Personally Identifiable Information)カラム**  
> チェックを入れると、AppSheetのサーバー側で記録される監査ログ（Audit History）等で、そのデータがマスキング（黒塗り）されて保存される。GDPRなどのプライバシー保護要件に対応するための機能。

5. [Data] がすべて上述通りに設定が行えたら、一度右上の「Save」ボタンを用いて保存する。
6. 左側の [Views] メニューを押下して、レイアウトメニューに遷移し、以下手順でレイアウトを作成する。

#### **Primary Navigation**
##### **都道府県別**
既存の[Map]ナビゲーションを上書きして、都道府県別ナビゲーションを作成する。  
<img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/4de29f76-72ec-479b-bf9f-91ee68553d96" />
         
| セクション | 設定項目 (Item) | 設定値 (Value) |
| :--- | :--- | :--- |
| **基本設定** | View name | 都道府県別 |
| | For this data | [Data]メニューで定義しているデータ名（例: シート1） |
| | View type | deck |
| | Position | first |
| **View Options** | Sort by | name_raw / Ascending |
| | Group by | prefecture / Ascending |
| | Group aggregate | COUNT |
| | Main image | Auto assign (名刺画像) |
| | Image shape | Full Image |
| | Primary header | Auto assign (full_name) |
| | Secondary header | name_normalized |
| | Summary column | position |
| | Nested table column | （空欄） |
| | Show action bar | ON （トグル有効） |
| | Actions | Manual<br>・Compose Email (email) |
| **Display** | Icon | お好きなものを |
| | Display name | （空欄） |
| | Show if | （空欄） |
| **Behavior** | 特段設定変更不要 |
| **Documentation** | 特段設定変更不要 |


##### **名前別**
既存の[シート1]ナビゲーションを上書きして、名前別ナビゲーションを作成する。  
<img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/027edc8a-848f-4f62-a6a2-af1979d0d73b" />

| セクション | 設定項目 (Item) | 設定値 (Value) |
| :--- | :--- | :--- |
| **基本設定** | View name | 名前別 |
| | For this data | [Data]メニューで定義しているデータ名（例: シート1） |
| | View type | deck |
| | Position | next |
| **View Options** | Sort by | name_kana / Ascending |
| | Group by | フリガナ頭文字 / Ascending |
| | Group aggregate | COUNT |
| | Main image | Auto assign (名刺画像) |
| | Image shape | Full Image |
| | Primary header | Auto assign (full_name) |
| | Secondary header | name_normalized |
| | Summary column | position |
| | Nested table column | （空欄） |
| | Show action bar | ON （トグル有効） |
| | Actions | Manual<br>・Compose Email (email) |
| **Display** | Icon | お好きなものを |
| | Display name | （空欄） |
| | Show if | （空欄） |
| **Behavior** | 特段設定変更不要 |
| **Documentation** | 特段設定変更不要 |

#### System Generated（各シートの詳細と入力フォームの作成）
##### **シート1_Detail**
これが各ナビゲーションメニューからデータを押下した際の詳細画面を担う。  
<img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/cbc515ad-9745-4804-836c-6b53bf493d4c" />

| セクション | 設定項目 (Item) | 設定値 (Value) |
| :--- | :--- | :--- |
| **View Options** | Use Card Layout | OFF（トグル無効） |
| | Main image | Auto assign (None) |
| | Header columns | 1. full_name<br>2. position<br>3. name_normalized |
| | Quick edit columns | （空欄） |
| | Sort by | （空欄） |
| | Column order | Manual<br>・会社HP<br>・department<br>・email<br>・phone_mobile<br>・phone_work<br>・postal_code<br>・full_address<br>・estimated_industry<br>・business_summary<br>・名刺画像 |
| | Display mode | Automatic |
| | Image style | Fill |
| | Nested row display | 5 |
| | Slideshow mode | ON（トグル有効） |
| | Desktop layout | Split view |
| | Desktop multicolumn layout | ON（トグル有効） |
| **Display** | 特段設定変更不要 |
| **Behavior** | 特段設定変更不要 |
| **Documentation** | 特段設定変更不要 |

##### **シート1_Form**
これがDetailレイアウトから「Edit」ボタンを押下した際のフォーム画面を担う。  
<img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/b2638ac7-4b5d-4707-97f7-12df41408b1c" />

| セクション | 設定項目 (Item) | 設定値 (Value) |
| :--- | :--- | :--- |
| **View Options** | Page style | Automatic |
| | Form style | Automatic |
| | Column order | Manual<br>・name_normalized<br>・website<br>・full_name<br>・last_name<br>・first_name<br>・name_kana<br>・department<br>・position<br>・email<br>・phone_mobile<br>・phone_work<br>・phone_fax<br>・phone_other<br>・postal_code<br>・full_address<br>・<br>・prefecture<br>・item_link |
| | Save / cancel position | Bottom |
| | Max. nested rows | 5 |
| | Auto save | OFF（トグル無効） |
| | Auto re-open | OFF（トグル無効） |
| | Finish view | Auto assign (Go Back) |
| **Display** | 特段設定変更不要 |
| **Behavior** | 特段設定変更不要 |
| **Documentation** | 特段設定変更不要 |

7. 左側の [Actions] メニューを押下して、[Add]アクションを無効化（Position: Hide）する。これは、本システムにおいては、名刺の取り込みを `WorkspaceStudio` のみとし、`AppSheet` はあくまでも閲覧および既存データの編集のみを許可するためである。  
  <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/825cfc96-89f3-4bae-ae47-fa3b7ef95fc7" /> 
8. 左側の [Deploy]メニューを押下して、本システムをデプロイする。
   1. 「Run Deployment Check」を押下して、デプロイ前検査を行う。
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/57c6701c-54a2-4bc0-bf4f-03043de08518" />

> [!TIP]
> このとき、いくつかの項目でWarningが発生するが、それぞれ以下のWarningであるため、対応が必須なわけではない。
> * App description（アプリの基本情報）に関する警告: 「短いアプリの説明（short app description）の提供」、「対象業界（industry）の指定」、「アプリの機能・役割（function）の指定」の3項目の入力を求められており、[Settings] メニューの[Information] タブなどから入力が可能。
> * User Interface（ユーザーインターフェース）に関する警告: 「カスタムアプリアイコンが割り当てられていない（Custom app icon not assigned）」状態を示しており、デフォルトのアイコンのままでも動作はしますが、システムとして展開する際はオリジナルのアイコンを設定することが推奨される。
> * Content caching（モバイル端末でのコンテンツキャッシュ）に関する警告: 「アプリに画像やドキュメントデータが含まれており、オフラインデバイスキャッシュを利用するとメリットがある（The app has image or document data that would benefit from offline device caching）」と指摘されており、今回の名刺管理システムのように名刺画像を扱う場合、この設定を有効にすることで、画面表示の高速化やネットワークが不安定な場所での動作改善が見込める。

   2. 「Move app to deployed state」を押下して、デプロイする。  
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/90a55898-5467-4f64-b49a-56380017986d" />
   3. トップ画面に遷移するが正式にデプロイが完了するまで少々時間を要する。その後、再度デプロイチェック画面に遷移した際に以下の画面が表示されればデプロイが完了となり、PCやモバイルから利用ができる状態となる。
      <img width="1440" height="810" alt="Image" src="https://github.com/user-attachments/assets/070bb48a-ce53-4c24-87c7-3835e888cc69" />
