# rop-machine

rop machineはROP (Return Oriented Programming)の練習用のツールです。
接続すると以下のようなメニューが表示されます。

```
[menu]
1. append hex value
2. append "pop rdi; ret" addr
3. append "system" addr
8. show menu (this one)
9. show rop_arena
0. execute rop
>
```

1～3がプログラムに新たな命令を追加するコマンドになっています。
また、9がプログラムを実行するコマンドです。

例えば、
1. 3を1回押してsystemのアドレスを追加
2. 2を3回押して"pop rdi; ret"のアドレスを追加
3. 9でプログラム全体を表示

とすると以下の通りとなります。

```
> 3
"system" is appended
> 2
"pop rdi; ret" is appended
> 2
"pop rdi; ret" is appended
> 2
"pop rdi; ret" is appended
> 9
     rop_arena
+--------------------+
| system             |<- rop start
+--------------------+
| pop rdi; ret       |
+--------------------+
| pop rdi; ret       |
+--------------------+
| pop rdi; ret       |
+--------------------+
```

このrop_arenaの下に表示されているのがROPで作られたプログラム全体です。
コマンドとして0を入力すると`rop start`の位置の命令が実行されます。
ちなみに上をそのまま実行するとsystemの位置でsegmentation faultで落ちます。
うまくROPのプログラムが組まれているとシェルを呼び出すことができます。

任意のアドレスや値を入れたい場合には`1. append hex value`を使います。
以下が`0xdead`と`0xbeaf`の値を追加した例です。

```
> 1
hex value?: 0xDEAD
0x000000000000dead is appended
> 1
hex value?: 0xBEAF
0x000000000000beaf is appended
> 9
     rop_arena
+--------------------+
| 0x000000000000dead |<- rop start
+--------------------+
| 0x000000000000beaf |
+--------------------+
```

この状態で0で実行すると当たり前ですがsegmentation faultで落ちます。
