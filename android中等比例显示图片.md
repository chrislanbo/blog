layout: w
title: android中等比例显示图片
date: 2017-03-06 11:27:22
tags:
---

点击按钮
显示等比例显示的图片
main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#FFFFFFFF"
    android:orientation="vertical" >

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button" >
    </Button>

</LinearLayout>
```
image_dialog.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#FFFFFFFF"
    android:orientation="vertical" >

    <ImageView
        android:id="@+id/image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
        android:maxHeight="800dp"
        android:maxWidth="480dip"
        android:padding="1dp" />

</LinearLayout>
```
ImageDialogActivity.java
```java
import android.app.Activity;
import android.app.Dialog;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.view.WindowManager;
import android.widget.Button;
import android.widget.ImageView;
/**
 * 只显示图片的Dialog
 */
public class ImageDialogActivity extends Activity {
	private Dialog dialog;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        create();
        initButton();
    }
    private void initButton(){
    	Button button=(Button)findViewById(R.id.button1);
    	button.setOnClickListener(new Button.OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				show();				
			}
		});
    }
    private void show(){    	
    	dialog.show();
    }
    private void create(){
    	dialog=new Dialog(this);
    	dialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
    	dialog.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);
    	dialog.setContentView(R.layout.image_dialog);
    	initImageView();
    }
   
    private void initImageView(){
    	ImageView image=(ImageView)dialog.findViewById(R.id.image);
    	image.setImageResource(R.drawable.test);
    	image.setOnClickListener(new ImageView.OnClickListener() {
			
			@Override
			public void onClick(View arg0) {
				if(dialog!=null){
					dialog.dismiss();	
				}							
			}
		});
    }
}
```