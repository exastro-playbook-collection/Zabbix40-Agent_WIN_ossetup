# Ansible Role: Zabbix40-Agent_WIN_ossetup

# Trademarks
-----------
* Linuxは、Linus Torvalds氏の米国およびその他の国における登録商標または商標です。
* RedHat、RHEL、CentOSは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* Windows、PowerShellは、Microsoft Corporation の米国およびその他の国における登録商標または商標です。
* Ansibleは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* pythonは、Python Software Foundationの登録商標または商標です。
* Zabbixは、ラトビア共和国にあるZabbix LLCの商標です。
* Oracleは、Oracle International Corporation の米国およびその他の国における登録商標または商標です。
* NECは、日本電気株式会社の登録商標または商標です。
* その他、本ロールのコード、ファイルに記載されている会社名および製品名は、各社の登録商標または商標です。

## Description
本Roleは統合監視ソフトウェア"Zabbix"のエージェント"の環境設定を行います。
対象バージョンは以下のバージョンです。
- Zabbix 4.0
  - コミュニティ版

以下の設定を行います。
- ZabbixAgentサービスの自動起動設定
- ZabbixAgentサービスの再起動

## Supports
- 管理マシン(Ansibleサーバ)
  - Linux系OS（RHEL/CentOS）
  - Ansible バージョン2.5 以上 (動作確認済み：2.5、2.6)
  - Python バージョン2.6 または 2.7
- 管理対象マシン(ZabbixAgentマシン)
  - Microsoft Windows Server 2016, Microsoft Windows Server 2012 R2
  - PowerShell 5.0  

## Requirements
- 管理マシン(Ansibleサーバ)
  - 管理対象マシンへ	WinRM接続できること
  - WinRM接続設定は次のサイトを参考にすること
    - http://docs.ansible.com/ansible/intro_windows.html#windows-system-prep
- 管理対象マシン(ZabbixAgentマシン)
  - コミュニティ版ZabbixAgent4.0がインストールされていること
  - PowerShell 5.0を利用できること
  - ファイアフォールが適切に設定されていること

## Role Variables
### Mandatory variables

実行時には以下の変数を必ず指定します。

- WinRM設定（group_varsもしくはhost_varsに設定）
  * ''ansible_user'': ユーザ名(string)
  * ''ansible_password'': パスワード(string)
  * ''ansible_port'': ポート(number)
  * ''ansible_connection'': 接続方式(string)
  * ''ansible_winrm_server_cert_validation'': 暗号化有無(string)

### Optional variables

以下の変数は任意で指定します。

- Zabbix Agent サービス設定
  * ''VAR_Zabbix40AG_WIN_Enable'': サービス自動起動有効・無効 (on|off, default: "on")
    + on : スタートアップの種類を[自動]に設定
    + off : スタートアップの種類を[手動]に設定
  * ''VAR_Zabbix40AG_WIN_Started'': 再起動有効・無効 (on|off, default: "off")
    + on : Zabbix Agent サービスを再起動する。（停止している場合は起動）
    + off : Zabbix Agent サービスの起動・停止操作を行わない。（停止している場合は停止のまま）


## Usage
1. 本Roleを用いたPlaybookを作成します。
2. 必須変数を指定します。
3. 任意変数を必要に応じて指定します。
4. Playbookを実行します。

## Example Playbook

インストールとすべての設定をする場合は、提供した以下のRoleを"roles"ディレクトリに配置した上で、以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - group_vars/
    ・ all
    ・ server1
    ・ server2
  - host_vars/
    ・ host1
    ・ host2
  - roles/
    ・ Zabbix40-Agent_WIN_install/
    ・ Zabbix40-Agent_WIN_ossetup/
    ・ Zabbix40-Agent_WIN_register/
    ・ Zabbix40-Agent_WIN_setup/
  - Zabbix40-Agent_WIN_install.yml
  - Zabbix40-Agent_WIN_ossetup.yml
  - Zabbix40-Agent_WIN_register.yml
  - Zabbix40-Agent_WIN_setup.yml
  - conf.yml
  - hosts
  - site.yml
~~~


- マスターPlaybookサンプル 「Zabbix40-Agent_WIN_ossetup.yml」
~~~
# Zabbix40-Agent_WIN_ossetup.yml
 - name: OS setup for Zabbix Agent service
   hosts: all
   gather_facts: yes
   become: no
   tags:
    - ossetup
   roles:
     - Zabbix40-Agent_WIN_ossetup
~~~

## Running Playbook
- extra-vars を利用する場合の実行例
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml"

- group_vars を利用する場合の実行例  
group_vars で指定したグループ名が webserver1 の場合
> ansible-playbook site.yml  -i hosts -l webserver1

- host_vars を利用する場合の実行例  
host_vars で指定したグループ名が server1 の場合
> ansible-playbook site.yml  -i hosts -l server1

- 本Roleのみを実行する場合は、 --tags "setup" を付け加える
> ansible-playbook site.yml  -i hosts --extra-vars="@conf.yml" --tags "ossetup"


# Copyright
Copyright (c) 2018 NEC Corporation

# Author Information
NEC Corporation
