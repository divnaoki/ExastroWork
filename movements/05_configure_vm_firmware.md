# Movement: configure_vm_firmware（ファームウェア／セキュアブート）

| 登録項目 | 値 |
|----------|----|
| Movement名 | configure_vm_firmware |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/configure_vm_firmware/` |
| 所属Conductor | A「仮想マシン構築」(5番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション
```yaml
- hosts: all
  gather_facts: false
```

## 処理概要
- Generation 2 のセキュアブート設定。`item.secure_boot`：`on`/`off`/`rhel`（RHELは `SecureBootTemplate=MicrosoftUEFICertificateAuthority`）。
- 冪等性: 現SecureBoot状態・テンプレートと要求が異なる場合のみ `Set-VMFirmware`。前後取得。
