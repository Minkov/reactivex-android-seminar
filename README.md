<!-- section start -->
<!-- attr: { id:'title', class:'slide-title', hasScriptWrapper:true } -->
# Asynchronous programming in Android with ReactiveX
##  Fuctional, Reactive, Async

<div class="signature">
    <p class="signature-course">Async Programming in Android with ReactiveX</p>
    <p class="signature-initiative">Telerik Academy Plus</p>
    <a href="http://academy.telerik.com" class="signature-link">http://academy.telerik.com</a>
</div>

<!--<img src="./imgs/Android_Reactive.jpeg" style="position: absolute;top: 210px;left: 0;height: 250px;border: 1px solid white;border-radius: 5px;" showInPresentation="true" />-->
<!-- section start -->

# Table of Contents

- Recap: Creating Async requests in Android
  - HTTP request to fetch data
- What is ReactiveX?
- Introducing RxAndroid
  - Observables
  - Observers
  - Schedules
- Creating Async request with ReactiveX

<!-- section start -->

<!-- attr: {hasScriptWrapper: true, style: "font-size: 0.9em"} -->
# Who am I?

<div style="vertical-align: top">
  <div style="width: 35%; display:inline-block">
    <img src="imgs/doncho-minkov.jpeg" style="width: 100%"/>
  </div>
  <div style="width: 60%; display: inline-block">
    <ul>
      <li>
        <strong>Doncho Minkov</strong>
        <ul>
          <li>
            Pricipal Technical Trainer <br/>@ Telerik Academy
          </li>
          <li>
            10+ years in IT
            <ul>
              <li>
                7 as a trainer in Telerik Academy
              </li>
              <li>
                Front-end developer by heart
                  <ul>
                    <li>
                      Software developer by need
                      <br/>
                      <br/>
                    </li>
                  </ul>
              </li>
            </ul>
          </li>
      </li>
    </ul>
  </div>
</div>

- Experience with all popular mobile platforms
  - Android, iOS, Windows
- Fluent in .NET, Node.js, Angular, Java, C++
- Deep knowledge of Data Structures and Algorithms

<!-- section start -->

<!-- attr: {class: "slide-section"} -->
# Running Async Code in Android
##  With built-in functionality

<!-- attr: {hasScriptWrapper: true} -->
# Async Code in Android

- How can async code be executed in Android?
  - It's simple:
    1.  Inherit the `AsyncTask<Params, Progress, Result>` class
    2.  Call the `execute(...params)` method
    3.  The code in the `doInBackground(...)` method will be executed asynchronously
  
<!-- attr: {hasScriptWrapper: true} -->
# Async Code in Android: <br/>Example
- _Example: _ Implementing an `AsyncTask<>`
  
```java
class MainActivity ... { 
  class MyAsyncTask extends AsyncTask<String, Void, String> {
    protected String doInBackground(String... params) {
      // Asynchronous code
    }
  }

  protected void onCreate(....) {
    /* Activity setup code */
    new MyAsyncTask()
      .execute(/* Async code params */);
  }
}
```

<!-- attr: {class: "slide-section"} -->
# Async Code in Android
##  Demo

<!-- attr: {style: "font-size: 0.9em"} -->
# AsyncTask<>

- `AsyncTask` is great, but...
  - Hard to reuse
  - Hard to test
  - Kind of messy
    - Lots of nesting....
  - One more nesting, when we need to update the UI
    - `runOnUiThread(new Runnable{ /* ... */ })`
  - Error handling is a nightmare
- There are ways to solve this
  - Create generic `AsyncTasks`, that can be reused and inject success/error callbacks
  - Or use **ReactiveX**

<!-- section start -->

<!-- attr: {class: "slide-section", hasScriptWrapper: true} -->
# <div style="vertical-align: middle;"><img src="./imgs/reactivex.png"  style="border:none;background: none;box-shadow: 0; height: 70px; margin: 0"/> <span>ReactiveX</span></div>
##  Making async hell in Android managable

<!-- attr: {style: "font-size: 0.80em" }-->
# What is ReactiveX?

- [ReactiveX](http://reactivex.io) is an easy way to handle async operations
  - Embraces paradigms from **functional** and **reactive** programming
  - **Data** and **event**-driven usage
  - Provides very good async **error handling**
    - Handles callback hell and stuff...
  - Async operations are **chained much like functions on iteratables** (arrays, lists)
    - `map()`, `.filter()`, etc...
  - Easy way to execute your code in **new threads**
  - **Lazy execution**
    - Async operations are not executed if an `observe()` method is not called
  - Support for many [languages and platforms](http://reactivex.io/languages.html)
    - Some are Java, C#, JavaScript/TypeScript, Python, C++, Swift

# ReactiveX components

- ReactiveX is built of three core components
  - `Observable`
    - Built with Observer object
    - Entry point for ~90% of the ReactiveX APIs
    - Emits changes from the async operation
  - `Observer`
    - Observable pattern
    - Observers on observables and react to changes
  - `Schedulers`
    - Control on which thread the async operation will be executed

<!-- attr: {style: "font-size: .9em", hasScriptWrapper: true}-->
# Using ReactiveX in an<br/>Android Application

1.  Go to [RxAndroid github page](https://github.com/ReactiveX/RxAndroid)
2.  Copy the **binaries** code into your graddle
3.  Start using RxAndroid

<div style="text-align: center">
  <div style="width: 49%; display: inline-block;margin: 0; font-size: 0.85em">
    <strong>Creating the Observable</strong>
    <pre><code class="lang-java hljs">Observable&lt;Integer> getObservable() {
  int index = 0;
  return Observable.create(e -> {
    while(true) {
      Thread.sleep(1000);
      e.onNext(++index);
    }
  });
}</code></pre>
  </div>
  <div style="width: 49%; display: inline-block;margin: 0; font-size: 0.85em">
    <strong>Using the Observable</strong>
    <pre><code class="lang-java hljs">getObservable()
  .subscribeOn(Schedulers.io())
  .observeOn(
    AndroidSchedulers.mainThread())
  .subscribe(value -> {
    /&#42; use the values &#42;/
  });


</code></pre>
  </div>
</div>
<!-- attr: {class:"slide-section" }-->
# Using RxAndroid
##  Demo

<!-- section start -->

<!-- attr: {class:"slide-section" }-->
# Fetching data from a web server with ReactiveX

<!-- attr: {hasScriptWrapper: true} -->
# Fetching data from a <br/>web server with ReactiveX

- We will use additional libraries:

<div style="text-align: center">
  <div style="width: 49%; display: inline-block;margin: 0; font-size: 0.8em">
    <strong>[OkHttp](http://square.github.io/okhttp/)</strong>
    <p style="font-size:0.7em">for creating HTTP requests</p>
    <pre><code>Request req = 
  new Request.Builder()
    .get()
    .url(url)
    .build()
  
Response res = 
  okHttpClient.newCall(req)
    .execute();

String json = res
  .body()
    .string();
</code></pre>
  </div>
  <div style="width: 49%; display: inline-block;margin: 0; font-size: 0.8em">
    <strong>[Gson](https://github.com/google/gson)</strong><p style="font-size:0.7em">for parsing the JSON to POJO</p>
    <pre><code>Gson gson = new Gson();

// from JSON to POJO
Book[] books = 
  gson.fromJson(json, Book[].class);

// from POJO to JSON
String jsonBook = 
  gson.toJson(bookObject);

// from array to JSON
String jsonBooksArray = 
  gson.toJson(booksArray);
</code></pre>
  </div>
</div>

<!-- attr: {class:"slide-section" }-->
 # Fetching data from a web server with ReactiveX
##  Demo

<!-- section start -->
<!-- attr: { id:'questions', class:'slide-section' } -->
# Questions
## Async programming in Android with ReactiveX
