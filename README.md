# lineBot_backend-AWS_Lambda_note
把line bot後端flask丟到AWS Lambda的重點筆記

# AWS Lambda
[使用 AWS Lambda 部署 LINE 聊天機器人（上）](https://www.ecloudture.com/deploy-line-chatbot-using-aws-lambda-1/)

[使用 AWS Lambda 部署 Line 聊天機器人 （下）
](https://www.ecloudture.com/deploy-line-chatbot-using-aws-lambda-2/)

[如何使用 Lambda 定期停止和啟動 Amazon EC2 執行個體？](https://aws.amazon.com/tw/premiumsupport/knowledge-center/start-stop-lambda-eventbridge/)

[使用Serverless，Lambda和DynamoDB構建Python REST API](https://www.serverless.com/blog/flask-python-rest-api-serverless-lambda-dynamodb/)


```
利用 AWS Lambda Layers 在 AWS Lambda 環境中部署 Python 函式庫
使用第三方函式庫要設在Lambda Layer中，Lambda Function 才可引用函式庫

照以下結構打包成 Zip
📂python
 ┗ 📂lib
   ┗ 📂python3.8 // This is for python 3.8
     ┗ 📂site-packages
       ┗ **All files**
```


---
0. 本地測試
使用 virtualenv 創建虛擬環境，並在其中安裝所需的依賴套件。(可以用ngrok先行測試)

創建虛擬環境
```shell
virtualenv venv
```

激活虛擬環境
```shell
source venv/bin/activate
```

pip 安裝所需的依賴套件
```shell
pip install -r requirements.txt
```

將虛擬環境和所需的文件壓縮成一個 ZIP 文件
```shell
主要把site-packages中的依賴項全部塞進**All files**
📂python
 ┗ 📂lib
   ┗ 📂python3.8 // This is for python 3.8
     ┗ 📂site-packages
       ┗ **All files**
```

1. 在AWS上建立Lambda函數
登錄AWS管理控制台並打開Lambda服務。接著，創建一個新的Lambda函數，選擇Python x.x，在"編輯程式碼"頁面，將Flask程式碼貼到程式碼編輯器

2. 設定環境變數
將LINE Bot聊天機器人的基本資料存儲在環境變數中。在Lambda函數中，可以使用"環境變數"功能設定這些變數。AWS Lambda的管理控制台中，在Lambda函數頁面上找到"環境變數"選項。設置Channel Access Token和Channel Secret變數。

使用以下方式來讀取這些環境變數：

```python
import os

channel_access_token = os.environ.get('CHANNEL_ACCESS_TOKEN')
channel_secret = os.environ.get('CHANNEL_SECRET')
```

3. 設置API Gateway
設置API Gateway，以便LINE Bot與AWS Lambda通信:在AWS Lambda的管理控制台中，使用"API Gateway"選項建立新的API Gateway。可以選擇REST API，API Gateway設置與Lambda函數相關聯。當LINE Bot收到新的訊息時，API Gateway把訊息傳遞給Lambda函數進行處理。可以使用API Gateway的測試功能來測試Bot是否運行正常。


---

## 虛擬環境筆記
建立虛擬環境

使用以下命令在目前目錄下建立名為env的虛擬環境：

python -m venv env

您可以將env替換為您想要的環境名稱。

啟動虛擬環境

在Windows上，在命令提示符或PowerShell中啟動虛擬環境：

.\env\Scripts\activate

在Linux或macOS上，在終端中啟動虛擬環境：

source env/bin/activate

啟動虛擬環境後，您將在命令提示符或終端上看到環境名稱的前綴，表明您現在正在使用該虛擬環境。

安裝依賴套件

在虛擬環境中使用pip安裝依賴套件：

pip install package_name

OR

pip install -r requirements.txt

列出已安裝的套件

在虛擬環境中使用pip列出已安裝的套件：

pip list

匯出依賴套件清單

將目前虛擬環境中安裝的所有依賴套件匯出到requirements.txt文件中：

pip freeze > requirements.txt

退出虛擬環境

在虛擬環境中使用以下命令退出虛擬環境：

deactivate

請注意，在Windows上，您也可以使用以下命令退出虛擬環境：

.\env\Scripts\deactivate.bat

在Linux或macOS上，您可以使用以下命令退出虛擬環境：

deactivate







要查看venv中已建立的所有虛擬環境，可以瀏覽到venv所在的目錄，並查看其中的子目錄。每個子目錄都對應著一個虛擬環境。在Windows上，預設情況下，venv所在的目錄是在目前使用者目錄的下面，名稱為venv。在Linux或macOS上，venv所在的目錄通常在目前工作目錄下的.venv目錄中。

要刪除虛擬環境，只需刪除該虛擬環境的目錄即可。在Windows上，可以使用以下命令刪除名稱為env的虛擬環境：

rmdir /s env

在Linux或macOS上，可以使用以下命令刪除名稱為env的虛擬環境：

rm -rf env

請注意，刪除虛擬環境將刪除該環境中安裝的所有套件和函式庫。如果您需要在以後再次使用該虛擬環境，則需要重新建立該虛擬環境並重新安裝依賴套件。
