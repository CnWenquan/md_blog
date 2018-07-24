---
title: Day09
url: 56.html
id: 56
categories:
  - Android课程总结
date: 2016-05-22 20:11:35
tags:
---

Android 09 总结  

----------------

**摘要**：异步任务接口回调、对话框、菜单、webView

**异步任务接口回调：**

1.  定义一个接口，且该接口中有一个抽象方法，且该方法至少有一个参数
    
2.  定义一个接口属性
    
3.  在传递数据的地方调用接口对象的方法，将需要传递数据作为参数
    
4.  定义一个set方法
    

**对话框：**

A：普通对话框：  

        AlertDialog:  

                AlertDialog.Builder builder  = new AlertDialog.Builder(mContext);
                builder.setIcon(R.mipmap.ic_launcher);
                builder.setTitle("提示：");
                builder.setMessage("确定退出吗？");
                builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        finish();
                    }
                });
                builder.setNegativeButton("取消", null);
                builder.show();

        DatePickerDialog:  

                Calendar calendar = Calendar.getInstance();
                int year = calendar.get(Calendar.YEAR);
                int month = calendar.get(Calendar.MONTH);
                int day = calendar.get(Calendar.DAY\_OF\_MONTH);

                DatePickerDialog datePickerDialog = new DatePickerDialog(mContext,
                        new DatePickerDialog.OnDateSetListener() {
                            @Override
                            public void onDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth) {
                                String dateInfo = year + "-" + (monthOfYear+1) + "-" + dayOfMonth;
                                Toast.makeText(mContext, dateInfo, Toast.LENGTH_SHORT).show();
                            }
                        } , year , month , day);
                datePickerDialog.show();

        TimePickerDialog:  

                Date date = new Date();
                int hour = date.getHours();
                int minute = date.getMinutes();
                TimePickerDialog timePickerDialog = new TimePickerDialog(mContext, new TimePickerDialog.OnTimeSetListener() {
                    @Override
                    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
                        String timeInfo = hourOfDay + ":" + minute;
                        Toast.makeText(mContext,timeInfo, Toast.LENGTH_SHORT).show();
                    }
                } , hour ,minute , true);
                timePickerDialog.show();

        自定义Dialog:  

                AlertDialog.Builder builder2 = new AlertDialog.Builder(mContext);
                builder2.setIcon(R.mipmap.ic_launcher);
                builder2.setTitle("用户登录");

                //填充自定义Dialog的布局文件
                //View view\_login = LayoutInflater.from(mContext).inflate(R.layout.dialog\_main_login , null);
                View view\_login = getLayoutInflater().inflate(R.layout.dialog\_main_login , null);
                //对Dialog自定义布局上的ui控件初始化
                final EditText editText\_dialog\_username = (EditText) view\_login.findViewById(R.id.editText\_dialog_username);
                final EditText editText\_dialog\_pwd = (EditText) view\_login.findViewById(R.id.editText\_dialog_pwd);
                builder2.setView(view_login);

                builder2.setPositiveButton("登录", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        String username = editText\_dialog\_username.getText()+"";
                        String pwd = editText\_dialog\_pwd.getText()+"";
                        Toast.makeText(mContext, username + ":::" + pwd, Toast.LENGTH_SHORT).show();
                    }
                });
                builder2.setNegativeButton("取消" , null);
                builder2.show();

B:    列表对话框(本质都是自定义对话框)：

**菜单**

A：选项菜单

        选项菜单被创建：onCreateOptionsMenu()  

        选项菜单被监听：onOptionsItemSelected()  

B：上下文菜单

    上下文菜单被创建：onCreateContextMenu()

    上下文菜单被监听：onContextItemSelected()  

C：PopupMenu

    初始化一个PopupMenu对象，传递两个参数，一个上下文对象、一个当前视图View  

    给当前view设置填充器  

    给PopupMenu设置监听器（setOneItemClickListener）  

    展示PopupMenu  

popupMenu = new PopupMenu(mContext,view);

popupMenu.getMenuInflater().inflate(R.menu.popupmenu_mian,popupMenu.getMenu());
//给popupMenu设置监听器
popupMenu.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
    @Override
    public boolean onMenuItemClick(MenuItem item) {

        switch (item.getItemId()){
            case R.id.action_width:
                setTitle(view.getWidth()+"");
                break;
            case R.id.action_height:
                setTitle(view.getHeight()+"");
                break;
        }
        return false;
    }
});
popupMenu.show();

  

      

D：PopupWindow（自定义菜单）

              

5、webView：

        ①：定义一个UI控件，并初始化  

        ②：让webView充当打开浏览器的控件：  

            WebView.setWebViewClient(new WebViewClient());  

        ③：支持特殊的JS：  

            WebView.getSettings().setJavaScriptEnabled(true);  

        ④：浏览器设置为Chrome  

            WebView.setWebChromeClient(new WebChromeClient());  

        ⑤：设置加载数据的方式：  

            WebView.loadUrl();  

            WebView.loadData();