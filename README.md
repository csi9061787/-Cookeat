# 好煮義 cook eat -- 食譜推薦系統

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E5%B0%88%E9%A1%8C%E6%B5%B7%E5%A0%B1.png" width="500" height="300"/><br/>

## 整體架構
### 圖像辨識架構

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E5%9C%96%E5%83%8F%E8%BE%A8%E8%AD%98%E6%9E%B6%E6%A7%8B1.jpg" width="800" height="400"/><br/>

* __圖片資料來源__：[Open Images](https://storage.googleapis.com/openimages/web/index.html)  

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/openimage%20download.jpg" width="700" height="400"/><br/>

* __Images test__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E8%BE%A8%E8%AD%981.jpg" width="700" height="400"/><br/>


### 食譜推薦架構

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E9%A3%9F%E8%AD%9C%E6%8E%A8%E8%96%A6%E6%9E%B6%E6%A7%8B1.jpg" width="800" height="400"/><br/>

* __爬蟲資料來源：icook, cook1cook, 聯合新聞網__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E7%88%AC%E8%9F%B2%E8%B3%87%E6%96%99%E4%BE%86%E6%BA%90.jpg" width="700" height="400"/><br/>

* __資料清洗流程__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E6%B8%85%E6%B4%97%E6%B5%81%E7%A8%8B.jpg" width="700" height="300"/><br/>

* __食譜分群 - k-means__

挑選15種營養素，依據每篇食譜的營養素做分群，將含有相似營養素的食譜分成一群，最後發現分5群較為合適。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E9%A3%9F%E8%AD%9C%E5%88%86%E7%BE%A4.jpg" width="700" height="400"/><br/>

* __協同過濾 - ALS__

取得使用者想要搜尋的食材名稱後 我們會分兩個步驟同時進行：

1. 從MySQL挑出符合食材名稱的食譜。
  
2. 從協同過濾的model裡，取得針對這位使用者前幾名高分的食譜名單。
  
綜合兩邊的資訊取出共同食譜，並搭配使用者喜好的風格取出5筆，若是結果不足5筆，則會依據符合食材的食譜+風格篩選，挑選按讚+瀏覽數較高的食譜。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E6%8E%A8%E8%96%A6%E6%B5%81%E7%A8%8B.jpg" width="700" height="300"/><br/>

* __記錄使用者行為__

使用了Spark MLlib裡的ALS模型，評分矩陣是透過使用者點擊食譜上的"我喜歡"按鈕次數 記錄使用者的評分，並且會將使用者點擊的行為記錄在HDFS裡。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E8%A8%98%E9%8C%84%E4%BD%BF%E7%94%A8%E8%80%85%E8%A1%8C%E7%82%BA.jpg" width="700" height="400"/><br/>

## 環境架構

在ubuntu作業系統上建立了這些服務。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E7%92%B0%E5%A2%83%E6%9E%B6%E6%A7%8B.jpg" width="500" height="300"/><br/>

* __kafka 流程__

架設Kafka的主要目的是避免過多使用者同時使用的情況下，對資料庫造成太多的負載，並且可以暫存使用者的相關資訊，避免當伺服器過載而掛掉時造成資料遺失。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/kafka%E6%B5%81%E7%A8%8B.png" width="500" height="200"/><br/>

## Line功能
* __Line 使用流程圖__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/line%E4%BD%BF%E7%94%A8%E6%B5%81%E7%A8%8B%E5%9C%96.jpg" width="500" height="300"/><br/>

* __會員專區__

讓使用者填寫相關訊息，並依據填寫內容搭配關鍵字搜尋。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E5%A1%AB%E5%AF%AB%E5%95%8F%E5%8D%B7.jpg" width="500" height="300"/><br/>

* __主題推薦__

五種主題提供給使用者選取。

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E4%B8%BB%E9%A1%8C%E6%8E%A8%E8%96%A6.jpg" width="500" height="300"/><br/>

* __關鍵字搜尋__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E9%97%9C%E9%8D%B5%E5%AD%97%E6%90%9C%E5%B0%8B.png" width="250" height="400"/><br/>

* __拍攝食材辨識__

<img src="https://github.com/csi9061787/CEB102_Project/blob/main/cookeat_Image/%E5%9C%96%E7%89%87%E8%BE%A8%E8%AD%98.png" width="250" height="400"/><br/>

* __Line Demo Video__

https://user-images.githubusercontent.com/78249517/119629065-40d7c900-be40-11eb-9916-af8afc13d092.mp4


* 如果你對專題發表有興趣可以觀看：https://www.youtube.com/watch?v=W6ht3U5TXmc
