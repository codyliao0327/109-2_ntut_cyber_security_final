班級：資財二乙  
學號：108AB0744  
姓名：廖劭其  

# Analyze Apache HTTP server access log with Pandas

### 資料來源
* https://mmas.github.io/read-apache-access-log-pandas
* https://mmas.github.io/analyze-apache-access-log-pandas

### 資料內容
資料來源有分成兩個網頁，作者將資料分析與讀取資料分為兩個章節作介紹
###### 讀取資料
1. 介紹Apache HTTP Server Access Log的格式
2. 利用pandas讀取csv檔，同時將資料做一些標題以及資料的轉換
3. 過濾掉一些不需要的資料，例如：request非GET、status非200
###### 分析資料(使用Matplotlib畫圖)
1. 依照總天數分析每天Access的比例，並用直條圖畫出來
2. 分析每天Access的總量，並用折線圖畫出來
3. 分析網站來源的標籤，並用圓餅圖畫出來
4. 分析三個不同的Entries和Total的Entries，並用折線圖畫出來，顯示三個不同的Entries和Total的Entries在同個時間下的次數關係

### 實作過程
###### 實作Apache Access Log檔案來源
* https://github.com/elastic/examples/blob/master/Common%20Data%20Formats/apache_logs/apache_logs  
###### 實作Analyze Apache Access Log with Pandast成果  
*
> ![1](https://user-images.githubusercontent.com/82448674/122752452-9084a580-d2c3-11eb-9555-5e0f11014b7b.png)  
#### Log檔下載下來為txt檔  
　
> ![2](https://user-images.githubusercontent.com/82448674/122753044-418b4000-d2c4-11eb-8bed-860ac989a596.png)  
#### 將txt檔載入Excel變成csv檔  
　
> ![3](https://user-images.githubusercontent.com/82448674/122753837-659b5100-d2c5-11eb-8570-02d97092c905.png)
#### 將csv檔載入，結果發現疑似是時間的格式有誤，無法成功載入  
　
> ![4](https://user-images.githubusercontent.com/82448674/122753942-8ebbe180-d2c5-11eb-8a71-24ed7be7b864.png)
#### 發現是在txt轉換為csv時，自動將時間分為兩欄，於是透過Power Query將兩欄資料合併為一欄  
　
> ![6](https://user-images.githubusercontent.com/82448674/122754325-1a357280-d2c6-11eb-932f-5d743c50843c.png)
#### 可以成功載入csv檔了，不過發現request、referer、user_agent三欄資料似乎都少了開頭第一個字...  
　
> ![7](https://user-images.githubusercontent.com/82448674/122754573-6e405700-d2c6-11eb-87cb-6f18fde7fcae.png)
#### 發現是原程式碼有將這三欄的資料做轉換才會少字，原程式碼刪除開頭與結尾的作用應該是為了刪除[]    
　
> ![8](https://user-images.githubusercontent.com/82448674/122754905-cbd4a380-d2c6-11eb-8a5c-7ee819639af6.png)
#### 接著這部分出現了錯誤，應該是資料型態造成資料處理上的問題  
　
> ![9](https://user-images.githubusercontent.com/82448674/122755029-faeb1500-d2c6-11eb-8a76-bfee1a2da811.png)
#### 因為在replace與extract的資料處理上出現了資料型態不同而無法處理，所以後來將兩行程式碼合併，並且將replace先行執行再執行extract，就可以成功了  
　
> ![10](https://user-images.githubusercontent.com/82448674/122755386-85337900-d2c7-11eb-9981-3bd8722f83e4.png)
#### 因為我們的Log只有擷取禮拜日到禮拜三的Access，所以將原程式碼的星期一到星期日改為星期日到星期三。這邊可以看到星期二的訪問人數最多  
　
> ![11](https://user-images.githubusercontent.com/82448674/122755683-eeb38780-d2c7-11eb-87a0-6c6481547382.png)
#### 這邊出現錯誤。經過網路上搜尋後，發現是新版本的pandas有將resample的語法更新...  
　
> ![12](https://user-images.githubusercontent.com/82448674/122755834-29b5bb00-d2c8-11eb-8b08-95cd57ba1d69.png)
#### 將語法更改為新版的之後就可以正常執行了。依照時間，可以看到禮拜二的Access次數最多  
　
> ![13](https://user-images.githubusercontent.com/82448674/122756013-6b466600-d2c8-11eb-8d5c-add4cc3f2dff.png)
#### 如果換成以小時計算後，也可以更精準地看到每小時的Access次數  
　
> ![14](https://user-images.githubusercontent.com/82448674/122756196-ae083e00-d2c8-11eb-8ddb-5af1275c3874.png)
#### 又出現問題...似乎是因為match跟extract那邊出問題，導致圖出不來  
　
> ![15](https://user-images.githubusercontent.com/82448674/122756328-dc861900-d2c8-11eb-93f3-9862fcbb59af.png)
#### 將match改為findall即可，不過因為使用的函式不同，出來的結果也稍微不大相同  
　
> ![15-1](https://user-images.githubusercontent.com/82448674/122756586-2c64e000-d2c9-11eb-9dff-37edfa21aae1.png)
#### 承上，如果將match換成contains，結果也會不一樣  
　
> ![16](https://user-images.githubusercontent.com/82448674/122756942-a1381a00-d2c9-11eb-9b0f-55b87c9f56a6.png)
#### 這邊同樣又遇到resample語法更新的問題，只要將語法換成新版的即可  
　
> ![17](https://user-images.githubusercontent.com/82448674/122757334-1572bd80-d2ca-11eb-9653-bef37d6563d2.png)
#### 因為我們的Log檔與作者的不同，所以造成繪製出來的圖不太好看，這邊可以做一點參數改變  
　
> ![18](https://user-images.githubusercontent.com/82448674/122757481-44892f00-d2ca-11eb-8877-4231db91c7f1.png)
#### 將資料由天計算改為由秒計算，可以更詳細看到某天的某個時段下，我們指定的url與全部Access次數的對照  
　
### 實作困難點  
1. 從最剛開始的尋找網路上現成的Access Log檔非常難找...我只有找到這個檔案
2. 找到了檔案結果是txt，幸好另一堂MIS的課有教如何使用Excel載入各種格式資料並且作資料處理，才能成功將txt轉為csv，並且將時間欄合併為原本該有的樣子
3. 後面遇到了resample的問題讓我困擾算是最久的，查了非常久的資料後，因為似乎pandas是最近才將resample的語法更新，所以網路上的新資訊很少
4. 在繪圖上，也會一直遇到我們Log檔記錄的資料數不夠多的問題(只有三天的Access...)，導致繪圖出來顯示的資訊不夠明瞭，必須將時間分段的頻率換成天或是小時甚至是秒，才可以讓圖呈現更多我們想知道的資訊
### 學習到了什麼  
經由這次的期末報告我學到非常多東西，本來覺得Apache Access Log分析應該很難做，沒想到後來跟著教學步驟一步一步做，一步一步去理解程式碼的作用，遇到問題時就先根據錯誤資訊初步判斷是什麼地方出問題，再上網搜尋搜尋解決的方式。在分析的其中其實還有遇到非常多零碎的小問題，不過幸好我有學過Python，對於這些txt檔、csv檔也都知道要如何處理，讓我可以運用既有學習到的能力結合在這項分析的報告上。至於分析完後要打成Markdown上傳到Github也是一項大工程，要再摸索一下Github如何上傳、如何使用，Markdown語法也要再搜尋一下，才能讓報告的呈現比較美觀。
