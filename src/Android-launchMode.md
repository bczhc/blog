# Android launchMode

## 语法

```xml
<activity android:launchMode=["standard" | "singleTop" | "singleTask" | "singleInstance"] ... > ... </activity>
```

## 基本行为

- `standard`（默认行为）：

  - 把目标Activity放在调用者Activity的栈顶

- `singleTask`：

  - 如果目标Activity没有在启动着，则让目标Activity启动在目标Activity所属App的任务栈栈顶（而不是在调用者Activity的任务栈中），并且把这整个任务栈都放在调用者Activity的任务线上方

  - 如果目标Activity已经在其所属App任务栈中，则不重新创建，而是把目标Activity上方所有Activity都清掉（不是杀死），使目标Activity在所属App任务栈最上方，然后再把这一整个任务栈都搬到调用者Activity任务栈上面

    （**唯一性**）

- `singleInstance`：

  - 如果目标Activity没有在启动着，则创建一个包含着目标Activity的任务栈，这个栈中只有这么一个Activity，再把此任务栈放在调用者Activity上方

  - 如果目标Activity已经启动着，则把它上面和下面的所有Activity都清除（不是杀死），确保那个任务栈中只有一个目标Activity，再把此任务栈放在调用者Activity上方

  - 如果在此目标Activity中又启动了Activity，由于“单一实例”的限制，只能把新启动的Activity放在其所属App中任务栈中，再把整个任务栈般过来放在原任务栈上方

    （**唯一性**、**独占性**）

- `singleTop`：

  - 和`standard`基本相同，只是在启动时如果发现任务栈顶已经是目标Activity了，则复用而不启动。

注：以上行为只考虑默认`taskAffinity`。

