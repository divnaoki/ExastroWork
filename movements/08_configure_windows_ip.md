# Movement: configure_windows_ip（Windows静的IP投入／PowerShell Direct）

| 登録項目 | 値 |
|----------|----|
| Movement名 | configure_windows_ip |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **windows_config** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True**（実行対象はHyper-Vホスト） |
| 紐付けるロール | `packages/windows_config/roles/configure_windows_ip/` |
| 所属Conductor | B「Windows設定変更」(1番目) |
| オペレーション | Hyper-V_Windows設定_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- 実行対象は **Hyper-Vホスト**。そこから `Invoke-Command -VMName`（PowerShell Direct）でゲスト内のIPを設定。
- 対象: **Windowsゲストのみ**（`when: item.os_type == 'windows'`）。RHELはkickstart割当済みのためスキップ。
- 機密: ゲスト管理者パスワードを扱うタスクは `no_log: true`。
- 冪等性: ゲスト内現IPと要求値が異なる場合のみ設定。前後取得＋assert（パスワードは出力に含めない）。
