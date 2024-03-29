---
title: "Separation of concerns"
date: 2022-12-15T11:51:01+09:00
---

# はじめに
[前回の記事](https://ntk221.github.io/posts/how_to_build_blog/)では，テストファーストのやり方で関数を開発するということをしました。結果として以下のようなコードが出来上がりました。

```
#include <stdbool.h>
#include <stdio.h>
#include <limits.h>

int my_abs(int x)
{
    if (INT_MIN)
        return (-1);
	if (x < 0)
        return (-x);
    else
        return (x);
}

void test(void)
{
	bool result = (my_abs(-100) == 100) && (my_abs(100) == 100);

    	bool special = (my_abs(INT_MIN) == -1) && \
                        (my_abs(INT_MAX) == INT_MAX) && \
                        (my_abs(0) == 0);

	if (result)
    		puts("ok");
	else
		puts("ko");
}

int main(void)
{
	test();
}
```

しかし,前回のコードにはちょっとした問題の芽が潜んでいます。それは，**テストの対象となるコードと，テスト用のコードが同一のファイルに含まれている**という点です。今のやり方でコードを管理していると，テスト対象となるコードが，テスト用のコードを呼び出すといった意図しない呼び出し関係が生じてしまうかもしれません。

# 関心ごとの分離
前回の記事に書いたように，ソフトウェアはそれぞれが固有の働きをする複数のパーツからなると考えることができます。この時非常に重要なのは，**各パーツは一つの仕事を担当するように設計する**，ということです。各パーツの仕事を明確にして管理しなければ，先ほど確認したようなコード間での意図しない結合が起きてしまったり,どのファイルにどの関数が含まれるのかわからなくなってしまうからです。このような設計指針で設計されたソフトウェアのパーツを**単一責務**であると言います。ところで，ソフトウェアのパーツのことは一般に**モジュール**と呼ばれるので，今後は我々もモジュールという言葉を使うことにしましょう。

テストと開発コードはそれぞれ異なる仕事をする関数です。従って，これらのコードを含むモジュールが単一責務を満たすようにするためには，これらは異なるモジュールに分離する必要があります。このように単一責務を満たすために，モジュールを管理しやすい単位に分割することを関心ごとの分離，と言います。今回は`my_abs()`と，`test()`を違うファイルに分割すれば良いことになります。それぞれ，`my_abs.c`,`test.c`というファイルを作って分離しましょう。

./my_abs.c
```
int my_abs(int num)
{
    ...
}
```

./test.c
```
void test(void)
{
    ...
}

int main(void)
{
    test();
    return (0);
}
```

# 依存性の注入
このままでは，`test.c`は単独でコンパイルすることができません。なぜなら,`test()`関数は,`my_abs()`を呼び出しているという点で`my_abs()`関数に依存しているからです。このことをファイルの中で表現し，ファイル単独でコンパイルできるようにするには，`my_abs()`の
プロトタイプ宣言を含むヘッダーファイルを用意して，それをファイルの先頭でインクルードすれば良いです。以下のようになります。

./include/my_abs.h
```
#ifndef MY_ABS_H
#define MY_ABS_H

int my_abs(int num);

#endif
```

test.c
```
#include "include/abs.h"

...

```

これで，テストコードの開発コードに対する依存性を表現することができました。

# Makefile
C言語では，Makefileというビルドツールを使うことで，ビルド過程を簡略化することができます。以下のように超簡単なMakefileを用意しましょう。

./Makefile
```
test:
    gcc *.c -o test
    ./test
    rm test
```

これで，test用のコマンドを作ることができました。`make test`とコマンドを打ってみましょう。

![](/images/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202022-12-15%2014.14.05.png)

すごくいい感じですね！

最後に，`test.c`と,`my_abs.c`が異なるモジュールに属することを明示するために，フォルダ構成を整えましょう。

```
$> tree
.
├── Makefile
├── include
│   └── my_abs.h
├── srcs
│   └── my_abs.c
└── tests
    └── test.c
```

これに合わせて，Makefileも多少変更しましょう。

```
SRCS	=	./srcs/*.c

TESTS	=	./tests/*.c

CC	=	gcc

.PHONY:	test
test:
	$(CC) $(SRCS) $(TESTS) -o test
	./test
	rm test
```
よし，これで上手に関心事を分離したディレクトリ構成になりましたね！

# おわりに
今回も最後まで読んでいただきありがとうございました。次回はいよいよ Github Actions を使ったテストの自動化について書きたいと思います！ではでは〜。

### **参考文献**
- [組込みソフトウェア開発のための構造化プログラミング](https://www.shoeisha.co.jp/book/detail/9784798147611)
- [Learning Test-Driven Development](https://www.oreilly.com/library/view/learning-test-driven-development/9781098106461/)
