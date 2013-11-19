Selenium.HtmlElements.Net
=========================

## Define your custom element

```C#
internal class Pagination : HtmlElement {

    public Pagination(IWebElement wrapped) : base(wrapped) {
        PageFactory.InitElementsIn(this, wrapped);
    }

    [FindsBy(How = How.CssSelector, Using = ".value.rating")]
    private HtmlElement _currentPageNumber;

    [FindsBy(How = How.CssSelector, Using = ".nextPage")]
    private HtmlLink _nextPageLink;

    public int CurrentNumber {
        get { return int.Parse(_currentPageNumber.Text); }
    }

    public DevLifePage OpenNextPage() {
        return _nextPageLink.Open<DevLifePage>();
    }
  
}
```

## and use it within page or other element

```C#
internal class DevLifePage : CustomElement {

    [FindsBy(How = How.CssSelector, Using = ".jslink:nth-child(1)")]
    private HtmlLink _sortByDate;

    [FindsBy(How = How.CssSelector, Using = ".jslink:nth-child(3)")]
    private HtmlLink _sortByHottest;

    [FindsBy(How = How.CssSelector, Using = ".jslink:nth-child(2)")]
    private HtmlLink _sortByRatig;

    [FindsBy(How = How.CssSelector, Using = ".entry")]
    public DevLifePost Posts { get; private set; }

    public DevLifePage(ISearchContext wrapped) : base(wrapped) {}

    [FindsBy(How = How.CssSelector, Using = ".pagination")]
    public Pagination Pagination { get; private set; }

    public void SortByDate() {
        _sortByDate.Click();
    }

    public void SortByRating() {
        _sortByRatig.Click();
    }

    public void SortByHottest() {
        _sortByHottest.Click();
    }

}
```

## Finally apply PageFactory to your page

```C#
var devLifePage = PageFactory.InitElementsIn<DevLifePage>(new FirefoxDriver());
```
