# 函式 (method, function)

## 簡短
1. [Fitnesse 20070621代碼](https://github.com/bensnows/Readings/blob/main/CleanCode/file/chapter3/HtmlUtil.md)

1. [Fitnesse 重構後代碼](https://github.com/bensnows/Readings/blob/main/CleanCode/file/chapter3/HtmlUtil_Refectoring.md)

1. 要多短？如果可以，每個函式不超過4行，且每個函式都透露出本身的「意圖」，並帶領你到下一個函式

## 區塊(Blocks) & 縮排(Indenting)
1. if, else, while 及相關敘述都應該只有一行
1. 維持函式的簡短，使用具備好的韓式名稱來描述意圖，也能增添類似文件說明的價值
1. 盡可能不讓函式包含巢狀結構，縮排程度不應大於1或2層，增加可閱讀性

## 只做一件事情
1. 什麼是一件事？
1. 函式 3-3
    1. 判斷此頁是否是測試頁面
    1. 如果是測試頁面，則納入設定與拆解步驟
    1. 將此頁面轉成html
1. 回顧方法名稱 renderPage With Setups And Teardowns
    1. renderPage
    1. setup
    1. teardown
1. 一件事的定義：
    1. 同一層次的抽象概念
    1. 撰寫韓式的原因，就為了將大的概念拆解成下一層次的數個步驟
    1. 觀察函式能否從函式中提煉出另一個函式，但此函式不能只是重新詮釋原函式的實現過程
    1. example
        1. 一個麥當勞套餐
            1. 取得飲料
                1. 調整冰塊
                1. 裝綠茶
            1. 取得主餐
                1. 放蔬菜
                1. 放肉
```java
    public Set getSet(SetOrder order) {
        MSet set = new MSet();
        set.addDrink(getDrink(order.getDrinkOrder()));
        set.addMainCource(getMainCource(order.getMainCource()));
        return set;
    }
    public Drink getDrink(DrinkOrder order) {
        Drink drink = new Drink(order.getSize());
        drink.addIce(order.getIceAmount());
        drink.addDrink(order.getDrinkType());
        return drink;
    }

    public Burger getMainCource(MainCourseOrder order){
        return createCource(order);
    }

    public Burger createBurger(MainCourseOrder order){
        Burger burger = new Burger();
        burger.addVegetable(order.getVegetableType());
        burger.addMeat(order.getMeatType());
        return burger;
    }
```

## 由上而下閱讀程式碼：降層準則
    1. 為了要包含設定和拆解，我們先納入「設定」，再納入「測試頁的內容」，最後納入「拆解」
        1. 為了要納入設定，如果是套件，會先納入「套件設定步驟」，然後再引入「一般設定步驟」
            1. 為了要納入套件設定，先搜索 SuiteSetup 頁面的上層，然後納入該頁面路徑的敘述
                1. 為了要搜索上一層....
            1. 為了要納入一般設定步驟
        1. 為了要納入測試頁的內容
        1. 為了要納入拆解
        
## Switch 敘述
```java
    public Money calculatePay(Employee e){
        switch (e.type){
            case PartTime:
                return calculateByHour(e);
            case Hired:
                return calculateByMonth(e);    
        }
    }
```
1. 分析
    1. 太攏長，當加入新的職員型態，函式會變的更長
    2. 這函式做的超過一件事
        1. 判斷型態
        2. 依照型態算費用
    3. 違反單一職責原則 (Single Responsibility Principle,SRP)
    4. 違反開放封閉原則 (Open-Closed Principle,OCP)
    5. 代碼中必然有很多類似的結構模式.....
        1. isPayDay(Employee e, Date date)
        2. deliverPay(Employee e, Money pay)
1. 處理方法
    1. 將物件的多型應用在此，以 Factory重構此代碼
        1. 函式：完成計算薪資的目的
        2. Factory：完成切換物件型態的目的

```java
    public abstrac class Employee {
        public abstract boolean isPayDay();
        public abstract Money calculatePay();
        public abstract void deliverPay(Money pay);
    }

    public interface EmployeeFactory {
        public Employee makeEmployee(EmployeeRecord r);
    }
    public class EmployeeFactoryImpl {
        Employee makeEmployee(EmployeeRecord record){
            switch (e.type){
                case PartTime:
                    return PartTimeEmployee(record);
                case Hired:
                    return HiredEmployee(record);
                default:
                    throw new InvalidEmployeeType(r.type);
            }

        }
    }

```

## 具備描述能力的名稱
    為方法選一個好名稱
    有助於理解方法意圖
    ex: isTestPage, includeSetupAndTeardownPages,renderPageWithSetupsAndTeardowns

## 方法帶有的參數數量
    

