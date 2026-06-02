# パラメータシート定義: VM構築パラメータ

Exastro ITA v2.x パラメータシート（メニュー）の手動登録ガイド。本アプリは登録を行わない（設計のみ）。

## メニュー基本情報
| 項目 | 値 |
|------|----|
| メニュー名 | VM構築パラメータ |
| **バンドル設定** | **ON**（1ホスト=1 Hyper-Vホストに対し、複数VMレコードを縦持ち） |
| 1レコードの単位 | **1レコード = 1VM** |
| 縦軸（ホスト） | Hyper-Vホスト（実行対象） |

> バンドルONのため、1つのHyper-Vホスト＋オペレーションに対し、そのホストで作成するVMの数だけ行（レコード）を登録する。各行の並び順は **自動代入値設定の代入順序**で制御する（= Playbookの `loop` 順）。

## 項目定義（17項目 = 複数具体値変数 `VAR_vm` のメンバー）
| 項番 | 項目名 | メンバー変数(item.*) | 型 | 必須 | 機密 | 備考 |
|----|--------|----------------------|----|----|----|------|
| 1 | VM名 | name | 文字列 | ● | | リネーム後のVM名 |
| 2 | OS種別 | os_type | 文字列(rhel/windows) | ● | | 分岐制御 |
| 3 | テンプレートパス | template_path | 文字列 | ● | | `.vmcx`の場所。OS種別で別 |
| 4 | vCPU数 | cpu_count | 整数 | ● | | |
| 5 | 起動メモリMB | memory_startup_mb | 整数 | ● | | |
| 6 | 動的メモリ | memory_dynamic | 真偽(true/false) | ● | | |
| 7 | 最小メモリMB | memory_min_mb | 整数 | △ | | 動的時のみ |
| 8 | 最大メモリMB | memory_max_mb | 整数 | △ | | 動的時のみ |
| 9 | 仮想スイッチ名 | switch_name | 文字列 | ● | | |
| 10 | VLAN ID | vlan_id | 整数 | △ | | 未使用時は空 |
| 11 | セキュアブート | secure_boot | 文字列(on/off/rhel) | ● | | RHELは`rhel` |
| 12 | ゲスト管理者 | guest_admin_user | 文字列 | △ | △ | PowerShell Direct用(Windows) |
| 13 | ゲスト管理者PW | guest_admin_password | 文字列 | △ | **●** | **機密・no_log**(Windows) |
| 14 | 静的IP | guest_ip_address | 文字列 | △ | | Windowsのみ |
| 15 | プレフィックス長 | guest_subnet_prefix | 整数 | △ | | Windowsのみ |
| 16 | デフォルトGW | guest_default_gateway | 文字列 | △ | | Windowsのみ |
| 17 | DNSサーバ | guest_dns_servers | 配列(文字列) | △ | | Windowsのみ |

## 登録例（1 Hyper-Vホスト hv01 のレコード例）
| 代入順序 | name | os_type | cpu_count | memory_startup_mb | switch_name | secure_boot | guest_ip_address |
|----|------|---------|-----------|-------------------|-------------|-------------|------------------|
| 1 | rhel01 | rhel | 4 | 8192 | vSwitch01 | rhel | （空） |
| 2 | win01 | windows | 4 | 4096 | vSwitch01 | on | 192.168.10.21 |
| 3 | win02 | windows | 4 | 4096 | vSwitch01 | on | 192.168.10.22 |

> RHELレコードは項目14〜17（IP系）を空にする。`configure_windows_ip` ロールが `when: os_type == 'windows'` でRHELをスキップする。

## 注意
- Windowsレコードでは 12・13（資格情報）と 14〜17（IP系）を必須入力。
- 13（パスワード）はパラメータシート上も機密区分とし、Playbookでは `no_log: true`。
- 動的メモリ(6=true)時のみ 7・8 を入力。
