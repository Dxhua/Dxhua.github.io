---
layout:     post                    # 使用的布局（不需要改）
title:     android中Radiobutton的使用               # 标题
subtitle:   模拟一个选择支付界面 #副标题
date:       2018-08-14             # 时间
author:     Dxhua                      # 作者
header-img: img/tag-bg.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - android
    - android控件
---

# android中Radiobutton的使用模拟一个选择支付的界面  
> 欢迎访问我的blog[The Blog of Dxhua](http://dxhua.top)  
> android中Radiobutton能实现在众多选择中实现单选，是一个很好的控件，在我们平常的开发中很多地方都会涉及到单选择的的场景
但是要实现单一选择，必须要和RadioGroup结合起来用，只有在一个RadioGrou中的RadioButton才能实现单选，否则每个按钮都是可以选择的。  
- 下面先贴出RadioButton的代码    
```
<RadioGroup
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:orientation="vertical"
       android:id="@+id/payWay_choose">
       <RadioButton
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:id="@+id/alipay"
           android:padding="20dp"
           android:drawableLeft="@drawable/alipay"
           android:text="支付宝支付"/>
       <RadioButton
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:id="@+id/weChat"
           android:padding="20dp"
           android:drawableLeft="@drawable/wechat"
           android:text="微信支付"/>
   </RadioGroup>
```       
在上面代码中我在一个RadioGroup中实现了两个RadioButton,分别是支付宝支付和微信支付，在其中android:drawable***方法可以来调整图片的位置，这还可以自己来试试，这里就不一一来展示了，的实现出来的效果如下图所示：  
![效果图](http://ovt2nfhfc.bkt.clouddn.com/RadioButton%E5%B1%95%E7%A4%BA%E5%9B%BE.png)  
在这里就只能单一选择支付方式了，在这里还能讲讲怎么拿到其他apk里面的图片，在我们的日常开发中，我们可能需要某些软件的icon图，  
在网上又不好找，当我们找不到是，可以把这个软件的apk下下来，然后将后缀名改成zip压缩文件格式在打开压缩文件，一般的icon图片就   
在res文件中，就拿支付宝来举例吧。
1. 首先将支付宝apk下下来，然后将.apk改成.zip  
![支付宝apk](http://ovt2nfhfc.bkt.clouddn.com/%E6%94%AF%E4%BB%98%E5%AE%9Dapk.png)![gaibian](http://ovt2nfhfc.bkt.clouddn.com/%E6%94%AF%E4%BB%98%E5%AE%9D%E5%8F%98zip%E6%96%87%E4%BB%B6.png)  
2.双击打开，找到res文件打开里面就有我们需要的图片了  
![zhifubaoneibu](http://ovt2nfhfc.bkt.clouddn.com/%E6%94%AF%E4%BB%98%E5%AE%9Dapk%E5%86%85%E6%96%87%E4%BB%B6.png)  
![zhifubaotupian](http://ovt2nfhfc.bkt.clouddn.com/appicon.png)  
3.基本的软件都是在res文件中，但是微信有点不同，他的icon图片在r文件中
![weinxinneibu](http://ovt2nfhfc.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1apk%E5%86%85%E9%83%A8.png)  
 好了，题外话说完了，继续我们的支付选择界面，下面附上完整的支付界面layout代码     
 ```
 <?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1">
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/text"
            android:text="这里可以加图片或者加上支付内容"
           android:layout_gravity="center"/>
    </LinearLayout>
    <View
        android:layout_width="match_parent"
        android:layout_height="0.1dp"
        android:layout_marginTop="5dp"
        android:background="#000" />

    <LinearLayout
    android:layout_width="wrap_content"
    android:layout_height="0dp"
    android:layout_weight="2">
   <RadioGroup
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:orientation="vertical"
       android:id="@+id/payWay_choose">
       <RadioButton
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:id="@+id/alipay"
           android:padding="20dp"
           android:drawableLeft="@drawable/alipay"
           android:text="支付宝支付"/>
       <RadioButton
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:id="@+id/weChat"
           android:padding="20dp"
           android:drawableLeft="@drawable/wechat"
           android:text="微信支付"/>

   </RadioGroup>
</LinearLayout>
    <View
        android:layout_width="match_parent"
        android:layout_height="0.1dp"
        android:layout_marginTop="5dp"
        android:background="#000" />

        <TextView
            android:id="@+id/sure"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            android:layout_marginTop="50dp"
            android:background="#4F94CD"
            android:gravity="center"
            android:paddingBottom="10dp"
            android:paddingTop="10dp"
            android:paddingStart="50dp"
            android:paddingEnd="50dp"
            android:text="确定"
            android:textColor="#CD0000"
            android:textSize="22sp" />

</LinearLayout>
 ```    
 这个布局实现的效果如下图：  
 ![支付界面](http://ovt2nfhfc.bkt.clouddn.com/%E6%A8%A1%E6%8B%9F%E6%94%AF%E4%BB%98%E7%95%8C%E9%9D%A2%E6%95%88%E6%9E%9C%E5%9B%BE.png)    
 接下来是一些简单的逻辑书写，实现选择不同的RadioButton实现不同的支付方式    
 ```
 package com.dxhua.payuiradiobuttontest;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    private RadioGroup radioGroup;
    private RadioButton alipay;
    private RadioButton weChat;
    private int payWay = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
        alipay = (RadioButton)findViewById(R.id.alipay);
        weChat = (RadioButton)findViewById(R.id.weChat);
        radioGroup = (RadioGroup)findViewById(R.id.payWay_choose);
        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                if (alipay.getId()==i){
                    payWay = 1;
                }
                if (weChat.getId()==i){
                    payWay = 2;
                }
            }
        });


    }
public void initView(){
        findViewById(R.id.sure).setOnClickListener(this);
        findViewById(R.id.text).setOnClickListener(this);

}


    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.sure:
                if (payWay==0){
                    Toast.makeText(this,"请选择支付方式",Toast.LENGTH_LONG).show();
                }else if (payWay==1){
                    Toast.makeText(this,"你选择了支付宝支付",Toast.LENGTH_LONG).show();
                }else if (payWay==2){
                    Toast.makeText(this,"你选择了微信支付",Toast.LENGTH_LONG).show();
                }
                break;
        }

    }
}
 ```    
 这样一个简单的支付选择界面就完成了，当然我们也可以加入其它的支付方式  效果如下，选择不同的支付方式，去往不同支付界面  
 ![不选择](http://ovt2nfhfc.bkt.clouddn.com/%E6%9C%AA%E9%80%89%E6%8B%A9%E6%94%AF%E4%BB%98.png)
 ![选择支付宝](http://ovt2nfhfc.bkt.clouddn.com/%E6%94%AF%E4%BB%98%E5%AE%9D%E6%94%AF%E4%BB%98.png)
 ![选择微信支付](http://ovt2nfhfc.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E6%94%AF%E4%BB%98.png)  
