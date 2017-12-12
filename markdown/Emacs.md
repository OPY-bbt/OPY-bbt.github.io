# Emac  使用简介

## 按键定义(mac 下)
- M(eta) option
- s(uper) commond
- S(hift)
- C(trl)

## 光标操作
- C-f (forward) 向前一个字符（character）
- C-b (backward) 向后一个字符
- M-f 向前一个单词（word）
- M-b 向后一个单词
- C-p (previous) 向前一行
- C-n (next) 向后一行
- C-a (ahead) 跳到行首
- C-e (end) 跳到行尾
- M-a 跳到句首（sentence）
- M-e 跳到句尾

## 删除操作
- \<del> 删除光标前一个字符
- C-d 删除光标后一个字符
- M-\<del> 删除光标前一个单词
- M-d 删除光标后一个单词
- C-k 删除光标到行尾之间
- M-d 删除光标到句尾之间

## 撤销
- C-y 撤销 C-k 删除的行 but 我感觉更像粘贴
- M-y 可以看到 C-y 保存记录
- C-/, C-_ 撤销
- M-_ redo

## 屏幕操作
- M-v or Esc v 跳到上一屏 
- C-v (view) 跳到下一屏
- M-< 跳到屏首 (也许需按shift)
- M-> 跳到屏尾
- C-l (line) 将光标所在位置放置在屏幕(中间，上部，下部)
- C-x [number] 保留 window 个数
  - C-x 1: One window (i.e., kill all other windows)

## 文件操作
- C-x C-f 打开文件
- C-x C-s 保存当前缓冲区

## 前缀命令
- C-x 一般用于文件操作
- C-h (help) 
  - C-h key [commond] 查看 commond's manual 
- C-c 与某些特殊的编辑模式有关
- C-g 用于终端取消或中断之前的指令
- C-u [number] [command] prefix argument 重复number次commond
  - C-u 8 * 输入8次*

## buffer 类似于便签页
- C-x C-b list buffers
- C-x b swtich buffer quickly
- C-x s save some buffers

## quit
- C-x C-c 退出并且保存

## auto save
- when computer crashed, emacs auto save file which you editing, the temporary file name is start with # and end with #;

## switch major mode
- M-x i.e. M-x text-mode

## search
- C-s forward search
- C-r backward search

## window
- C-x o (other) move cursor to other window