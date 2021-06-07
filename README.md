Ansible Playbooks
====
WindowsユーザとGoogle Workspacesにユーザを作成するAnsible playbook置き場です。  
今まで使用していたAnsible版と分けたのは念のためです。  
  
以下の環境で確認しています。  
  
* Amazon Linux2
* Requires
    * Ansible 2.10.8 or newer
    * Python 3.7.9 or newer
  
## Playbookの使い方
  
* ansibleをインストールする。
* git cloneする。  
`git clone ssh://git@github.com/akashima/ansible-windows.git`
* 環境変数を設定する。  
`source env.sh`
* useraddのタスクならgroup_vars/main.ymlを修正し、usergetであればroles/userget/vars/main.ymlを修正します。  
* 実行する  
例1: ADやGCPにユーザを追加する場合  
`ansible-playbook -vv -i windows useradd.yml -k`  
例2: ADに登録されている個別のユーザ情報を取得する場合  
`ansible-playbook -vv -i windows userget.yml -k`  

## 機密情報の取り扱いについて

機密情報を取り扱う場合は、必ず暗号化しましょう！
暗号化対象例：password, accesskey/secretkey, 秘密鍵 ...etc

暗号化するvarsを別ファイルに切り出し、暗号化を行います。

* 暗号化  
`ansible-vault encrypt 対象ファイル`
* 複合  
`ansible-vault decrypt 対象ファイル`
* 暗号化したvarsを外部変数として読み込ませる例  
例:`ansible-playbook -vv -i windows windows.yml --extra-vars="@暗号化された.yml" --ask-vault-pass`

参考サイト:[Ansibleでのパスワードの取り扱い](http://qiita.com/jimaoka/items/535d5feb9b99fe5b3318)

## 変数の説明
group_vars/main.yml にサンプルを載せていますが、項目毎の説明は以下のようになります  

### 変数の項目単位の説明
以下は変数の項目単位の説明になります。  

```
dirsync: true or falseでWindows ServerにインストールされているDirectorySyncを使用するかどうかを選びます。(true = syncする)
userid: メールのユーザ名
password: 対象の人が使う初期パスワードです。
domainname: ドメインを指定します。
email: メールアドレスをそのまま指定します。
firstname: フルネームの名前部分を入力します。
firstname_eng: フルネームを英語で名前部分を入力します。
lastname: フルネームの氏名部分を入力します。
lastname_eng: フルネームを英語で氏名部分を入力します。
description: 社員IDとしての連番か案件名＋相手会社名を入力します。
groups: グループ名をリスト形式で列挙します。まとめてグループ参加させられます。
orgunit: Googleの組織にある子組織名を入力します。
```

### 変数のサンプル
以下はサンプルコードでパスワードは適当です。  
```
  - userid: "YourID"
    password: "password is here"
    domainname: "your.domain.com"
    email: "YourID@your.domain.com"
    firstname: "YourFirstName"
    firstname_eng: "English Your first name"
    lastname: "YourLastNamae"
    lastname_eng: "Your last name"
    description: "Your Description"
    groups: ["AD Group Name 1","AD Group Name 2","AD Group 3", "AD Group 4"]
    orgunit: "/GoogleOrgName"
```
