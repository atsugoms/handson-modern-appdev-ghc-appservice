# 前提環境の構築

- GitHub Copilot
- GitHub Codespaces



#### ⏳ 推定時間

- 3 ~ 5分

#### 💡 学習概要

本ハンズオンでは単純な仮想マシンを保護対象とし、さまざまなセキュリティ対策を行っていきます。
準備する環境は以下のような単純な環境です。

![](../images/ex00/0000-env.png)

「ARMテンプレート」 または 「terraform」 を利用してデプロイが可能です。
どちらかの方法でデプロイを行います。

#### 🗒️ 目次

- WSL
- Docker
- Visual Studio Code
- Node.js



## WSLインストール

1. PowerShell を管理者で起動

1. Linux 用 Windows サブシステムを有効化

    ```
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

1. 仮想マシン機能を有効化

    ```
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

1. WSLインストール

    ```
    wsl --install ubuntu-24.04
    ```

1. ウィザードに従って新規ユーザーを作成

    - user account
    - password

1. 自動的にUbuntuへログイン



(*) 参考: コマンド

- WSLインストールされたディストリビューション一覧

    ```
    wsl --list --verbose
    ```

- 指定のディストリビューションを起動してログイン

    ```
    wsl --distribution <DISTRIBUTION_NAME> --user <USER_NAME>
    ```

    例： `root` ユーザーでデフォルトディストリビューションの `~` ユーザーディレクトリへログイン

    ```
    wsl --user root --cd ~
    ```

(*) 参考: リンク
- [以前のバージョンの WSL の手動インストール手順](https://learn.microsoft.com/ja-jp/windows/wsl/install-manual)
- [WSL を使用して Windows に Linux をインストールする方法](https://learn.microsoft.com/ja-jp/windows/wsl/install)


## WSL (Ubuntu) にツールインストール

### Docker (Linux版) インストール

1. PowerShell を立ち上げて、WSL (Ubuntu) にログイン

    ```
    wsl ~
    ```

1. `apt` 更新

    ```
    # Add Docker's official GPG key:
    sudo apt-get update
    sudo apt-get install ca-certificates curl
    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    # Add the repository to Apt sources:
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    ```

1. Dockerパッケージをインストール

    ```
    sudo apt-get install -y \
        docker-ce \
        docker-ce-cli \
        containerd.io \
        docker-buildx-plugin \
        docker-compose-plugin
    ```

1. `sudo`なしで実行できるよう設定

    ```
    sudo groupadd docker
    sudo gpasswd -a $USER docker
    sudo systemctl restart docker
    ```

1. Ubuntu 再起動

    ```
    sudo shutdown -r now
    ```

1. 再度 WSL (Ubuntu) へログイン

    ```
    wsl ~
    ```

1. 動作確認

    テストコンテナの起動

    ```
    docker run hello-world
    ```

    不要イメージ、コンテナの削除

    ```
    docker system prune -af
    ```

(*) 参考: リンク

- [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)


### Node.js インストール

1. Node.js をインストール

    ```
    sudo apt update
    sudo apt install nodejs npm
    ```

1. 動作確認

    ```
    npm --version
    ```

### az cli インストール

1. az cli をインストール

    ```
    sudo curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    ```

1. 動作確認

    ```
    az --version
    ```

### gh コマンド インストール

1. gh コマンドをインストール

    ```
    (type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
        && sudo mkdir -p -m 755 /etc/apt/keyrings \
        && out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
        && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
        && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
        && sudo mkdir -p -m 755 /etc/apt/sources.list.d \
        && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
        && sudo apt update \
        && sudo apt install gh -y
    ```

1. 動作確認

    ```
    git --version
    gh --version
    ```

1. git 初期設定

    ```
    git config --global user.name "<GITHUB_ACCOUNT_NAME>"
    git config --global user.email "<GITHUB_ACCOUNT_EMAIL>"
    ```

### (参考) WSL コマンド

- `root`ユーザーでログイン

    ```
    wsl --user root --cd ~
    ```


## Visual Stdio Code インストール

1. 以下のページへアクセス

    https://code.visualstudio.com/download

1. 環境にあわせたインストーラーをダウンロード

1. インストール


## GitHub Copilot の設定

1. 画面右下のCopilotアイコンを開き「Copilotの設定」を選択

1. 「GitHubで続行する」を選択し、Copilotのライセンスがあるアカウントで GitHub へログイン


## Visual Studio Code 拡張機能の追加

1. 以下の拡張機能を追加

    - [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    - [GitHub Actions](https://marketplace.visualstudio.com/items?itemName=github.vscode-github-actions)
<!--
     - [Container Tools](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-containers)
-->

## 開発ツール


