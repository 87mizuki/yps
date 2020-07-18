# task_1
## EC2インスタンス作成～SSH接続
①AWSでEC2インスタンスを作成  
【元ツイ】https://twitter.com/yotaro__ok/status/1284115619034484737

②TeraTermからSSHで接続
┗ユーザー名：centos  
┗ポート：（初期値）22  
┗秘密鍵：任意の保存先とファイル名　「～.pem」  
　【参考】https://www.dot-plus.com/cloud-service/aws/2935/#TeraTermSSH  


## rootユーザーのSSHログイン禁止＆ポート番号変更　※セキュリティのため
### ①TeraTermでインスタンスに接続した状態で`sudo setenforce 0`
　→selinuxを無効化  

### ②`sudo vi /etc/selinux/config`
「SELINUX=enforcing」を  
「SELINUX=disabled」に変更  
　→selinuxを恒久的に無効化  

※viコマンド「:wq」（保存して終了）    
※Linux標準教科書の85ページにviの操作方法が載っています  
※チートシート：　https://vim.rtorr.com/lang/ja  

### ③`getenforce`→Permissiveが返ってくるかチェック
※①の後は、②で保存できていなくてもPermissive返ってくるので注意

### ④AWSでセキュリティグループID→インバウンドルールを編集→ルールの追加
【元ツイ】https://twitter.com/yotaro__ok/status/1284155021500674049

ポート番号について  
【元ツイ】https://twitter.com/yotaro__ok/status/1284323605921165312  
【元ツイ】https://twitter.com/yotaro__ok/status/1284324747879124992  

### ⑤`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.org`
sshd_configを複製  
.org　＝　「（変更前の元ファイルとか）オリジナルのファイルだよ～」ということを表現したいときによく付ける拡張子

### ⑥`sudo vi /etc/ssh/sshd_config`
「#Port 22」→「Port ルールの追加で入力した番号」  
「#PermitRootLogin yes」→「PermitRootLogin no」  

※ 「#」コメントアウトはずす

### ⑦`sudo sshd -t`　※エラーが表示されないことを確認

### ⑧`sudo systemctl restart sshd`　※sshd再起動
【元ツイ】https://twitter.com/yotaro__ok/status/1284187432380817408

### ⑨ターミナルをもう１つ起動して新しいポート番号でログインできるか確認
### ⑩インバウンドルールからSSHを削除、ルールの保存
### ⑪`sudo yum update -y`　アップデート　※少し時間がかかります
### ⑫`exit` でターミナル終了
※無料枠の間はEC2インスタンス起動しっぱなしでも課金されない
