---
title: 阿里消息服务mns --Android
tags:
  - Android
url: 350.html
id: 350
categories:
  - Android
date: 2016-11-11 15:47:16
---

概念在php中已经写过了，直接上android：

准备：

    Android Studio

    导入第三方jar包：（下载地址）

[http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43477/cn\_zh/1472189354580/aliyun\_MNS\_Android\_SDK\_20160826.zip?spm=5176.doc43478.2.1.Or4xPd&file=aliyun\_MNS\_Android\_SDK_20160826.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43477/cn_zh/1472189354580/aliyun_MNS_Android_SDK_20160826.zip?spm=5176.doc43478.2.1.Or4xPd&file=aliyun_MNS_Android_SDK_20160826.zip)  

compile 'com.squareup.okio:okio:1.11.0'
compile 'com.squareup.okhttp3:okhttp:3.4.2'
compile files('libs/aliyun-oss-sdk-android-2.2.2.jar')

* * *

配置Config：

package com.example.a123.testmnsbyalia;

import com.alibaba.sdk.android.common.ClientException;
import com.alibaba.sdk.android.common.ServiceException;
import com.alibaba.sdk.android.common.auth.CredentialProvider;
import com.alibaba.sdk.android.common.auth.PlainTextAKSKCredentialProvider;
import com.alibaba.sdk.android.mns.callback.MNSCompletedCallback;
import com.alibaba.sdk.android.mns.model.request.ChangeMessageVisibilityRequest;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteMessageRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.request.GetQueueAttributesRequest;
import com.alibaba.sdk.android.mns.model.request.ListQueueRequest;
import com.alibaba.sdk.android.mns.model.request.PeekMessageRequest;
import com.alibaba.sdk.android.mns.model.request.ReceiveMessageRequest;
import com.alibaba.sdk.android.mns.model.request.SendMessageRequest;
import com.alibaba.sdk.android.mns.model.request.SetQueueAttributesRequest;
import com.alibaba.sdk.android.mns.model.result.ChangeMessageVisibilityResult;
import com.alibaba.sdk.android.mns.model.result.CreateQueueResult;
import com.alibaba.sdk.android.mns.model.result.DeleteMessageResult;
import com.alibaba.sdk.android.mns.model.result.DeleteQueueResult;
import com.alibaba.sdk.android.mns.model.result.GetQueueAttributesResult;
import com.alibaba.sdk.android.mns.model.result.ListQueueResult;
import com.alibaba.sdk.android.mns.model.result.PeekMessageResult;
import com.alibaba.sdk.android.mns.model.result.ReceiveMessageResult;
import com.alibaba.sdk.android.mns.model.result.SendMessageResult;
import com.alibaba.sdk.android.mns.model.result.SetQueueAttributesResult;

/**
 * Created by wenquan on 2016/11/11.
 */
public class MNSTestConfig {
    public static final String ENDPOINT = "http://1436212568237182.mns.cn-beijing.aliyuncs.com/";

    public static final String AK = "QF3ON8fkoflbIyTV";

    public static final String SK = "gSdE5RXEy7tgwPGWyR6DZ6yXeCseyl";

    public static final CredentialProvider credentialProvider = newAKSKCredentialProvider();

    public static final String QUEUE_NAME = "AndroidQueueTest";

    public static CredentialProvider newAKSKCredentialProvider() {
        return new PlainTextAKSKCredentialProvider(AK, SK);
    }

    public final static class TestCreateQueueCallback implements MNSCompletedCallback<CreateQueueRequest, CreateQueueResult> {
        public CreateQueueRequest request;
        public CreateQueueResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(CreateQueueRequest request, CreateQueueResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(CreateQueueRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestDeleteQueueCallback implements MNSCompletedCallback<DeleteQueueRequest, DeleteQueueResult> {
        public DeleteQueueRequest request;
        public DeleteQueueResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(DeleteQueueRequest request, DeleteQueueResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(DeleteQueueRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestGetQueueAttributesCallback implements MNSCompletedCallback<GetQueueAttributesRequest, GetQueueAttributesResult> {
        public GetQueueAttributesRequest request;
        public GetQueueAttributesResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(GetQueueAttributesRequest request, GetQueueAttributesResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(GetQueueAttributesRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestSetQueueAttributesCallback implements MNSCompletedCallback<SetQueueAttributesRequest, SetQueueAttributesResult> {
        public SetQueueAttributesRequest request;
        public SetQueueAttributesResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(SetQueueAttributesRequest request, SetQueueAttributesResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(SetQueueAttributesRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestListQueueCallback implements MNSCompletedCallback<ListQueueRequest, ListQueueResult> {
        public ListQueueRequest request;
        public ListQueueResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(ListQueueRequest request, ListQueueResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(ListQueueRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestSendMessageCallback implements MNSCompletedCallback<SendMessageRequest, SendMessageResult> {
        public SendMessageRequest request;
        public SendMessageResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(SendMessageRequest request, SendMessageResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(SendMessageRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestReceiveMessageCallback implements MNSCompletedCallback<ReceiveMessageRequest, ReceiveMessageResult> {
        public ReceiveMessageRequest request;
        public ReceiveMessageResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(ReceiveMessageRequest request, ReceiveMessageResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(ReceiveMessageRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestDeleteMessageCallback implements MNSCompletedCallback<DeleteMessageRequest, DeleteMessageResult> {
        public DeleteMessageRequest request;
        public DeleteMessageResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(DeleteMessageRequest request, DeleteMessageResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(DeleteMessageRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestPeekMessageCallback implements MNSCompletedCallback<PeekMessageRequest, PeekMessageResult> {
        public PeekMessageRequest request;
        public PeekMessageResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(PeekMessageRequest request, PeekMessageResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(PeekMessageRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }

    public final static class TestChangeMessageVisibilityCallback implements MNSCompletedCallback<ChangeMessageVisibilityRequest, ChangeMessageVisibilityResult> {
        public ChangeMessageVisibilityRequest request;
        public ChangeMessageVisibilityResult result;
        public ClientException clientException;
        public ServiceException serviceException;

        @Override
        public void onSuccess(ChangeMessageVisibilityRequest request, ChangeMessageVisibilityResult result){
            System.out.println("OnSuccess");
            this.request = request;
            this.result = result;
        }

        @Override
        public void onFailure(ChangeMessageVisibilityRequest request, ClientException clientException, ServiceException serviceException)
        {
            System.out.println("OnFail");
            this.request = request;
            this.clientException = clientException;
            this.serviceException = serviceException;
        }
    }
}

  

* * *

创建、删除队列：

    首先写一个类，封装好创建队列、删除队列的方法：  

package com.example.a123.testmnsbyalia;

import android.test.AndroidTestCase;

import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.common.MNSLog;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.QueueMeta;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;

/**
 * Created by wenquan on 2016/11/11.
 */
public class CreateQueueTest extends AndroidTestCase {
        MNS mns;

        @Override
        public void setUp() throws Exception {
            if (mns == null) {
                Thread.sleep(5 * 1000);
                MNSLog.enableLog();
                mns = new MNSClient(getContext(), MNSTestConfig.ENDPOINT, MNSTestConfig.credentialProvider);
            }
        }

        public void testCreateQueue() throws Exception {
            CreateQueueRequest request = new CreateQueueRequest(MNSTestConfig.QUEUE_NAME);
            QueueMeta queueMeta = new QueueMeta();
            queueMeta.setVisibilityTimeout(60L);
            queueMeta.setLoggingEnabled(1);
            request.setQueueMeta(queueMeta);
            MNSTestConfig.TestCreateQueueCallback callback = new MNSTestConfig.TestCreateQueueCallback();
            MNSAsyncTask task = mns.asyncCreateQueue(request, callback);
            task.waitUntilFinished();
            assertNull(callback.serviceException);
            assertNull(callback.clientException);
            assertTrue(callback.result.getStatusCode() == 204 || callback.result.getStatusCode() == 201 );


        }

    public void testDeleteQueue() throws Exception{
            DeleteQueueRequest deleteRequest = new DeleteQueueRequest(MNSTestConfig.QUEUE_NAME);
            MNSTestConfig.TestDeleteQueueCallback deleteQueueCallback = new MNSTestConfig.TestDeleteQueueCallback();
            MNSAsyncTask deleteTask = mns.asyncDeleteQueue(deleteRequest, deleteQueueCallback);
            deleteTask.waitUntilFinished();
            assertNull(deleteQueueCallback.serviceException);
            assertNull(deleteQueueCallback.clientException);
            assertEquals(204, deleteQueueCallback.result.getStatusCode());
    }
}

    其次：UI线程调用：

package com.example.a123.testmnsbyalia;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.alibaba.sdk.android.common.ClientConfiguration;
import com.alibaba.sdk.android.common.ClientException;
import com.alibaba.sdk.android.common.ServiceException;
import com.alibaba.sdk.android.common.auth.CredentialProvider;
import com.alibaba.sdk.android.common.auth.PlainTextAKSKCredentialProvider;
import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.callback.MNSCompletedCallback;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.result.CreateQueueResult;

import static junit.framework.Assert.assertEquals;
import static junit.framework.Assert.assertNull;

public class MainActivity extends AppCompatActivity {

    private String TAG = "MainActivity";
    CreateQueueTest createTest = new CreateQueueTest();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initMns(); //初始化mns

        createMnsQueue();//创建队列

        deleteMnsQueue();//删除队列
    }

    /*
    删除队列
     */
    private void deleteMnsQueue() {
        try {
            createTest.testDeleteQueue();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void initMns() {
        try {
            createTest.setUp();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    /*
    创建队列
     */
    private void createMnsQueue() {
        try {
            createTest.testCreateQueue();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

* * *

发送消息：

    首先，写一个MessageTest类：

package com.example.a123.testmnsbyalia;

import android.test.AndroidTestCase;

import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.common.MNSLog;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.Message;
import com.alibaba.sdk.android.mns.model.QueueMeta;
import com.alibaba.sdk.android.mns.model.request.ChangeMessageVisibilityRequest;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteMessageRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.request.PeekMessageRequest;
import com.alibaba.sdk.android.mns.model.request.ReceiveMessageRequest;
import com.alibaba.sdk.android.mns.model.request.SendMessageRequest;

/**
 * Created by wenquan on 2016/11/11.
 */
public class MessageTest extends AndroidTestCase {
    MNS mns;

    @Override
    public void setUp() throws Exception {
        if (mns == null) {
            Thread.sleep(5 * 1000);
            MNSLog.enableLog();
            mns = new MNSClient(getContext(), MNSTestConfig.ENDPOINT, MNSTestConfig.credentialProvider);
        }

        CreateQueueRequest request = new CreateQueueRequest(MNSTestConfig.QUEUE_NAME);
        QueueMeta queueMeta = new QueueMeta();
        queueMeta.setVisibilityTimeout(60L);
        queueMeta.setLoggingEnabled(1);
        request.setQueueMeta(queueMeta);
        MNSTestConfig.TestCreateQueueCallback callback = new MNSTestConfig.TestCreateQueueCallback();
        MNSAsyncTask task = mns.asyncCreateQueue(request, callback);
        task.waitUntilFinished();
        assertNull(callback.serviceException);
        assertNull(callback.clientException);
        assertTrue(callback.result.getStatusCode() == 204 || callback.result.getStatusCode() == 201 );
    }

    @Override
    public void tearDown() throws Exception {
        DeleteQueueRequest deleteRequest = new DeleteQueueRequest(MNSTestConfig.QUEUE_NAME);
        MNSTestConfig.TestDeleteQueueCallback deleteQueueCallback = new MNSTestConfig.TestDeleteQueueCallback();
        MNSAsyncTask deleteTask = mns.asyncDeleteQueue(deleteRequest, deleteQueueCallback);
        deleteTask.waitUntilFinished();
        assertNull(deleteQueueCallback.serviceException);
        assertNull(deleteQueueCallback.clientException);
        assertEquals(204, deleteQueueCallback.result.getStatusCode());
    }

    public void testSendAndReceiveMessage() throws Exception {
       // SendMessage Test Case
        SendMessageRequest sendRequest = new SendMessageRequest(MNSTestConfig.QUEUE_NAME);
        Message message = new Message();
        message.setMessageBody("TestBody");
        sendRequest.setMessage(message);
        MNSTestConfig.TestSendMessageCallback sendMessageCallback = new MNSTestConfig.TestSendMessageCallback();
        MNSAsyncTask sendMessageTask = mns.asyncSendMessage(sendRequest, sendMessageCallback);
        sendMessageTask.waitUntilFinished();
        assertNull(sendMessageCallback.clientException);
        assertNull(sendMessageCallback.serviceException);
        assertEquals(201, sendMessageCallback.result.getStatusCode());
        assertTrue("F5DCCA6A22CBF7D6A59766721DAEFA44".equals(sendMessageCallback.result.getMessageBodyMd5()));

        // PeekMessage Test Case
        PeekMessageRequest peekRequest = new PeekMessageRequest(MNSTestConfig.QUEUE_NAME);
        MNSTestConfig.TestPeekMessageCallback peekMessageCallback = new MNSTestConfig.TestPeekMessageCallback();
        MNSAsyncTask peekMessageTask = mns.asyncPeekMessage(peekRequest, peekMessageCallback);
        peekMessageTask.waitUntilFinished();;
        assertNull(peekMessageCallback.clientException);
        assertNull(peekMessageCallback.serviceException);
        assertEquals(200, peekMessageCallback.result.getStatusCode());
        assertNull(peekMessageCallback.result.getMessage().getReceiptHandle());
        assertTrue("TestBody".equals(peekMessageCallback.result.getMessage().getMessageBody()));

        // ReceiveMessage Test Case
        ReceiveMessageRequest receiveRequest = new ReceiveMessageRequest(MNSTestConfig.QUEUE_NAME);
        MNSTestConfig.TestReceiveMessageCallback receiveMessageCallback = new MNSTestConfig.TestReceiveMessageCallback();
        MNSAsyncTask receiveMessageTask = mns.asyncReceiveMessage(receiveRequest, receiveMessageCallback);
        receiveMessageTask.waitUntilFinished();;
        assertNull(receiveMessageCallback.clientException);
        assertNull(receiveMessageCallback.serviceException);
        assertEquals(200, receiveMessageCallback.result.getStatusCode());
        System.out.println(receiveMessageCallback.result.getMessage());
        assertTrue("TestBody".equals(receiveMessageCallback.result.getMessage().getMessageBody()));

        // ChangeMessageVisibility Test Case
        ChangeMessageVisibilityRequest changeRequest = new ChangeMessageVisibilityRequest(MNSTestConfig.QUEUE_NAME,
                                                            receiveMessageCallback.result.getMessage().getReceiptHandle(), 50);
        MNSTestConfig.TestChangeMessageVisibilityCallback changeCallback = new MNSTestConfig.TestChangeMessageVisibilityCallback();
        MNSAsyncTask changeTask = mns.asyncChangeMessageVisibility(changeRequest, changeCallback);
        changeTask.waitUntilFinished();
        assertNull(changeCallback.clientException);
        assertNull(changeCallback.serviceException);
        assertEquals(200, changeCallback.result.getStatusCode());

        // DeleteMessage Test Case
//        DeleteMessageRequest deleteRequest = new DeleteMessageRequest(MNSTestConfig.QUEUE_NAME, changeCallback.result.getReceiptHandle());
//        MNSTestConfig.TestDeleteMessageCallback deleteMessageCallback = new MNSTestConfig.TestDeleteMessageCallback();
//        MNSAsyncTask deleteMessageTask = mns.asyncDeleteMessage(deleteRequest, deleteMessageCallback);
//        deleteMessageTask.waitUntilFinished();
//        assertNull(deleteMessageCallback.clientException);
//        assertNull(deleteMessageCallback.serviceException);
//        assertEquals(204, deleteMessageCallback.result.getStatusCode());
    }

    public void testDeleteMessage() throws Exception {
        MNSTestConfig.TestChangeMessageVisibilityCallback changeCallback = new MNSTestConfig.TestChangeMessageVisibilityCallback();
        DeleteMessageRequest deleteRequest = new DeleteMessageRequest(MNSTestConfig.QUEUE_NAME, changeCallback.result.getReceiptHandle());
        MNSTestConfig.TestDeleteMessageCallback deleteMessageCallback = new MNSTestConfig.TestDeleteMessageCallback();
        MNSAsyncTask deleteMessageTask = mns.asyncDeleteMessage(deleteRequest, deleteMessageCallback);
        deleteMessageTask.waitUntilFinished();
        assertNull(deleteMessageCallback.clientException);
        assertNull(deleteMessageCallback.serviceException);
        assertEquals(204, deleteMessageCallback.result.getStatusCode());
    }


    }

  

    其次：UI线程调用

package com.example.a123.testmnsbyalia;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.alibaba.sdk.android.common.ClientConfiguration;
import com.alibaba.sdk.android.common.ClientException;
import com.alibaba.sdk.android.common.ServiceException;
import com.alibaba.sdk.android.common.auth.CredentialProvider;
import com.alibaba.sdk.android.common.auth.PlainTextAKSKCredentialProvider;
import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.callback.MNSCompletedCallback;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.result.CreateQueueResult;

import static junit.framework.Assert.assertEquals;
import static junit.framework.Assert.assertNull;

public class MainActivity extends AppCompatActivity {

    private String TAG = "MainActivity";
    MessageTest messageTest = new MessageTest();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initMns(); //初始化mns

        sendMnsMessage();//发送消息

    }

    private void initMns() {
        try {
            messageTest.setUp();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMnsMessage() {
        try {
            messageTest.testSendAndReceiveMessage();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }






}

* * *

接收消息：

    首先，写一个接收消息的类  

package com.example.a123.testmnsbyalia;

import android.test.AndroidTestCase;

import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.common.MNSLog;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.QueueMeta;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.request.GetQueueAttributesRequest;
import com.alibaba.sdk.android.mns.model.request.SetQueueAttributesRequest;

/**
 * Created by wenquan on 2016/11/11.
 */
public class QueueAttrTest extends AndroidTestCase {
    MNS mns;

    @Override
    public void setUp() throws Exception {
        if (mns == null) {
            Thread.sleep(5 * 1000);
            MNSLog.enableLog();
            mns = new MNSClient(getContext(), MNSTestConfig.ENDPOINT, MNSTestConfig.credentialProvider);
        }

        CreateQueueRequest request = new CreateQueueRequest(MNSTestConfig.QUEUE_NAME);
        QueueMeta queueMeta = new QueueMeta();
        queueMeta.setVisibilityTimeout(60L);
        queueMeta.setLoggingEnabled(1);
        request.setQueueMeta(queueMeta);
        MNSTestConfig.TestCreateQueueCallback callback = new MNSTestConfig.TestCreateQueueCallback();
        MNSAsyncTask task = mns.asyncCreateQueue(request, callback);
        task.waitUntilFinished();
        assertNull(callback.serviceException);
        assertNull(callback.clientException);
        assertTrue(callback.result.getStatusCode() == 204 || callback.result.getStatusCode() == 201 );
    }

    @Override
    public void tearDown() throws Exception {
        DeleteQueueRequest deleteRequest = new DeleteQueueRequest(MNSTestConfig.QUEUE_NAME);
        MNSTestConfig.TestDeleteQueueCallback deleteQueueCallback = new MNSTestConfig.TestDeleteQueueCallback();
        MNSAsyncTask deleteTask = mns.asyncDeleteQueue(deleteRequest, deleteQueueCallback);
        deleteTask.waitUntilFinished();
        assertNull(deleteQueueCallback.serviceException);
        assertNull(deleteQueueCallback.clientException);
        assertEquals(204, deleteQueueCallback.result.getStatusCode());
    }

    public void testSetAndGetQueueAttr() throws Exception {
        SetQueueAttributesRequest setRequest = new SetQueueAttributesRequest(MNSTestConfig.QUEUE_NAME);
        QueueMeta queueMeta = new QueueMeta();
        queueMeta.setVisibilityTimeout(10L);
        setRequest.setQueueMeta(queueMeta);
        MNSTestConfig.TestSetQueueAttributesCallback setQueueAttrCallback = new MNSTestConfig.TestSetQueueAttributesCallback();
        MNSAsyncTask setQueueAttrTask = mns.asyncSetQueueAttributes(setRequest, setQueueAttrCallback);
        setQueueAttrTask.waitUntilFinished();
        assertNull(setQueueAttrCallback.clientException);
        assertNull(setQueueAttrCallback.serviceException);

        GetQueueAttributesRequest getRequest = new GetQueueAttributesRequest(MNSTestConfig.QUEUE_NAME);
        MNSTestConfig.TestGetQueueAttributesCallback getQueueAttrCallback = new MNSTestConfig.TestGetQueueAttributesCallback();
        MNSAsyncTask getQueueAttrTask = mns.asyncGetQueueAttributes(getRequest, getQueueAttrCallback);
        getQueueAttrTask.waitUntilFinished();
        assertNull(getQueueAttrCallback.clientException);
        assertNull(getQueueAttrCallback.serviceException);
        QueueMeta getQueueMeta = getQueueAttrCallback.result.getQueueMeta();
        assertEquals(10L, (long)getQueueMeta.getVisibilityTimeout());
        assertEquals((Integer)1, (Integer)getQueueMeta.getLoggingEnabled());
    }

}

  

    其次，UI线程调用：

package com.example.a123.testmnsbyalia;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.alibaba.sdk.android.common.ClientConfiguration;
import com.alibaba.sdk.android.common.ClientException;
import com.alibaba.sdk.android.common.ServiceException;
import com.alibaba.sdk.android.common.auth.CredentialProvider;
import com.alibaba.sdk.android.common.auth.PlainTextAKSKCredentialProvider;
import com.alibaba.sdk.android.mns.MNS;
import com.alibaba.sdk.android.mns.MNSClient;
import com.alibaba.sdk.android.mns.callback.MNSCompletedCallback;
import com.alibaba.sdk.android.mns.internal.MNSAsyncTask;
import com.alibaba.sdk.android.mns.model.request.CreateQueueRequest;
import com.alibaba.sdk.android.mns.model.request.DeleteQueueRequest;
import com.alibaba.sdk.android.mns.model.result.CreateQueueResult;

import static junit.framework.Assert.assertEquals;
import static junit.framework.Assert.assertNull;

public class MainActivity extends AppCompatActivity {

    private String TAG = "MainActivity";
    QueueAttrTest queueAttrTest = new QueueAttrTest();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initMns();//初始化Mns

        getQueueMessage();//获取消息

    }

    private void getQueueMessage() {
        try {
            queueAttrTest.testSetAndGetQueueAttr();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void initMns() {
        try {
            queueAttrTest.setUp();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }



}