---
title: Day10
url: 69.html
id: 69
categories:
  - Android课程总结
date: 2016-05-27 20:34:00
tags:
---

Android10 总结
------------

**摘要**：InternalStorage、外部存储

**InternalStorage：内部存储**

getCacheDir()：获取内存的缓存路径

getFilesDir():获取内存文件存储路径

openFileInput():读取输入流

openFileOutput():将输出流写入文件

public class MainActivity extends Activity {
    private Context mContext = this;
    private EditText editText_username;
    private EditText editText_number;
    private TextView textView_info;
    private String fileName = "myinfo";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();
    }

    private void initView() {
        editText\_username = (EditText) findViewById(R.id.editText\_username);
        editText\_number = (EditText) findViewById(R.id.editText\_number);
        textView\_info = (TextView) findViewById(R.id.textView\_info);
    }

    public void clickButton(View view) {
        switch (view.getId()) {
            case R.id.button_store:
                String content = editText_username.getText().toString() + ":"
                        + editText_number.getText().toString();
                // 保存的路径是：/data/data/包名/Files
                try {
                    FileOutputStream fos = openFileOutput(fileName,
                     Context.MODE\_PRIVATE | Context.MODE\_APPEND);
                    //将输出流写入文件
                    boolean flag = IOHelper.writeFileOutputStream(content.getBytes(), 
                    fos);
                    Toast.makeText(mContext, flag + "", Toast.LENGTH_SHORT).show();
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                }
                break;
            case R.id.button_restore:
                BufferedInputStream bis = null;
                try {
                    FileInputStream fis = openFileInput(fileName);
                    //读取输入流
                    byte\[\] data = IOHelper.readFileInputStream(fis);

                    textView_info.setText(new String(data));

                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                }
                break;
        }
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }

}

  

  

  

**外部存储：**

1、外部存储的存储目录：

1.  9大公有目录
    
2.  私有目录
    

1.  私有cache目录
    
2.  私有files目录（内置了7个目录）
    

4.  SD卡工具类的目录
    
5.  存储Bitmap图片到外部存储