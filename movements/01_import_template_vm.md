# Movement: import_template_vm（テンプレートVMインポート）

| 登録項目 | 値 |
|----------|----|
| Movement名 | import_template_vm |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **vm_build** |
| ホスト指定形式 | ホスト名（IPでも可） |
| WinRM接続 | **True** |
| 紐付けるロール | `packages/vm_build/roles/import_template_vm/` |
| 所属Conductor | A「仮想マシン構築」(1番目) |
| オペレーション | Hyper-V_VM構築_初回 |

## ヘッダーセクション（プレイヘッダー）
```yaml
- hosts: all
  gather_facts: false
```
※ WinRM接続のため `become` は省略。タスクはロール側（tasks/）に「タスクのみ」を実装。

## 処理概要
- 対象: Hyper-Vホスト（実行対象）。`loop: VAR_vm` で全VMを処理。
- テンプレート(.vmcx)を `Import-VM -Copy -GenerateNewId` → `Rename-VM`。
- 冪等性: `Get-VM` で同名VM存在を確認し、無い場合のみインポート。前後取得＋assert。
