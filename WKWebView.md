# WKWebView

## WKNavigationDelegate

```
- (void)webView:(WKWebView *)webView
decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction
decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler;
```

用户点击打开网页时，先调用这个回调方法，`navigationAction`参数有两个属性，`sourceFrame`和`targetFrame`，分别表示源页面与目标页面，类型是`WKFrameInfo`，`WKFrameInfo`有一个 `mainFrame`的属性，正是这个属性决定是否要新开页面。如果 `targetFrame`的`mainFrame`属性为NO，表明这个`WKNavigationAction`将会新开一个页面。这时会调用`WKUIDelegate`中的创建WebView的代理方法。

## WKUIDelegate

[WKWebView遇到_blank的处理方法](https://www.jianshu.com/p/3a75d7348843)

```
- (nullable WKWebView *)webView:(WKWebView *)webView
createWebViewWithConfiguration:(WKWebViewConfiguration *)configuration
forNavigationAction:(WKNavigationAction *)navigationAction
windowFeatures:(WKWindowFeatures *)windowFeatures;
```


## WKScriptMessageHandler




