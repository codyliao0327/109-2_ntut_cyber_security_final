班級：資財二乙
學號：108AB0744
姓名：廖劭其

# Analyze Apache HTTP server access log with Pandas

#### 資料來源
* https://mmas.github.io/read-apache-access-log-pandas
* https://mmas.github.io/analyze-apache-access-log-pandas

#### 資料內容
資料來源有分成兩個網頁，作者將資料分析與讀取資料分為兩個章節作介紹
###### 讀取資料
1. 介紹Apache HTTP Server Access Log的格式
2. 利用pandas讀取csv檔，同時將資料做一些標題以及資料的轉換
3. 過濾掉一些不需要的資料，例如：request非GET、status非200
###### 分析資料(使用Matplotlib畫圖)
1. 依照總天數分析每天Access的比例，並用直條圖畫出來
2. 分析每天Access的總量，並用折線圖畫出來
3. 分析網站來源的標籤，並用圓餅圖畫出來
4. 分析三個不同的Entries和Total的Entires，並用折線圖畫出來，顯示三個不同的Entries和Total的Entries在同個時間下的次數關係
