Mainly for testing

Git とか Jekyll とか知らない人によるメモ帳。簡単なものしかないです。

## html に Markdown を埋め込む

### 1. _layouts フォルダーと .html ファイルを作る

例:

- myhttps.github.io（リポジトリ）
  - _layouts
    - default.html

.html ファイルの名前は何でもいいみたいです（経験上）。私が見た解説サイトでは「default.html」を使っていました。

[.html ファイルの内容](https://github.com/myhttps/myhttps.github.io/blob/master/_layouts/default.html)（例）:

```
<!DOCTYPE html>
<html lang="ja">
<head>
<title>testl0</title>
</head>
<body>
  {{ content }}
</body>
</html>

```

「{{ content }}」のところに Markdown が埋め込まれます。

### 2. 適当な場所に .md ファイルを作る

例:

- myhttps.github.io（リポジトリ）
  - article
    - index.md

.md ファイルの名前は「**index.md**」推奨（私的）。URL が「<ユーザー名>.github.io/<index.md の親フォルダー名>/」になるからです。（例えば、例の場合は「myhttps.github.io/article/」になる。）

[.md ファイルの内容](https://github.com/myhttps/myhttps.github.io/blob/master/article/index.md?plain=1)（例）:

```
---
title: testl1
layout: default
---

# Welcome to Example!

No information

```

「title: testl1」って書いてるけど、これはどこに表示されるのかわからん。解説サイトを真似しただけです。別に書かなくても動作しました。

「layout: default」の「default」の部分には、手順 1 で作った .html ファイルの名前（拡張子抜き）を書きます。

あとは Action 待ちです。[こちらが完成したページです](https://myhttps.github.io/article/)。手順 1 の default.html で .css とか指定していないのでまっさらですね状態。

あとは頑張ってください（投げやり）

## GitHub Pages のトレイリング スラッシュを覆滅する

URL 末尾のスラッシュ（/）ですね。海外の解説サイトを真似しただけです。

### .html にコードを貼り付ける

URL が https://myhttps.github.io/experiment/00/ になる場合、/experiment/00/index.html の先頭にこのコードを書きます。

```
---
permalink: /experiment/00
---

```

[全体を見る](https://github.com/myhttps/myhttps.github.io/blob/master/experiment/00/index.html)とこんな感じ

```
---
permalink: /experiment/00
---

<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="utf-8">
  <title>Hello, world! 00</title>
</head>
<body>
  <p>experiment</p>
</body>

```

これで https://myhttps.github.io/experiment/00 にアクセスしても、スラッシュありの URL にリダイレクトされなくなりました。パチパチ

ただ、これでは /experiment/00/ と /experiment/00/index.html にアクセスしたときに 404 になってしまいます。次はこれを解決しましょう。

### 404.html にコードを貼り付ける

例:

- myhttps.github.io（リポジトリ）
  - 404.html

404.html にこのコードを貼り付けます。

```
<script>
  const PTH = window.location.href;
  if (PTH.endsWith('/')) {
    window.location.href = PTH.slice(0, -1);
  } else if (PTH.endsWith('/index.html')) {
    window.location.href = PTH.slice(0, -11);
  }
</script>
```

「URL の末尾に / が含まれていたら 1 文字消す」、「URL の末尾に /index.html が含まれていたら 11 文字消す」ってコードだと思います。

コード内の「PTH」は多分改変できます

```
<script>
  const fourzerofour = window.location.href;
  if (fourzerofour.endsWith('/')) {
    window.location.href = fourzerofour.slice(0, -1);
  } else if (fourzerofour.endsWith('/index.html')) {
    window.location.href = fourzerofour.slice(0, -11);
  }
</script>
```

[全体を見る](https://github.com/myhttps/myhttps.github.io/blob/master/404.html)とこんな感じ

```
<!DOCTYPE html>
<html lang="ja">
<head>
  <script>
    const PTH = window.location.href;
    if (PTH.endsWith('/')) {
      window.location.href = PTH.slice(0, -1);
    } else if (PTH.endsWith('/index.html')) {
      window.location.href = PTH.slice(0, -11);
    }
  </script>
  <meta charset="utf-8">
  <title>Welcome to 404</title>
</head>
</html>

```

https://myhttps.github.io/experiment/00/ と https://myhttps.github.io/experiment/00/index.html にアクセスしてみましょう。スラッシュなしの URL にリダイレクトしれくれました。

Markdown 埋め込みとスラッシュ覆滅って併用できるんですかね？
