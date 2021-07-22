### 自己较常用的一些screen操作

#### 基本窗口操作

**create:** screen -S \<session-name\>

**reattach:** screen -dr \<session-name\>

**list:** screen -ls

**detach:**

```
       ───────────────────────────────────────────────────────────────────────
       C-a d              (detach)          Detach screen from this terminal.
       ───────────────────────────────────────────────────────────────────────
```

#### 多视图

```
       ───────────────────────────────────────────────────────────────────────
       C-a S              (split)           Split the current region horizon‐
                                            tally into  two  new  ones.   See
                                            also only, remove, focus.
       ───────────────────────────────────────────────────────────────────────
       C-a |              (split -v)        Split the current  region  verti‐
                                            cally into two new ones.
       ───────────────────────────────────────────────────────────────────────
       C-a tab            (focus)           Switch the  input  focus  to  the
                                            next region.  See also split, re‐
                                            move, only.
       ───────────────────────────────────────────────────────────────────────
       C-a digit          (select 0-9)      Switch to window number 0 - 9
       ───────────────────────────────────────────────────────────────────────
       C-a c,             (screen)          Create  a new window with a shell
                                            and switch to that window.
       ───────────────────────────────────────────────────────────────────────
       C-a '              (select)          Prompt  for a window name or num‐
                                            ber to switch to.
       ───────────────────────────────────────────────────────────────────────
       C-a "              (windowlist -b)   Present a list of all windows for
                                            selection.
       ───────────────────────────────────────────────────────────────────────
       C-a Q              (only)            Delete  all  regions but the cur‐
                                            rent one.  See  also  split,  re‐
                                            move, focus.
       ───────────────────────────────────────────────────────────────────────
```

若要在reattach之后仍然保持布局，在已attach上screen的时候进行：

1. 

```
       ───────────────────────────────────────────────────────────────────────
       C-a :              (colon)           Enter command line mode.
       ───────────────────────────────────────────────────────────────────────
```

2. 输入`layout save default`命令并回车

#### 更多窗口操作

**kill:**

```
       ───────────────────────────────────────────────────────────────────────
       C-a k              (kill)            Destroy current window.
       ───────────────────────────────────────────────────────────────────────
```

**进入光标移动模式（可上下浏览滚动）：**

```
       ───────────────────────────────────────────────────────────────────────

       C-a [,             (copy)            Enter copy/scrollback mode.
       C-a esc
       ───────────────────────────────────────────────────────────────────────
```

**退出光标移动模式：**

<kbd>esc</kbd>/<kbd>q</kbd>

