# macproxy

macOS 系統代理管理工具，一鍵切換 HTTP/HTTPS/SOCKS 代理，適用於 ClashX / Clash Verge / Surge 等代理軟體。

## 特性

- 自動偵測活躍網路介面（無需手動指定 Wi-Fi 或 Ethernet）
- 同時設定 HTTP、HTTPS、SOCKS 代理
- 支援自訂代理位址，設定持久化儲存
- 自動產生環境變數檔案供終端使用
- `init` / `uninit` 指令管理 shell 自動載入，無需手動編輯設定檔

## 安裝

將 `macproxy` 複製到 PATH 中的任意位置：

```bash
cp macproxy /usr/local/bin/
chmod +x /usr/local/bin/macproxy
```

首次安裝後執行 `init`，讓新終端視窗自動載入代理環境變數：

```bash
macproxy init
```

## 用法

```bash
macproxy on                          # 啟用系統代理
macproxy off                         # 關閉系統代理
macproxy status                      # 查看代理狀態
macproxy config                      # 查看當前設定
macproxy config <host> [port]        # 設定代理位址與連接埠
macproxy config <host>:<port>        # 設定代理位址與連接埠
macproxy init                        # 將自動載入寫入 shell 設定檔
macproxy uninit                      # 從 shell 設定檔移除自動載入
```

### 範例

```bash
# 設定代理伺服器位址
macproxy config 192.168.1.100 7890

# 也支援 host:port 格式
macproxy config 192.168.1.100:1080

# 啟用代理
macproxy on

# 查看狀態
macproxy status

# 關閉代理
macproxy off
```

## 設定檔

| 檔案 | 說明 |
|------|------|
| `~/.macproxy.conf` | 代理位址設定（由 `macproxy config` 管理） |
| `~/.macproxy_env` | 環境變數檔案（由 `macproxy on` 產生，`macproxy off` 刪除） |

### 關於環境變數與 `init`

`macproxy on` 透過 `networksetup` 設定的是**系統層級**的代理，瀏覽器等 GUI 應用程式會自動套用。但終端中的命令列工具（如 `curl`、`git`、`npm`）不會讀取系統代理設定，它們依賴環境變數 `http_proxy` / `https_proxy` / `all_proxy`。

因此 `macproxy on` 會同時產生 `~/.macproxy_env`：

```bash
export http_proxy=http://192.168.1.100:7890
export https_proxy=http://192.168.1.100:7890
export all_proxy=socks5://192.168.1.100:7890
```

執行一次 `macproxy init` 後，會在 shell 設定檔（`~/.zshrc` 或 `~/.bashrc`）中寫入自動載入邏輯：

```bash
# macproxy 環境變數自動載入
[[ -f ~/.macproxy_env ]] && source ~/.macproxy_env
```

之後每個新終端視窗都會自動載入代理環境變數。代理關閉時 `~/.macproxy_env` 會被刪除，不會載入任何東西。

若不再需要，執行 `macproxy uninit` 即可清除。
