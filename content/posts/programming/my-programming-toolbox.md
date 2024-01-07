---
title: "My Programming Toolbox"
date: 2024-01-07T17:45:32+08:00
author: "cboy"
tags: ["Programming"]
draft: false
---

# Editor

## Shortcut

| Function (MacOS) | JetBrains IDEA | iTerm2 | Vim | Common Editor |
| --- | --- | --- | --- | --- |
| 上/up | Control + p | Control + p | k | Control + p |
| 下/down | Control + n | Control + n | j | Control + n |
| 左/left | Control + b | Control + b | h | Control + b |
| 右/right | Control + f | Control + f | l | Control + f |
| 行首/Move Caret to Line Start | Control + a | Control + a | ^ | Control + a |
| 行尾/Move Caret to Line End | Control + e | Control + e | $ | Control + e |
| 选中/Add Selection for Next Occurrence | Control + g |  |  |  |
| 全选/Select All Occurrences | Control + Command + g |  |  |  |
| 撤销/Undo | Command + z |  | u | Command + z |
| 返回撤销/Redo | Shift + Command + z |  | ctrl + r |  |
| 水平右侧分屏/ Split Right | Shift + Command + v |  |  |  |
| 全局查找/ Find in Files | Shift + Command + f |  |  |  |
| 往前/Forward | Command + ] |  |  |  |
| 后退/Back | Command + [ |  |  |  |
| 下一个方法/Next Method | Control + y |  |  |  |
| 重命名/Rename | Shift + ^ |  |  |  |
| 创建Java文件/New Java Class | Shift + Command + j |  |  |  |
| 创建文件/New File | Shift + Command + i |  |  |  |
| 创建文件夹/ New Directory | Shift + Command + p |  |  |  |

## Font

|  | JetBrains |
| --- | --- |
| Font | JetBrains Mono |
| Size | 18 |
| Line height | 1.4 |

## Live Templates(Jetbrains Editor Only)

```bash
############################################
##########          Log         ############
############################################
lgi = 'log.info("[$className$:$method$] $content$", "$arg$");'
lgw = 'log.warn("[$className$:$method$] $content$", "$arg$");'
lge = 'log.error("[$className$:$method$] $content$", "$arg$");'

############################################
##########          SQL         ############
############################################
ctable = 'create table `$table$`
(

    `create_time`               DATETIME(3)     NOT NULL DEFAULT CURRENT_TIMESTAMP(3) COMMENT '创建时间',
    `update_time`               DATETIME(3)     NOT NULL DEFAULT CURRENT_TIMESTAMP(3) ON UPDATE CURRENT_TIMESTAMP(3) COMMENT '更新时间',
    PRIMARY KEY (`id`) using btree,
) ENGINE = InnoDB
  DEFAULT CHARSET = utf8mb4
  COLLATE = utf8mb4_unicode_520_ci COMMENT $comment$;'
col = '`$col$`                  $type$       NOT NULL COMMENT '$comment$',$END$'
cid = '`id`                     BIGINT       NOT NULL AUTO_INCREMENT COMMENT 'id','
```

# Shell

## Alias

```bash
############################################
##########          Git         ############
############################################
alias gs='git status'
alias gp='f() { git push $1 $2 };f'
alias gpl='f() { git pull $1 $2 };f'
alias gc='f() { git commit -m "$1" };f'
alias gss='f() { git stash save $1 };f'
alias gsp='f() { git stash pop $1 };f'
alias gck='f() { git checkout $1 };f'
alias gckb='f() { git checkout -b $1 };f'
alias gm='f() { git merge $1 };f'
alias gl='git log'
alias gcp='f() { git cherry-pick $1 };f'
```

## Zsh

```bash
# oh-my-zsh
# zsh plugins (git and zsh-autosuggestion)
```

## Command

## Vim

.vimrc

```bash
set encoding=utf-8
set fileencoding=utf-8
syntax on
set number
set relativenumber
set ignorecase
set smartcase
```

## SSH

config

```
Host github.com
	HostName github.com
	Port 22
	User git
	IdentityFile ~/.ssh/id_rsa
Host *
	ServerAliveInterval 300
 	TCPKeepAlive=yes
    	ServerAliveInterval 60
    	ServerAliveCountMax 600
    	GSSAPIAuthentication yes
    	GSSAPIDelegateCredentials no
    	StrictHostKeyChecking no
    	HostKeyAlgorithms +ssh-rsa
    	PubkeyAcceptedKeyTypes +ssh-rsa
```