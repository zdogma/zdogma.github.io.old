---
layout: post
title:  "Github Pages + Jekyll でハマったこと"
date:   2016-08-07 03:36:45 +0900
categories: Tech
---

はじまりは Processing の成果物を蓄積する場所としての Github Pages を想定していたが、
Github Pages が Jekyll を推奨していたこともあり、久々に触ってみることにした。

そうしたら思いの外ハマってしまったので、そのログを残しておく。

## 問題1: _layouts が見つからない

### 概要

したり顔で下記のように打ってみたのだが、
{% highlight bash %}
bundle install --path=vendor/bundle --jobs=4
bundle exec jekyll new .
{% endhighlight %}

下記のようなエラーが出てビルドできない。
{% highlight bash %}
$ bundle exec jekyll build
Configuration file: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_config.yml
            Source: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io
       Destination: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
     Build Warning: Layout 'post' requested in _posts/2016-08-07-welcome-to-jekyll.markdown does not exist.
  Liquid Exception: The included file '/Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_includes/icon-github.html' should exist and should not be a symlink in about.md
{% endhighlight %}

jekyll new した時に作られるべき `_layouts` が存在しない...？

### 解決方法
Github Pages 側の公式ドキュメントに導入方法があったので読んでみる。

[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)

{% highlight bash %}
bundle exec jekyll new . --force
New jekyll site installed in /Users/octocat/my-site.
{% endhighlight %}

force オプションが必要だったらしい。

なぜ必要だったかについてはまだ追えていないので、調べる。

## 問題2: Invalid date でフォーマットエラー

### 概要
またもビルドで落ちる。
{% highlight bash %}
$ bundle exec jekyll serve
Configuration file: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_config.yml
            Source: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io
       Destination: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
             ERROR: YOUR SITE COULD NOT BE BUILT:
                    ------------------------------------
                    Invalid date '<%= Time.now.strftime('%Y-%m-%d %H:%M:%S %z') %>': Document 'vendor/bundle/ruby/2.3.0/gems/jekyll-3.1.6/lib/site_template/_posts/0000-00-00-welcome-to-jekyll.markdown.erb' does not have a valid date in the YAML front matter.
{% endhighlight %}

なにやら gem の中身を読みに行っていてそこで落ちている？

### 解決方法
`_config.yml` に下記を追記する。
{% highlight bash %}
exclude: [vendor]
{% endhighlight %}


なんとか上記を踏まえて、ビルドできるようになった。

{% highlight bash %}
$ bundle exec jekyll serve --watch
Configuration file: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_config.yml
            Source: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io
       Destination: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.273 seconds.
 Auto-regeneration: enabled for '/Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io'
Configuration file: /Users/tomohiro.zoda/git/github.com/zdogma/zdogma.github.io/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
{% endhighlight %}

これでこちらの Github Pages も使えるようになったので、
はてなダイアリーとうまく棲み分けながら使っていきたい。


[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
