# AndServer-CN
`AndServer`��һ��`Android`��`Web`������, ֧�ֲ���̬��վ�;�̬��վ, ֧��д`Http`�ӿڣ���`Java`��`Servlet`һ����

QQ��������Ⱥ��46523908����Ⱥ���Ķ�[Ⱥ��Ϊ�淶](https://github.com/yanzhenjie/SkillGroupRule)��

----

# �ص�
1. ����̬��վ��
2. ����̬��վ��
3. ��̬Http API����������ͨ��˵�ķ������ӿڡ�
4. ���ܿͻ����ļ��ϴ���
5. ���ܿͻ��������ļ���

# ����
* Gradle
```groovy
compile 'com.yanzhenjie:andserver:1.0.2'
```

* Maven
```xml
<dependency>
  <groupId>com.yanzhenjie</groupId>
  <artifactId>andserver</artifactId>
  <version>1.0.2</version>
  <type>pom</type>
</dependency>
```

* Eclipse
[Download Jar File](./Jar/andserver.jar?raw=true)

# ʹ�÷���
��õĽ̳���`sample`�������ز鿴��Ȼ����**README**�͸������ˡ�

## ����������
```java
AndServer andServer = new AndServer.Build()
    ...
    .build();

// ������������
Server mServer = andServer.createServer();
...

// ������������
mServer.start();
...

// ֹͣ��������
mServer.stop();
...

// ����������������
boolean running = mServer.isRunning();
```

## �˿ںź���Ӧ��ʱ����
```java
AndServer andServer = new AndServer.Build()
    .port(8080) // Ĭ����8080��Androidƽ̨����Ķ˿ںŶ����ԡ�
    .timeout(10 * 1000) // Ĭ��10 * 1000���롣
    ...
    .build();
...
```

## ������վ
������վ��ͨ��`Website`�ӿڣ���Ҳ�����Լ�ʵ������ӿڣ���Ȼ`AndServer`�Ѿ��ṩ������Ĭ��ʵ�֣�  

* [AssetsWebsite](./andserver/src/main/java/com/yanzhenjie/andserver/website/AssetsWebsite.java)
* [StorageWebsite](./andserver/src/main/java/com/yanzhenjie/andserver/website/StorageWebsite.java)

�������������ʵ��ע�������վ����ô���Ĭ����ҳ��`index.html`���ǣ�  
`http://ip:port/`  
`http://ip:port/youPath`  
`http://ip:port/youPath/index.html`  

### ע����վ��AndServer
```java
Wesite wesite = new AssetsWebsite(AssetManager, youPath);
// ����
Wesite wesite = new StorageWebsite(youPath);

AndServer andServer = new AndServer.Build()
    ...
    .website(wesite);
    .build();
```

### AssetsWebsite��ʹ��
��������վ��`assets`�£���ô�����`AssetsWebsite`�����������վ��  

ʹ�÷����ǣ�  
```java
AssetManager mAssetManager = getAssets(); //AssetManager can not be closed.

Wesite wesite = new AssetsWebsite(mAssetManager, youPath);
```

* ��������վ��`assets`��Ŀ¼��, ���`path`����`""`�����磺  

![web_assets.png](./image/web_assets.png)
```java
Wesite wesite = new AssetsWebsite(mAssetManager, "");
```

��ô���Ĭ����ҳ���ʵ�ַ���ǣ�  
`http://ip:port`  
`http://ip:port/index.html`  

��ô�������ҳ����ʵ�ַ�ǣ�  
`http://ip:port/login.html`  
`http://ip:port/error.html`  

���磺  
```
http://192.168.1.12:8080/index.html  
http://192.168.1.12:8080/login.html
```

* ��������վ��Ŀ¼��`assets`����Ŀ¼�£���ô�㴫��`assets`�����Ŀ¼��ַ�ͺ��˱��������վ��`assets`��`web`Ŀ¼�����磺  

![web_assets.png](./image/web_assets_son.png)
```java
Wesite wesite = new AssetsWebsite(mAssetManager, "web");
```

��ô���Ĭ����ҳ���ʵ�ַ���ǣ�  
`http://ip:port`  
`http://ip:port/web`  
`http://ip:port/web/index.html`  

��ô�������ҳ����ʵ�ַ�ǣ�  
`http://ip:port/web/login.html`  
`http://ip:port/web/error.html`  

���磺  
```
http://192.168.1.12:8080/
http://192.168.1.12:8080/index.html
http://192.168.1.12:8080/web/index.html
http://192.168.1.12:8080/web/index.html  
http://192.168.1.12:8080/web/login.html
```

### StorageWebsite��ʹ��
��������վ��`assets`�£���ô�����`StorageWebsite`�����������վ�����������վ��SD����ʱ��

ʹ�÷����ǣ�  
```java
Wesite wesite = new StorageWebsite(youPath);
```

���ܼ򵥣�ֻҪ���������վ�Ĵ洢Ŀ¼��ַ���ɣ����������վ��SD���µ�`www`Ŀ¼��  
```java
File file = new File(Environment.getExternalStorageDirectory(), "www");
String websiteDirectory = file.getAbsolutePath();

Wesite wesite = new StorageWebsite(websiteDirectory);
```

���ʵ�ַ��`AssetsWebsite`�ĵ�����ͬ��

## Http API
Http API��ͨ��`RequestHandler`�ӿ���ע��ģ�����һ��`java interface`������`Java`��`Servlet`һ����  

����Ҫʵ������ӿڣ�Ȼ����`AndServer`ע�ἴ�ɣ����磺  
```java
public class RequestLoginHandler implements RequestHandler {

    @Override
    public void handle(HttpRequest req, HttpResponse res, HttpContext con) {
        Map<String, String> params = HttpRequestParser.parse(request);

        // Request params.        
        String userName = params.get("username");
        String password = params.get("password");

        if ("123".equals(userName) && "123".equals(password)) {
            StringEntity stringEntity = new StringEntity("Login Succeed", "utf-8");
            response.setEntity(stringEntity);
        } else {
            StringEntity stringEntity = new StringEntity("Login Failed", "utf-8");
            response.setEntity(stringEntity);
        }
    }
}
```

Ȼ����`AndServer`��ע�᣺  
```java
AndServer andServer = new AndServer.Build()
    ...
    .registerHandler("login", new RequestLoginHandler())
    .build();
```

������͵õ���һ��Ψһ�ķ��ʵ�ַ��`http://ip:port/login`, ���磺  
```
http://192.168.1.12:8080/login?username=123&password=123
```

�ļ����غ��ļ��ϴ�������������`sample`�鿴��

## Html���ύ
��`Html`��`form`��`action`��������ע��`RequestHandler`ʱ��`key`�ͣ�Ȼ����`RequestHandler`��`handle(HttpRequest, HttpResponse, HttpContext)`�����Ϳ��Ի�ȡ`form`�ύ�Ĳ����ˡ�  

������������ע��`Login RequestHandler`��`form`������ʹ��:  
```html
<form id="form1" method="post" action="login">
...
</form>
```

## ������������״̬
```java
private Server.Listener mListener = new Server.Listener() {
    @Override
    public void onStarted() {
        // �����������ɹ�.
    }

    @Override
    public void onStopped() {
        // ������ֹͣ�ˣ�һ���ǿ����ߵ���server.stop()�Ż�ֹͣ��
    }

    @Override
    public void onError(Exception e) {
        // ������������������һ���Ƕ˿ڱ�ռ�á�
    }
};

AndServer andServer = new AndServer.Build()
    ...
    .listener(mListener)
    .build();
```

#License
```text
Copyright 2017 Yan Zhenjie

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```