##UIWebView自适应宽度 
2014年4月10日 上午11:51

```
#pragma mark - UIWebViewDelegate Methods

- (void)webViewDidFinishLoad:(UIWebView *)webView
{
    //若已经加载完成，则显示webView并return
    if(self.isLoadingFinished)
    {
        [self.webView setHidden:NO];
        self.isLoadingFinished = NO;
        return;
    }
    
    //js获取body宽度
    NSString *bodyWidth= [webView stringByEvaluatingJavaScriptFromString: @"document.body.scrollWidth"];
    
    int widthOfBody = [bodyWidth intValue];
    
    NSString *jsToGetHTMLSource = @"document.getElementsByTagName('html')[0].innerHTML";
    
    NSString *HTMLSource = [self.webView stringByEvaluatingJavaScriptFromString:jsToGetHTMLSource];
    //获取实际要显示的html
    NSString *html = [self htmlAdjustWithPageWidth:widthOfBody
                                              html:HTMLSource
                                           webView:webView];
    
    //设置为已经加载完成
    self.isLoadingFinished = YES;
    [self.webView setHidden:YES];
    //加载实际要显示的html
    //首先在隐藏html显示的情况下 加载一次html 通过js代码 计算出缩放比例
    //将计算出来的缩放比例写回html 取消隐藏 重新加载webview
    NSString *htmlFilePath = [NSHomeDirectory() stringByAppendingString:@"/Documents/OEBPS/Text"];
    NSURL *baseURL = [NSURL fileURLWithPath:htmlFilePath];
    
    [self.webView loadHTMLString:html baseURL:baseURL];
}

//获取宽度已经适配于webView的html。这里的原始html也可以通过js从webView里获取
- (NSString *)htmlAdjustWithPageWidth:(CGFloat )pageWidth
                                 html:(NSString *)html
                              webView:(UIWebView *)webView
{
    NSMutableString *str = [NSMutableString stringWithString:html];
    //计算要缩放的比例
    CGFloat initialScale = webView.frame.size.width/pageWidth;
    //将</head>替换为meta+head
    NSString *stringForReplace = [NSString stringWithFormat:@"<meta name=\"viewport\" content=\" initial-scale=%f, minimum-scale=0.1, maximum-scale=2.0, user-scalable=yes\"></head>",initialScale];
    
    NSRange range =  NSMakeRange(0, str.length);
    //替换
    [str replaceOccurrencesOfString:@"</head>" withString:stringForReplace options:NSLiteralSearch range:range];
    return str;
}
```
