Mainly for testing

Git とか Jekyll とか知らない人によるメモ帳。簡単なものしかないです。

## 目次

- [html に Markdown を埋め込む](#html-%E3%81%AB-markdown-%E3%82%92%E5%9F%8B%E3%82%81%E8%BE%BC%E3%82%80)
- [GitHub Pages のトレイリング スラッシュを覆滅する](#github-pages-%E3%81%AE%E3%83%88%E3%83%AC%E3%82%A4%E3%83%AA%E3%83%B3%E3%82%B0-%E3%82%B9%E3%83%A9%E3%83%83%E3%82%B7%E3%83%A5%E3%82%92%E8%A6%86%E6%BB%85%E3%81%99%E3%82%8B)

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
<title>{{ page.title }} - testl0</title>
</head>
<body>
  {{ content }}
</body>
</html>

```

`{{ content }}` のところに Markdown が埋め込まれます。

### 2. 適当な場所に .md ファイルを作る

例:

- myhttps.github.io（リポジトリ）
  - article
    - index.md

.md ファイルの名前は「**index.md**」推奨（私的）。URL が `<ユーザー名>.github.io/<index.md の親フォルダー名>` になるからです。（例えば、例の場合は `myhttps.github.io/article` になる。）

[.md ファイルの内容](https://github.com/myhttps/myhttps.github.io/blob/master/article/index.md?plain=1)（例）:

```
---
title: testl1
layout: default
---

# Welcome to Example!

No information

```

~~「title: testl1」って書いてるけど、これはどこに表示されるのかわからん。解説サイトを真似しただけです。別に書かなくても動作しました。~~

.md に書いた title は、手順 1 で作った .html の `{{ page.title }}` に挿入されるようです。.html のどこにでも挿入されるので、`<meta proparty="og:title" content="{{ page.title }} - Site Name">` みたいな感じでも使える。

`layout: default` の `default` の部分には、手順 1 で作った .html ファイルの名前（拡張子抜き）を書きます。

あとは Action 待ちです。[こちらが完成したページです](https://myhttps.github.io/article/)。手順 1 の default.html で .css とか指定していないのでまっさらですね状態。

**注:** 手順 2 で .md ファイルの名前を index.md にしてないとき、または URL で拡張子まで表示したいときは、URL は `<.md ファイルの名前>.md` ではなく `<.md ファイルの名前>.html` になります。

- https://myhttps.github.io/article/index.md
- https://myhttps.github.io/article/index.html

CSS とかは、あとは頑張ってください（投げやり）

## GitHub Pages のトレイリング スラッシュを覆滅する

URL 末尾のスラッシュ（/）ですね。海外の解説サイトを真似しただけです。

### .html にコードを貼り付ける

例:

- myhttps.github.io（リポジトリ）
  - experiment
    - 00
      - index.html

URL が https://myhttps.github.io/experiment/00/ になる場合、/experiment/00/index.html の先頭にこのコードを書きます。

```
---
permalink: /experiment/00
---

```

「myhttps.github.io」から先を書きます。最後にスラッシュを入れません。

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
<body>
  <p>Welcome to 404</p>
</body>
</html>

```

https://myhttps.github.io/experiment/00/ と https://myhttps.github.io/experiment/00/index.html にアクセスしてみましょう。スラッシュなしの URL にリダイレクトしてくれました。

Markdown 埋め込みとスラッシュ覆滅は[併用できました](https://github.com/myhttps/myhttps.github.io/blob/master/article/2024013000/index.md?plain=1)。（あと default.html 以外の layout を指定してみたり）

https://myhttps.github.io/article/2024013000

```
---
title: Heiyou
layout: custom
permalink: /article/2024013000
---

# Is it another layout?

No information

```
