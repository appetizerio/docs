title: 分析原理
---

# 分析原理
插桩包在运行后产生的log通过网络传到分析服务器后，服务器分析，筛选出有问题的情况，产生报告。

## 问题判定阈值

{% raw %}
<table>
<thead><tr><th>分类</th><th>项目</th><th>阈值</th><th>样例</th></tr></thead>
<tbody>
<tr><td>性能</td><td>主线程卡顿</td><td>100ms</td><td><img src="analysis-blocking.png" /></td></tr>
<tr><td>性能</td><td>主线程图片加载</td><td>40ms</td><td><img src="analysis-image.png" /></td></tr>
<tr><td>性能</td><td>主线程Layout构建</td><td>100ms</td><td><img src="analysis-layout.png" /></td></tr>
<tr>
<td>性能</td><td>页面Lifecycle</td>
<td>
  <b>(单位ms)</b>
  <pre lang="json">
  {
    "onCreate": 200,
    "onPause": 80,
    "onResume": 80,
    "onStart": 100,
    "onStop": 100,
    "onRestart": 100,
    "fragmentOnAttach": 100,
    "fragmentOnCreate": 100,
    "fragmentOnCreateView": 100,
    "fragmentOnActivityCreated": 100,
    "fragmentOnViewStateRestored": 100,
    "fragmentOnStart": 100,
    "fragmentOnResume": 100,
    "fragmentOnPause": 100,
    "fragmentOnStop": 100,
    "fragmentOnDestroyView": 100,
    "fragmentOnDestroy": 100,
    "fragmentOnDetach": 100
  }
  </pre>
</td>
<td><img src="analysis-lifecycle.png" /></td>
</tr>
<tr>
<td>网络</td><td>静态资源请求（返回image/*, text/*等）</td>
<td>首包150ms, 下载带宽200kB/s</td>
<td><img src="analysis-static-network.png" /></td>
</tr>
<tr>
<td>网络</td><td>接口请求（返回application/*等）</td>
<td>首包200ms, 下载带宽100kB/s</td>
<td><img src="analysis-dynamic-json.png" /></td>
</tr>
</tbody>
</table>
{% endraw %}

## 导出与二次分析csv
报告 - 导出为csv，数据格式如下：
```csv
Problem,URL,Method,Latency(ms),Transmission Time(ms),API Type,Status Code,Content Type,Content Length(Byte),Cacheable
"Slow transmission","http://i0.hdslb.com/bfs/archive/ae89aaec646b3114c0218531091bff20a93c2200.jpg",GET,1,109,urlconnection,200,image/jpeg,15323,false
"Slow transmission","http://i0.hdslb.com/bfs/live/e5cc973e0f9b7328ed2324cdbd899a3cce4dd59c.jpg",GET,0,144,urlconnection,200,image/jpeg,27058,false
"Slow transmission","http://i1.hdslb.com/bfs/archive/6381d2c7b878ae299e19b486c9779ec27023101b.jpg",GET,0,2142,urlconnection,200,image/jpeg,53929,false
"Slow transmission","http://i0.hdslb.com/bfs/archive/6aca57a8047a68ed639146554773c0d5ca0a5097.jpg",GET,28,366,urlconnection,200,image/jpeg,66405,false
"High latency","http://www.bilibili.com/index/rank/all-03-160.json",GET,230,12,okhttp3,200,application/json; charset=UTF-8,0,false
"High latency","http://www.bilibili.com/index/rank/all-03-5.json",GET,219,12,okhttp3,200,application/json; charset=UTF-8,0,false
```

## 导出与二次分析json
报告 - 导出为json，数据格式如下：
```json
{
  "allItems": [
    {
      "affected": 0,
      "brief": "Invoking a lengthy Android API inflate in the UI thread, occupying 37ms",
      "category": "perf",
      "elapsed_time": 37,
      "end": 1523959147100,
      "method": "inflate",
      "session": {
        "session_index": 0,
        "session_step": 21,
        "session_ticker": 5
      },
      "start": 1523959147063,
      "thread": 1,
      "thread_name": "main"
    },
    {
      "affected": 0,
      "brief": "Invoking a lengthy Android API setContentView in the UI thread, occupying 41ms",
      "category": "perf",
      "elapsed_time": 41,
      "end": 1523959147100,
      "method": "setContentView",
      "session": {
        "session_index": 0,
        "session_step": 21,
        "session_ticker": 5
      },
      "start": 1523959147059,
      "thread": 1,
      "thread_name": "main"
    },
    {
      "affected": 0,
      "brief": "Invoking a lengthy Android API setImageResource in the UI thread, occupying 40ms",
      "category": "perf",
      "elapsed_time": 40,
      "end": 1523959147141,
      "highlight": [
        {
          "filename": "MainActivity.java",
          "line": "119",
          "method": "com.hotbitmapgg.bilibili.module.common.MainActivity.initNavigationView"
        },
        {
          "filename": "MainActivity.java",
          "line": "73",
          "method": "com.hotbitmapgg.bilibili.module.common.MainActivity.initViews"
        },
        {
          "filename": "RxBaseActivity.java",
          "line": "40",
          "method": "com.hotbitmapgg.bilibili.base.RxBaseActivity.onCreate"
        },
        {
          "filename": "Activity.java",
          "line": "6915",
          "method": "android.app.Activity.performCreate"
        },
        {
          "filename": "Instrumentation.java",
          "line": "1123",
          "method": "android.app.Instrumentation.callActivityOnCreate"
        },
        {
          "filename": "ActivityThread.java",
          "line": "2746",
          "method": "android.app.ActivityThread.performLaunchActivity"
        },
        {
          "filename": "ActivityThread.java",
          "line": "2864",
          "method": "android.app.ActivityThread.handleLaunchActivity"
        },
        {
          "filename": "Native Method",
          "line": "Native Method",
          "method": "android.app.ActivityThread.-wrap12"
        },
        {
          "filename": "ActivityThread.java",
          "line": "1567",
          "method": "android.app.ActivityThread.H.handleMessage"
        },
        {
          "filename": "Handler.java",
          "line": "105",
          "method": "android.os.Handler.dispatchMessage"
        },
        {
          "filename": "Looper.java",
          "line": "156",
          "method": "android.os.Looper.loop"
        },
        {
          "filename": "ActivityThread.java",
          "line": "6523",
          "method": "android.app.ActivityThread.main"
        },
        {
          "filename": "Native Method",
          "line": "Native Method",
          "method": "java.lang.reflect.Method.invoke"
        },
        {
          "filename": "ZygoteInit.java",
          "line": "942",
          "method": "com.android.internal.os.ZygoteInit.MethodAndArgsCaller.run"
        },
        {
          "filename": "ZygoteInit.java",
          "line": "832",
          "method": "com.android.internal.os.ZygoteInit.main"
        }
      ],
      "method": "setImageResource",
      "session": {
        "session_index": 0,
        "session_step": 21,
        "session_ticker": 5
      },
      "start": 1523959147101,
      "thread": 1,
      "thread_name": "main"
    },
    {
      "affected": 0,
      "api_type": "okhttp3",
      "brief": "http://s.search.bilibili.com/main/hotword",
      "cacheable": false,
      "category": "http",
      "compressable": true,
      "elapsed_time": 81,
      "end_time": 1523959147379,
      "highlight": [
        {
          "filename": "RealCall.java",
          "line": "187",
          "method": "okhttp3.RealCall.ApplicationInterceptorChain.proceed"
        },
        {
          "filename": "RealCall.java",
          "line": "160",
          "method": "okhttp3.RealCall.getResponseWithInterceptorChain"
        },
        {
          "filename": "RealCall.java",
          "line": "57",
          "method": "okhttp3.RealCall.execute"
        },
        {
          "filename": "OkHttpCall.java",
          "line": "177",
          "method": "retrofit2.OkHttpCall.execute"
        },
        {
          "filename": "RxJavaCallAdapterFactory.java",
          "line": "111",
          "method": "retrofit2.adapter.rxjava.RxJavaCallAdapterFactory.CallOnSubscribe.call"
        },
        {
          "filename": "RxJavaCallAdapterFactory.java",
          "line": "92",
          "method": "retrofit2.adapter.rxjava.RxJavaCallAdapterFactory.CallOnSubscribe.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "50",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "30",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "50",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "30",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "50",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "30",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "50",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "OnSubscribeLift.java",
          "line": "30",
          "method": "rx.internal.operators.OnSubscribeLift.call"
        },
        {
          "filename": "Observable.java",
          "line": "8452",
          "method": "rx.Observable.unsafeSubscribe"
        },
        {
          "filename": "OperatorSubscribeOn.java",
          "line": "94",
          "method": "rx.internal.operators.OperatorSubscribeOn.1.call"
        },
        {
          "filename": "CachedThreadScheduler.java",
          "line": "222",
          "method": "rx.internal.schedulers.CachedThreadScheduler.EventLoopWorker.1.call"
        },
        {
          "filename": "ScheduledAction.java",
          "line": "55",
          "method": "rx.internal.schedulers.ScheduledAction.run"
        },
        {
          "filename": "Executors.java",
          "line": "428",
          "method": "java.util.concurrent.Executors.RunnableAdapter.call"
        },
        {
          "filename": "FutureTask.java",
          "line": "237",
          "method": "java.util.concurrent.FutureTask.run"
        },
        {
          "filename": "ScheduledThreadPoolExecutor.java",
          "line": "272",
          "method": "java.util.concurrent.ScheduledThreadPoolExecutor.ScheduledFutureTask.run"
        },
        {
          "filename": "ThreadPoolExecutor.java",
          "line": "1133",
          "method": "java.util.concurrent.ThreadPoolExecutor.runWorker"
        },
        {
          "filename": "ThreadPoolExecutor.java",
          "line": "607",
          "method": "java.util.concurrent.ThreadPoolExecutor.Worker.run"
        },
        {
          "filename": "Thread.java",
          "line": "776",
          "method": "java.lang.Thread.run"
        }
      ],
      "method": "GET",
      "read_finished_time": 1523959147381,
      "request": {
        "content_length": 0,
        "header": {},
        "header_size": 0
      },
      "response": {
        "cache_control": {
          "max-age": "3600",
          "public": true
        },
        "content_encoding": "identity",
        "content_length": 0,
        "content_type": {
          "MIME": "application/json",
          "boundary": "",
          "charset": "unknown"
        },
        "header": {
          "Access-Control-Allow-Headers": "origin, authorization, accept",
          "Access-Control-Allow-Methods": "GET, OPTIONS",
          "Access-Control-Allow-Origin": "*",
          "Cache-Control": "public, max-age=3600",
          "Connection": "keep-alive",
          "Content-Type": "application/json; charset=UTF-8",
          "Date": "Tue, 17 Apr 2018 09:59:09 GMT",
          "Etag": "\"1107b290b1968bdb8d819685aad1addcb98ac76b\"",
          "Expires": "Tue, 17 Apr 2018 09:59:39 GMT",
          "OkHttp-Received-Millis": "1523959147374",
          "OkHttp-Sent-Millis": "1523959147330",
          "Server": "Tengine",
          "Vary": "Accept-Encoding",
          "X-Cache": "MISS from cn-jsnt-dx-w-01.hdslb.com",
          "X-TKID": "1523959149057551794424"
        },
        "header_size": 549,
        "status_code": 200
      },
      "session": {
        "session_index": 0,
        "session_step": 25,
        "session_ticker": 5
      },
      "start_time": 1523959147298,
      "transmit_time": 2,
      "url": "http://s.search.bilibili.com/main/hotword?access_key=ec0f54fc369d8c104ee1068672975d6a&actionKey=appkey&appkey=27eb53fc9058f8c3"
    },
  ]
}
```