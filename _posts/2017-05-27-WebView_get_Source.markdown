---
layout: post
title: "WebView获取网页源代码"
date: 2017-05-27 +0800
toc: true
category: android
tags: Android
excerpt: 本篇文件讲解如何使用WebView获取网页源代码
---
#### WebView获取网页源代码
{% highlight java %}
public class WifiActivity extends AppCompatActivity {
    WebView webView;
    @SuppressLint("JavascriptInterface")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_wifi);
        init();
    }

    @SuppressLint("JavascriptInterface")
    private void init(){
        webView=(WebView) findViewById(R.id.show);
        webView.getSettings().setJavaScriptEnabled(true);
        webView.addJavascriptInterface(new InJavaScript(), "local");
        webView.loadUrl("http://202.196.64.132:8080");
        webView.setWebViewClient(new WebViewClient() {
            @Override
            public void onPageFinished(WebView view, String url) {
                super.onPageFinished(view, url);
                view.loadUrl("javascript:window.local.showSource(document.body.innerHTML);");
            }
        });
    }

    public class InJavaScript{
        @JavascriptInterface
        public void showSource(String html) {
            Log.i("====>html:",html);
        }
    }

}
{% endhighlight java %}