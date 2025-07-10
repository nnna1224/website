---
title: Arch Linux
authors: nnna
date: 2025-07-10
---

# WSLにおけるArch Linuxの初期設定
## pacman-keyの初期化と鍵の設定
```
pacman-key --init
pacman-key --populate archlinux
```

## デフォルトエディタの設定
```
export EDITOR="$(which nvim)"
```

## reflectorでミラーリストを最適化
```
reflector --country Japan --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

## ユーザ設定
以下のコマンドでrootユーザのパスワードを設定する．
```
passwd
```

続いて，以下のコマンドで一般ユーザを追加する．<br/>
このとき，`sudo`を使用できるようwheelグループに追加しておく．<br/>
※ `{USERNAME}`は任意のユーザ名に置き換える．
```
useradd -m -G wheel -s /bin/zsh -d /home/{USERNAME} {USERNAME}
passwd {USERNAME}
```
:::info[オプションの説明]
* -m： ホームディレクトリ作成
* -G グループ名： グループに追加
* -s シェルのパス： デフォルトシェルを指定
* -d ホームディレクトリのパス： ホームディレクトリのパスを指定
:::

## sudo権限の付与
以下のコマンドを実行してエディタを開き，`/etc/sudoers`ファイル内の`%wheel ALL=(ALL:ALL) ALL`のコメントアウトを解除する．
```
sudoedit /etc/sudoers
```

## ロケールの設定
以下のコマンドを実行してエディタを開き，`ja_JP.UTF-8 UTF-8`のコメントアウトを解除．
```
sudoedit /etc/locale.gen
```

保存してエディタを抜け，以下を実行する．
```
locale-gen
echo LANG=ja_JP.UTF-8 > /etc/locale.conf
```

## WSL特有の設定
`/etc/wsl.conf`に以下を記述する．<br/>
※ `{USERNAME}`は先ほど作成したユーザ名に置き換える．
```
[boot]
systemd = true

[user]
default={USERNAME}
```

## zshの設定
Arch Linuxを一度閉じてから再度開き直す．<br/>
あるいは，`su - {USERNAME}`を実行して，ユーザを切り替える．

`{USERNAME}`で設定したアカウントに切り替わるとzshの設定を対話的に聞かれるため，答えていく．

## yayのインストール
AUR(Arch User Repository)インストールのため，以下のコマンドを実行してヘルパーであるyayをインストールしておく．
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
cd ..
rm -rf yay
```

---

以上でWSLにおけるArch Linuxの設定は完了である．<br/>
以降はSSH設定などを行う．
