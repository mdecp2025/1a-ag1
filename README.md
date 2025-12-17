

# Brython Letter Display (字母顯示器) 

### 這是一個以 Brython（Python for Browser） 撰寫的互動式網頁程式，能將使用者輸入的英文字母或數字，轉換成由 7×7 彩色方格 組成的像素字
(為了這句 A strict teacher produces excellent student，多搞了幾個版本 🧠)

<img width="1247" height="846" alt="image" src="https://github.com/user-attachments/assets/36f4b46b-ed41-4c12-8d4b-5065b2bdf9bf" />

 
 ##  點擊 [此處打開模擬器](https://leceichen.github.io/Advanced-Brython-Letter-Display/ )


##  Advanced Brython 字母顯示器功能

互動字母視覺化 – 使用 Brython（瀏覽器中的 Python）動態顯示與動畫化字母

- 即時輸入支援 – 使用者可輸入字母，立即在畫布上呈現

- 可自訂樣式 – 可以調整字型大小、顏色與顯示效果

- 網格佈局 – 字母以結構化的網格系統排列顯示

- 互動控制 – 包含按鈕與滑桿，可控制動畫速度、字母大小與佈局

- 瀏覽器直接運行 – 無需安裝，透過 GitHub Pages 即可使用


  

 ##  運作原理
1️⃣ Brython 與前端結構

Brython 讓 Python 程式可在瀏覽器中運行，取代 JavaScript
HTML 中的 <script type="text/python"> 區塊即是主要邏輯

主要組成部分：

HTML + CSS：定義按鈕、輸入框與顯示區域

Brython：以 Python 控制 DOM、Canvas、按鈕事件
#

2️⃣ 顯示核心（點陣繪製邏輯）

每個字元（如 A, B, 1, 2, …）在字典 LETTER_PATTERNS 中定義
範例：

'A': ["0011100",
      "0110110",
      "1100011",
      "1111111",
      "1100011",
      "1100011",
      "1100011"]


每一行代表 7 個像素點，1 為有色方格，0 為空白
#

3️⃣ World 類別（繪圖世界）

負責建立一個 Canvas「世界」：

有三層：grid（格線）、background（背景）、display（顯示層）

每一格方塊為 25px × 25px。

使用 draw_color_block(x, y, color) 於指定座標畫出有色方格
#

4️⃣ LetterDisplay 類別（顯示控制）

接收輸入文字，依照字數建立對應寬度的繪圖世界

逐字取出 LETTER_PATTERNS 中的資料，依序繪製

每個字元顏色不同，從 self.colors 中循環取色
#

5️⃣ 互動操作

透過 Brython 綁定按鈕與輸入事件：

「顯示」：讀取輸入框內容並呼叫 display.display_text()

「清除」：清空輸入框與顯示區

範例按鈕：快速輸入並顯示預設字串

按下 Enter 鍵也能觸發顯示

##

# 7X7 Lab  

12/16
<img width="1894" height="882" alt="image" src="https://github.com/user-attachments/assets/ba0ab088-bf22-432d-9f86-f5fcf8023f87" />
##


##  點擊 [此處打開模擬器](https://mdecp2025.github.io/1a-ag1/)


## ##  點擊 [此處打開介紹影片] (https://youtu.be/usFmvSHkH84)

這個網頁程式是一個相當有趣、實用的小工具 ——
它的名稱可以概括為 「7×7 像素文字 3D 列印生成器」。
下面我幫你詳細介紹它的功能、原理與用途

#
一、 網站程式架構 

結構 (HTML)	定義頁面的內容骨架，包括標題、輸入表單（文字輸入框、尺寸設定）、結果輸出區（G-code 文本框、預覽畫布）以及按鈕等。

樣式 (CSS)	定義頁面的視覺風格，採用深色模式，確保介面的現代感和易讀性。

邏輯 (JavaScript)	核心的運作引擎，包含字型數據、數學計算、G-code 生成邏輯、以及所有的使用者互動處理（事件監聽）。

核心資料	charMap 物件，這是應用程式的字型資料庫，定義了 7x7 像素陣列。

視覺化	使用 <canvas> 元素來實現 2D 列印路徑的繪製與即時互動預覽。

#
二、 運作原理與資料流這個應用程式的運作流程是一個清晰的單向數據流，從使用者輸入開始，經過一系列計算，最終產生 G-code 輸出：

輸入觸發:使用者在文字框、尺寸或樣式下拉選單中進行任何變動（input 或 change 事件），都會立即觸發 showFontPreview() 函數。

解析與預覽:showFontPreview() 函數會查詢內建的 charMap（7x7 像素字型數據庫），將輸入文字轉換為二進位點陣圖。同時，它會更新網頁上的像素預覽圖。

計算路徑:預覽完成後，系統會呼叫 calculatePath() 函數。這個函數依據使用者設定的 pixelSize 和 printStyle（實心或空心），計算出機器需要移動的所有 $X, Y, Z$ 座標點，並儲存為路徑數據陣列 (pathData)。

視覺化呈現:drawPath() 函數讀取 pathData，使用 Canvas API 將這些計算出的路徑點繪製出來，提供即時的 2D 俯視圖預覽，讓使用者可以檢查路徑是否正確。

G-code 輸出:當使用者點擊「生成 G-code」按鈕時，generateGcode() 函數被執行。它結合路徑數據、分層邏輯、速度設定以及核心的擠出量計算，將所有的動作指令（如 G0、G1）組合成最終的 G-code 文本。
#

三、 核心邏輯 (Core Logic)
應用程式的效率和準確性取決於兩個關鍵的數學計算：
1. 座標轉換與 Y 軸翻轉目的: 將 7x7 像素陣列的抽象索引轉換為 3D 空間的實際毫米座標。Y 軸修正 (關鍵):
   在 7x7 陣列中，row 0 代表文字的頂部。為了讓 G-code 輸出的 Y 座標能正確對應到 3D 列印床上的方向（通常 Y 座標向上延伸），程式碼必須將 Y 座標進行反向計算，確保頂部像素對應到較高的 Y 值，底部像素對應到較低的 Y 值。

2. 擠出量計算 (Extrusion Volume)E 值計算:
   程式碼必須精確計算每次移動（G1 指令）時需要擠出的塑料量（G-code 中的 E 值）。體積守恆: 它採用了 3D 列印的標準原理：擠出體積 必須等於 路徑長度乘以線寬乘以層高 所形成的體積。這個計算使用了線材直徑、噴嘴直徑和層高等參數，確保塑料輸出量與列印路徑體積精確匹配。

#
四、 互動操作 (User Interaction)
所有的使用者互動都是透過 原生 JavaScript 事件監聽器 來實現的，確保了無需重新加載頁面即可獲得即時回饋：

即時更新: 所有與文字內容、pixelSize、blockHeight 和 printStyle 相關的輸入欄位，都綁定了 input 或 change 事件，即時呼叫 showFontPreview() 和 calculatePath() 來更新預覽。

Canvas 預覽控制:

平移 (Pan): 透過滑鼠的 mousedown、mousemove 和 mouseup 事件來處理，實現拖動畫布的功能。

縮放 (Zoom): 透過滑鼠滾輪 wheel 事件或專用的縮放按鈕來控制畫布的縮放比例。

動畫播放: 專門的按鈕綁定 playPathAnimation() 函數，可以模擬 3D 列印機的移動過程，循序顯示 G-code 路徑。

透過這些結構和邏輯，這個輕量級的網頁應用程式實現了複雜的 3D 列印 G-code 生成功能。


   






     


