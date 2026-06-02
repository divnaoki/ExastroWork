# Movement: configure_rhel_settings（RHEL設定変更）※スケルトン

| 登録項目 | 値 |
|----------|----|
| Movement名 | configure_rhel_settings |
| オーケストレータ | Ansible Legacy Role |
| **ロールパッケージ** | **rhel_config** |
| ホスト指定形式 | IP（またはホスト名） |
| WinRM接続 | **False**（SSH接続） |
| 紐付けるロール | `packages/rhel_config/roles/configure_rhel_settings/` |
| 所属Conductor | C「RHEL設定変更」(1番目) |
| オペレーション | RHEL設定_初回 |

## ヘッダーセクション（プレイヘッダー）
```yaml
- hosts: all
  gather_facts: true
  become: true
```
※ RHELゲストはSSH接続。root権限が要る設定は `become: true`。

## 処理概要・状況
- **実行対象は RHELゲストVM そのもの**（vm_build/windows_config が Hyper-Vホストを対象とするのと異なる）。
  RHELはkickstartで静的IP割当済みのためAnsibleから直接SSH到達できる。
- 現時点でRHEL固有の設定値は**未確定**（IP=kickstart済 / 時刻同期=vm_buildで実施済）。
  本ロールは**スケルトン**。設定値（ホスト名・タイムゾーン・パッケージ等）が確定したら
  `tasks/configure_rhel_settings.yml` に「事前→変更(冪等)→事後→検証→エビデンス」で実装する。
- RHELゲストを実行対象にするため、別途**機器一覧へのRHELゲスト登録**と、必要なら専用パラメータシートが要る。
