User: 繁體中文說明如何應用這支程式下載指定 docker image

GitHub Copilot: 這是一個關於如何使用 `docker_pull.py` 腳本來下載 Docker Image 的繁體中文說明。這個腳本允許你在不依賴 Docker 客戶端的情況下，直接與 Docker Registry API 互動並下載 Image。

### 1. 環境準備

在使用之前，請確保您的環境已經安裝了 Python，並安裝必要的相依套件。

打開終端機 (Terminal) 並執行以下指令來安裝 `requests` 套件：

```bash
pip install requests
```

### 2. 基本用法

請在終端機中切換到腳本所在的目錄 (`d:\MyGit\micro\docs\docker image\`)，然後使用以下格式執行指令：

```bash
python docker_pull.py [registry/][repository/]image[:tag|@digest]
```

### 3. 使用範例

以下是幾種常見的使用情境：

*   **下載官方 Image (預設 latest 標籤):**
    例如下載 `hello-world`：
    ```bash
    python docker_pull.py hello-world
    ```

*   **下載指定版本的 Image:**
    例如下載 `mysql-server` 的 8.0 版本：
    ```bash
    python docker_pull.py mysql/mysql-server:8.0
    ```

*   **從特定的 Registry 下載 (例如 Microsoft):**
    例如下載 `mssql-tools`：
    ```bash
    python docker_pull.py mcr.microsoft.com/mssql-tools
    ```

*   **使用 Digest (雜湊值) 下載:**
    ```bash
    python docker_pull.py consul@sha256:6ba4bfe1449ad8ac5a76cb29b6c3ff54489477a23786afb61ae30fb3b1ac0ae9
    ```

### 4. 後續步驟：匯入 Docker

腳本執行完畢後，會在其所在的目錄下產生一個 `.tar` 檔案 (例如 `library_hello-world.tar`)。若要將這個 Image 匯入到您的 Docker 環境中，請使用 `docker load` 指令：

```bash
# 格式: docker load -i <產生的tar檔名>
docker load -i library_hello-world.tar
```

匯入成功後，您就可以像平常一樣使用 `docker run` 來啟動容器了。

User: PS D:\MyGit\micro> pip install requests
pip : 無法辨識 'pip' 詞彙是否為 Cmdlet、函數、指令檔或可執行程式的名稱。請檢查名稱拼字是否正確，如果包含路徑的話，請確認路徑是否正確，然後再
試一次。

GitHub Copilot: Ran terminal command: python --version; python -m pip --version

Configuring a Python Environment
Completed with input: {
  "resourcePath": "d:\\MyGit\\micro\\docs\\docker image\\docker_pull.py"
}

Using "Select a Python Environment"
Completed with input: {
  "resourcePath": "d:\\MyGit\\micro\\docs\\docker image\\docker_pull.py"
}

Ran terminal command: py --version; py -m pip --version

看起來您的環境中 `python` 或 `pip` 沒有被加入到系統路徑 (PATH) 中，但是 Windows 的 Python 啟動器 (`py`) 是可以使用的 (版本 3.14.2)。

您可以改用 `py` 指令來執行操作。請依照以下步驟進行：

### 1. 安裝 requests 套件

請使用 `py -m pip` 來代替直接輸入 `pip`：

```powershell
py -m pip install requests
```

### 2. 執行下載程式

安裝完成後，執行程式時請將 `python` 改為 `py`：

```powershell
# 基本語法
py docker_pull.py [image名稱]

# 範例：下載 hello-world
cd "docs\docker image"
py docker_pull.py hello-world
```

這樣應該就能順利執行了。

User: PS D:\MyGit\micro\docs\docker image> python docker_pull.py hello-world
Traceback (most recent call last):
  File "D:\MyGit\micro\docs\docker image\docker_pull.py", line 87, in <module>
    layers = resp.json()['layers']
             ~~~~~~~~~~~^^^^^^^^^^
KeyError: 'layers'

GitHub Copilot: Read [](file:///d%3A/MyGit/micro/docs/docker%20image/docker_pull.py#70-70), lines 70 to 90

Using "Replace String in File"


```

```
這個錯誤 `KeyError: 'layers'` 是因為 `hello-world` 這個 Docker Image 是一個多架構 (Multi-architecture) 的 Image，它回傳的是一個 `Manifest List` (包含多個平台的清單)，而不是單一的 `Manifest` (直接包含 layers)。

原本的程式碼只有在 HTTP 狀態碼不是 200 時才去檢查是否為 Manifest List，但現在 Docker Hub 在狀態碼為 200 時也可能回傳 Manifest List，導致程式在找 `layers` 欄位時失敗。

我已經修正了程式碼，增加了對這種情況的判斷。現在請再次嘗試執行指令：

```powershell
py docker_pull.py hello-world
```

修正後，程式應該會列出該 Image 支援的所有平台及其 **digest** (摘要碼/雜湊值)，例如 `windows/amd64`, `linux/amd64` 等。

**如何下載：**
看到列表後，請複製您需要的平台對應的 `digest` (通常是 `linux/amd64` 或 `windows/amd64`)，然後使用 `Image名稱@Digest` 的格式來下載。

例如 (請使用您執行結果中顯示的實際 digest)：

```powershell
# 範例：下載 linux/amd64 版本
py docker_pull.py hello-world@sha256:4729c725f77838520268565cc51d773663bace468087799564f27503f8319fbb
```

Made changes.

User: PS D:\MyGit\micro\docs\docker image> python docker_pull.py maven:3.9.5-eclipse-temurin-17@sha256:2ccef6f4a3e466b6594da71ba1de3aa6586c96c797ba55860a9af2e13261708c
[-] Cannot fetch manifest for library/maven:3.9.5-eclipse-temurin-17 [HTTP 404]
b'404 page not found\n'

GitHub Copilot: Read [](file:///d%3A/MyGit/micro/docs/docker%20image/docker_pull.py#15-15), lines 15 to 27

Using "Replace String in File"


```

```
發生錯誤的原因是您提供的 Image 名稱格式 `maven:3.9.5-eclipse-temurin-17@sha256:...` 包含了標籤 (`:`) 和 Digest (`@`)。
原始程式碼在解析名稱時，會將 `maven:3.9.5-eclipse-temurin-17` 整個誤認為 Image 名稱，導致向 Docker Registry 請求時網址錯誤 (`library/maven:3.9.5-eclipse-temurin-17`)，因此回傳 404 找不到。

我已經修正了程式碼，現在當同時提供標籤和 Digest 時，它會正確地忽略標籤部分，只使用 `maven` 作為 Image 名稱，並使用 Digest 進行下載。

請再次執行您的指令：

```powershell
py docker_pull.py maven:3.9.5-eclipse-temurin-17@sha256:2ccef6f4a3e466b6594da71ba1de3aa6586c96c797ba55860a9af2e13261708c
```

Made changes.