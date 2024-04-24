
# HtmlUtil.java v2
```java
public static String renderPageWithSetupsAndTeardowns(PageData pageData, boolean isSuite) throws Exception {
    boolean isTestPage = pageData.hasAttribute("Test");
    if (isTestPage) {
        WikiPage testPage = pageData.getWikiPage();
        StringBuffer newPageContent = new StringBuffer();
        includeSetupPages(testPage, newPageContent,isSuite);
        pageData.setContent(newPageContent.toString());
    }
    return pageData.getHtml();
}

```

# HtmlUtil.java v3
```java
public static String renderPageWithSetupsAndTeardowns(PageData pageData, boolean isSuite) throws Exception {
    if (isTestPage(pageData)) {
        includeSetupAndTeardownPages(pageData, isSuite);
    }
    return pageData.getHtml();
}
```