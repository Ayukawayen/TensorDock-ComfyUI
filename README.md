# TensorDock-ComfyUI
在TensorDock上租用GPU架設ComfyUI

本篇未盡之處請自行參閱TensorDock、ComfyUI等說明文件。

TensorDock租用之儲存空間在待機時仍會產生費用，請注意。 (通常而言，GPU和RAM停機時不會收費，請參閱TensorDock說明)

## 登入TensorDock
進入 TensorDock首頁 (https://tensordock.com/)

點 "Deploy a Cloud GPU"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_1.png?raw=true)

點 "Sign In"，然後在登入畫面註冊帳號(註冊流程省略)

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_2.png?raw=true)


## 設定SSH Key 這只需要做一次
產生SSH Key的方式省略

若不自行產生SSH Key，那麼Value輸入一個空白也行，不影響基本功能。


點 "Secret"；然後點 "Add Secret"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_2.1.png?raw=true)

輸入Key的名稱，Type選擇SSH Key，Value填入'ssh-rsa AAAAB3Nz...' (不要換行)；然後點 "Create Secret"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_2.2.png?raw=true)


## 儲值美金 (省略)

點左側主選單的 "Billing" 然後照畫面指示操作，使用信用卡付款，一次最低加值5美金。


## 部署一台有GPU的虛擬機器

點 "Deploy GPU" 進入選擇頁面，我通常就選最便宜的，點擊該顯卡的方格

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_3.png?raw=true)

點選 "Ubuntu"，"Ubuntu 22.04 LTS"，將資源調到2CPU/8GB/100GB(只跑ComfyUI這樣就夠了)，

可以修改Instance Name以便辨識，或是保持預設值。

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_4.png?raw=true)

設定外連的port，ComfyUI預設會用到8188，填入8188後點 "Request Port"；然後點選可選的地區

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_5.png?raw=true)

設定SSH Key

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_6.png?raw=true)

點 "Deploy Instance"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_8.png?raw=true)

等待部署完成

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_9.png?raw=true)

部署完成後第一動作，點 "Stop/Disassociate VM"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_11.png?raw=true)

關機完成後，點 "Modify VM"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_12.png?raw=true)

將GPU設成0個，暫時用不到GPU，可以省點錢；然後點 "Apply Changes"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_13.png?raw=true)


## 啟動虛擬機器

此處使用TensorDock內建終端介面，使用Putty或其他SSH軟體者可自行使用。

點 "Start VM"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_14.png?raw=true)

啟動完成後，點 "Open Console"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_14.5.png?raw=true)

在新網頁分頁的畫面進行連線，最後以 'user' 登入，內建終端介面不需要輸入使用者密碼

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_15.png?raw=true)
![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_17.png?raw=true)


## 安裝ComfyUI

在Ubuntu終端介面依序輸入以下指令完成系統更新

`sudo apt update`

`sudo apt upgrade`

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_20.png?raw=true)

進入ComfyUI的Github頁面 (https://github.com/comfyanonymous/ComfyUI)

點 "<> Code" ，然後點複製按鈕

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_21.png?raw=true)

在Ubuntu終端介面輸入 `git clone https://github.com/comfyanonymous/ComfyUI.git` (使用Shift+Insert貼上文字、滑鼠右鍵選單亦可)

等待下載完成

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_23.png?raw=true)

下載模型，以Civitai上的Anything XL為例 (https://civitai.com/models/9409?modelVersionId=30163)

在 "Download" 的連結上按滑鼠右鍵，複製連結網址

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_24.png?raw=true)

在Ubuntu終端介面輸入 `wget 'https://civitai.com/api/download/models/30163?type=Model&format=SafeTensor&size=full&fp=fp16'` 用引號將網址包起來比較容易處理

等待下載完成

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_26.png?raw=true)

搬移檔案並更改檔案名，輸入 `mv '30163?type=Model&format=SafeTensor&size=full&fp=fp16' ComfyUI/models/checkpoints/AnythingXL_50.safetensors` 遇檔名和目錄名可以用Tab鍵自動完成

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_27.png?raw=true)

輸入 `cd ComfyUI` 進入工作目錄

輸入 `pip install torch torchvision torchaudio` 安裝套件

輸入 `pip install -r requirements.txt` 安裝套件

下載與安裝需要一些時間。等待安裝完成

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_29.png?raw=true)
![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_31.png?raw=true)
![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_34.png?raw=true)

輸入 `exit`

關閉網頁分頁


## 啟用GPU

回到VM管理頁面，點 "Stop/Disassociate VM"，

等候關機完成，點 "Modify VM"

將GPU設成1個，然後點 "Apply Changes"

注意VM的Public IP和對應8188的port，之後會用到。

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_36.png?raw=true)
![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_37.png?raw=true)


## 使用ComfyUI

完成GPU設定後，再次點 "Start VM"，點 "Open Console"，然後登入Ubuntu終端介面

輸入 `cd ComfyUI` 進入工作目錄

輸入 `python3 main.py --listen 0.0.0.0` 啟動ComfyUI

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_39.png?raw=true)

由外部網頁瀏覽器開啟 http://(你的IP):(你的port)

順利的話就能看到ComfyUI畫面

選擇 "Image Generation"

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_40.png?raw=true)

修改Checkpoint選項

點選 "運行" 後產生圖片

(其他請參閱ComfyUI說明文件)

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_42.png?raw=true)
![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_43.png?raw=true)

結束後在Ubuntu終端介面輸入 `ctrl+C`，然後輸入 `exit` 可登出。

**生圖結束後記得去關閉VM**

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_45.png?raw=true)

下次要使用時，由左側的My Servers進入VM管理頁面，在Start VM後用同樣方式操作。

## (使用FileZilla下載圖片)

個人作法為使用FileZilla的SFTP下載檔案，多張圖片壓縮為單一檔案後下載，需要在FileZilla載入SSH私鑰檔案，內容在此不詳述。

下載上傳大型檔案時可以先卸下GPU，節省金錢。 (自行衡量檔案大小)

## (使用Google Compute Engine的GPU)

之前Google平台上很難拿到GPU，目前狀況未測試。

如果有300美金額度想嘗試看看的，也可以使用Google的Ubuntu VM，我的經驗是高記憶體最低等級的n1-highmem-2就可以跑得起來。

![](https://github.com/Ayukawayen/TensorDock-ComfyUI/blob/main/images/image_49.png?raw=true)

