---
title: 【Android解惑】RxBinding详解
date: 2017-04-21 15:31:35
tags: [android,rx,rxbinding]
categories: [Android解惑]
---

# RxBinding

>规范而强大的安卓UI响应式编程，让事件流程更清晰

JakeWharton 提供了一套在 Android 平台上的基于 RxJava的 Binding API。

UI中过多的监听事件，以及无规律的回调，会让你的页面（Fragment、Activity）变乱。
这样的UI响应费事费力。铺垫这么多，今天的主角RxBinding当然就能解决这个问题


看名字就知道支持RxJava语法，
小栗子：
 
```java
//button事件常规处理
Button b = (Button)findViewById(R.id.button);
b.setOnClickListener(new View.OnClickListener() {
       @Override
       public void onClick(View v) {
         // TODO   
       }
   });
        
//button事件RxBinding处理
Button b = (Button)findViewById(R.id.btn);
Subscription btnSub = 
RxView.clicks(b).subscribe(New Action<Void>(){
@Override
    public void call(Void aVoid) {
        // TODO
    }
});

```
<!-- more -->

还有一个栗子

```java
//EditText事件常规处理
final EditText name = (EditText) v.findViewById(R.id.name);
name.addTextChangedListener(new TextWatcher() {
    @Override
    public void beforeTextChanged(CharSequence s, int start, int count, int after{
    }
    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        // do some work here with the updated text
    }
    @Override
    public void afterTextChanged(Editable s) {
    }
});


//EditText事件RxBinding处理
final EditText name = (EditText) v.findViewById(R.id.name);
Subscription editTextSub =
    RxTextView.textChanges(name)
            .subscribe(new Action1<String>() {
                @Override
                public void call(String value) {
                    // do some work with the updated text
                }
            });
// Make sure to unsubscribe the subscription
```

这一点的改变并不是换汤不换药，使用RxBinding，这些监听事件的可以有一致的实现：RxJava的subscription


## 精确控制

针对EditText，需要的仅仅是onTextChanged()，使用RxBinding 的textChanges()方法仅仅对文本改变作出响应。

```java
//EditText的原始文本类型是CharSequence，获取并设置倒序的String类型的文本
final TextView nameLabel = (TextView) findViewById(R.id.name_label);
final EditText name = (EditText) findViewById(R.id.name);
Subscription editTextSub =
    RxTextView.textChanges(name)
            .map(new Func1<CharSequence, String>() {
                @Override
                public String call(CharSequence charSequence) {
                    return new StringBuilder(charSequence).reverse().toString();
                }
            })
            .subscribe(new Action1<String>() {
                @Override
                public void call(String value) {
                    nameLabel.setText(value);
                }
            });
```
每当EditText 文本发生改变，RxTextView.textChanges() 的 observable 被map() operator 转换成了返回值为String 的 observable，然后 subscription 将String类型的值显示在nameLabel上。


## 实现点击事件的多次监听

Android不支持同一个点击事件多次监听，RxBinding和RxJava结合很容易做到，

```java
Button b = (Button) v.findViewById(R.id.do_magic);
Observable<Void> clickObservable = RxView.clicks(b).share();//关键方法

Subscription buttonSub =
        clickObservable.subscribe(new Action1<Void>() {
            @Override
            public void call(Void aVoid) {
                // button was clicked. 
            }
        });
compositeSubscription.add(buttonSub);

Subscription loggingSub =
        clickObservable.subscribe(new Action1<Void>() {
            @Override
            public void call(Void aVoid) {
                // Button was clicked
            }
        });
compositeSubscription.add(loggingSub);

```
如果你把上面代码中的 .share() 移除的话，那只有最后一个subscription才能被回调。正如share()操作方法的文档描述一样：

返回一个新的Observable ，该Observable会广播给所有之前的。
在 context 中使用 share 允许对同一个button点击事件的多次监听，简直太强大了。


## 不可使用弱关联（弱引用）

>RxJava的subscription会做适当的拉近回收，弱关联可能会被回收掉。

## 只能返回一个参数

Android UI 事件内部接口可以返回多个参数，但是RxJava observables只能返回一个参数，如果有多个参数，需要进行封装传递

## 依赖问题

RxBinding对不同平台的类没有局限。这里的RxBinding库对Android支持库也有效。比如基本的平台类的RxBinding库依赖如下：
compile 'com.jakewharton.rxbinding:rxbinding:0.4.0'
假设使用 design support library ，想要RxBinding表现正常，可以添加该依赖：
compile 'com.jakewharton.rxbinding:rxbinding-design:0.4.0'
如果使用Kotlin，对于任何依赖简单地加上 -kotlin。例如：
compile 'com.jakewharton.rxbinding:rxbinding-kotlin:0.4.0'



Java8的 Lambda 表达式
在Lambda第三方支持插件里，比较推推崇：https://github.com/evant/gradle-retrolambda

![](http://img.blog.csdn.net/20160907093743054)   
![](http://img.blog.csdn.net/20160907093931444)

```java
public class MainActivity extends AppCompatActivity {
    Toolbar toolBar;
    EditText edit;
    TextView result;
    Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        init();
        logic();
    }

    private void init() {
        setSupportActionBar(toolBar);
        toolBar = (Toolbar) findViewById(R.id.toolBar);
        result = (TextView) findViewById(R.id.result);
        edit = (EditText) findViewById(R.id.edit);
        btn = (Button) findViewById(R.id.btn);
    }

    private void logic() {
        toolBar.setTitle("RxAndroidDemo");

        saveText();
        reFreshText();
    }

    private void reFreshText() {
        RxTextView.afterTextChangeEvents(edit).subscribe(textViewAfterTextChangeEvent -> {
            result.setText(textViewAfterTextChangeEvent.editable().toString());
        });
    }

    private void saveText() {
        RxView.clicks(btn)
//                .subscribeOn(Schedulers.io())
//                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Subscriber<Void>() {
    @Override
    public void onCompleted() {
                        result.setText(SharePreferencesTools.getString(MainActivity.this, "user") + " now ");
}

    @Override
    public void onError(Throwable e) {

    }

     @Override
     public void onNext(Void aVoid) {
         SharePreferencesTools.putString(MainActivity.this, "user", edit.getText().toString().trim());
         onCompleted();
     }
 });
    }
}

```

