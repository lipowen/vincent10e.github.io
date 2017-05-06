---
title: tmux 配合 tmuxifier
date: 2017-04-17 22:38:12
tags:
---

工作時一整天都會碰到 terminal ，在還沒有使用 tmux 之前，總是會在不同的 tab 中做切換，自從現在使用 tmux 之後，感覺到效率有稍稍的提升一點。

tmux 可以非常簡單地透過 Homebrew 來安裝

```
$ brew install tmux
```

安裝好後，要開啟 tmux session，

```
$ tmux
```

目前自己的 tmux [設定](https://gist.github.com/vincent10e/1802b342048467dc5c6b8d44926b07a8) 是 follow thoughtbot 的 screen 課程，其中覺得最值得調整的是以下幾個設定。

* 如果在一個 window 中切了多了 pane，這個設定可以方便地在不同的 pane 之中切換

```
# act like vim
bind-key -n C-h select-pane -L
bind-key -n C-j select-pane -D
bind-key -n C-k select-pane -U
bind-key -n C-l select-pane -R
```


* 垂直以及水平切個出新的 pane

```
# split window
bind-key - split-window -v  -c '#{pane_current_path}'
bind-key \ split-window -h  -c '#{pane_current_path}'
```

* 調整 pane 之間的大小，配合 shift 或 control 就能做到小幅度或者是大幅度的調整。

```
# Fine adjustment (1 or 2 cursor cells per bump)
bind -n S-Left resize-pane -L 2
bind -n S-Right resize-pane -R 2
bind -n S-Down resize-pane -D 1
bind -n S-Up resize-pane -U 1

# Coarse adjustment (5 or 10 cursor cells per bump)
bind -n C-Left resize-pane -L 10
bind -n C-Right resize-pane -R 10
bind -n C-Down resize-pane -D 5
bind -n C-Up resize-pane -U 5
```

## 再進階一點點

上面的設定都是能夠讓我們方便的將 pane 調成自己想要的設定，但如果今天 session 重開就要重新再切個 pane，然後再調整 pane 大小為自己想要的樣子，實在是不符合懶惰的原則，為了能夠達成恢復  tmux session，可以配合 [tmuxinator](https://github.com/tmuxinator/tmuxinator) 一起使用。

安裝完之後，使用 tmuxinator 建立新的 project。

```
$ tmuxinator new rails_demo_project
```


`~/.tmuxinator/sample.yml`，這邊做一個簡單的範例檔，當使用 tmuxinator 時會開啟一個 tmux window，在這個 window 中有兩個 panes，一個 pane 會執行 `rails server`，而另外一個則是執行 `rails console`。

```
# ~/.tmuxinator/rails_demo_project.yml

name: rails_demo_project
root: ~/rails_demo_project

windows:
  - develop:
      layout: main-vertical
      panes:
          - server:
              - bundle exec rails server
          - console:
              - bundle exec rails console


```


之後開發時，只要使用 tmuxinator，就能快速的開啟預先定義好的 session 畫面。

```
$ tmuxinator start rails_demo_project
```