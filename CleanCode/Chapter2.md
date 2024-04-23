# Chapter 2 有意義的命名
## 還好我們沒機會看到這個😱
```java
public List<int[]> getThem(){
    List<int[]> list1 = new ArrayList<>();
    for (int[] x: theList)
        if (x[0]==4)
            list1.add(x);
    return list1;
}
```

## 問題在哪？
    代碼中包含的信息：
    1. 一個搜集的List
    2. 一個迭代的清單
    3. 一個簡易的數據判斷
    4. 將目標加入List

    問題不在於程式碼的簡易度，而在於程式碼的隱含性
    即程式的上下文資訊未能由程式本身明確的展現出來

## 幾個問題在上述的代碼
1. theList 存放什麼類型的物件？
1. theList 項目內索引為0的資料，代表意義是什麼？
1. 數值 4 代表什麼意思
1. 該如何使用回傳的List數據

## 如果寫成這樣呢？
```java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<>();
    for (int[] cell: gameBoard)
        if (cell[STATUS_VALUE] == FLAGGED)
            flaggedCells.add(cell);
    return flaggedCells;
}
```

## 那如果寫成這樣呢？
```java
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell: gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
```

## 避免誤導
1. 不要用 accountList 作為變數
    1. List 對程式設計師具有特定意義，除非變數型態真的是List
    1. 如果是儲存帳號的容器，accountGroup, bunchOfAccounts 或 accounts 都比較好
1. 如果名稱只有一點點不同，使用時需特別小心
    1. 如何分辨出下面兩者的區別
        1. XYZControllerForEfficientHandlingOfStrings
        1. XYZControllerForEfficientStorageOfStrings
    1. 拼字與意圖相同就是洽當資訊，而意圖與拼字不一致的就是誤導!!
## 產生有意義的區別
1. Example 1:
    1. Product
    1. ProductInfo
    1. ProductData
    1. 解析：Info 跟 Data 是不可區分的無意義用詞
1. Example 2:
    1. zork
    1. theZork
    1. 解析：開發為了避開 zork 已經被使用，特意增加 the改變字詞，但卻需要花費更多時間釐清宣告變數的具體意義
1. Example 3:
    1. variable => 無意義詞
    1. nameString vs name?
        1. name 可能是浮點數？ 如果是....那命名是否誤導了？
1. Example 4:
    1. getActiveAccount();
    1. getActiveAccounts();
    1. getActiveAccountInfo();
    1. 解析：如果沒有特別約定....開發也只能跳進去看代碼了
1. Example 5:
    1. 減少使用單一個字命名
        1. 例如：e，因為在搜索時很不方便
        1. 例外：宣告在迴圈的常規習慣
            ```java
            for (int i = 0; i<10 ;i++) {

            }
            ```
1. 介面(Interfaces) vs 實作(Implementations)
    1. IShapeFactory => 以介面來說 I 是多餘的，對使用者來說，是不是介面並不重要
    1. 真的要讓介面跟實作有區別，作者會選在實作增加 ShapeFactoryImpl
1. 避免思維轉換
    1. 讀者不應該將名字在腦裡想一遍，才能翻譯成他們所熟知的名稱
    1. 開發者應該選擇使用"問題領域"的術語，或是"解決領域"的術語
1. 每個概念使用一種字詞
    1. 例如：fetch, retrieve, get 的差別並不大
1. 添加有意義的上下文資訊
    ```java
        private void printGuessStatistics(char candidiate, int count) {
            String number;
            String verb;
            String pluralModifier;

            if (count == 0){
                number = "no";
                verb = "are";
                pluralModifier = "s";
            } else if (count==1){
                number = "1";
                verb = "is";
                pluralModifier = "";
            } else {
                number = Integer.toString(count);
                verb = "are";
                pluralModifier = "s";                
            }

            String guessMessage = String.format("There %s %s %s%s", verb,number,candidate, pluralModifier );
            print(guessMessage);
        }
    ```

    ```java

        public class GuessStatisticsMessage {
            private String number;
            private String verb;
            private String pluralModifier;


            public String make(cgar candidate, int count) {

                createPluralDependentMessageParts(count);

                return String.format("There %s %s %s%s", verb,number,candidate, pluralModifier);
            }


            private void createPluralDependentMessageParts(int count){
                if (coount == 0 ) {
                    thereAreNoLetters();
                } else if (count == 1) {
                    thereIsOneLetter();
                } else {
                    thereAreManyLetters();
                }
            }

            private void thereAreNoLetters(){
                number = "no";
                verb = "are";
                pluralModifier = "s";
            }

            private void thereIsOneLetter(){
                number = "1";
                verb = "is";
                pluralModifier = "";
            }

            private void thereAreManyLetters(){
                number = Integer.toString(count);
                verb = "are";
                pluralModifier = "s";   
            }

        }
    ```
1. 別添加無理由的上下文
    1. example: 
        1. 如果有一個虛構的應用程式，叫做豪華版的加油站(Gas Station Deluxe)
        1. 每個類別都要加上 GSD 字首
        1. 那麼在IDE開發工具，搜索 G時，就會得到全部物件類別的建議，會降低IDE的幫助
    1. 若較小的名稱可以清楚表達含義，好過於較長的名稱
    
     

