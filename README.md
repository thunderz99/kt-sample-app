# kotlinプログラムをvscodeで開発・デバッグする

## TL;DR

fwcd.kotlinのextensionをインストールすること

## 概要

多くのQiita記事では、kotlinのプログラムをVSCodeで実行することを説明したが、肝心なデバッグを触れていない。

自分で試し成功した結果を皆さんに共有できれば。(基本Mac向けだが、ほとんどはwindowsと共通する)


## 前提条件

* vscode
* Java(OpenJDK)

上記のインストール手順は省略


## 構築手順

### 1. kotlin と gradle


```shell
% brew install kotlin
% brew install gradle
```

### 2. kotlin sample appの作成

```shell
% mkdir kt-sample-app
% cd kt-sample-app
% gradle init --type=kotlin-application

# 出てくる選択肢は全部デフォルトでOK

# 以下のフォルダ構造が自動生成される
% tree .
.
├── build.gradle.kts
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle.kts
└── src
    ├── main
    │   ├── kotlin
    │   │   └── kotlin
    │   │       └── sample
    │   │           └── app
    │   │               └── App.kt
    │   └── resources
    └── test
        ├── kotlin
        │   └── kotlin
        │       └── sample
        │           └── app
        │               └── AppTest.kt
        └── resources
 
# 疎通する
% ./gradlew build

BUILD SUCCESSFUL in 3s
8 actionable tasks: 8 executed
```

### 3. VSCodeの拡張のインストール

```
# 以下のvscode extensionの設定を行う

% mkdir -p .vscode
% vi .vscode/extensions.json

-----以下の内容を入力-----
{
    "recommendations": [
        "fwcd.kotlin",
        "vscjava.vscode-java-pack"
    ]
}

# ここからvscodeを起動
% code .

```

「recommended extensionをインストールしますか」のダイアログが表示されるので、
「Install All」でextensions.jsonに書いた２つのextensionをインストールする

ここまでApp.ktファイルを開けば、syntax highlightとauto compleletionが効くようになる

### 4. ビルド・デバッグの設定

```
% vi .vscode/tasks.json

-----以下の内容を入力-----

{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            "command": "./gradlew build -x test",
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "run",
            "type": "shell",
            "command": "./gradlew run"
        },
        {
            "label": "test",
            "type": "shell",
            "command": "./gradlew test"
        },
        {
            "label": "clean",
            "type": "shell",
            "command": "./gradlew clean",
            "problemMatcher": []
        },
        {
            "label": "check",
            "type": "shell",
            "command": "./gradlew check",
            "problemMatcher": []
        }
    ]
}

```

vscodeのメニュー Terminal -> Run Task... で runなどを選ぶ。
これで、 gradlewのbuild / run / check / clean / testのタスクを実施できる

最後は、デバッグの設定

```
% vi .vscode/launch.json

-----以下の内容を入力-----

{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "kotlin",
            "request": "launch",
            "name": "Kotlin Launch",
            "projectRoot": "${workspaceFolder}",
            "mainClass": "kt/sample/app/AppKt",
            "preLaunchTask": "build"
        }
    ]
}

```

※ mainClassのところは自分のAppに合わせて書き換えてください。

適当にbreakpointを設定して、vscodeのDebugタブ -> Runボタンでデバッグができる






