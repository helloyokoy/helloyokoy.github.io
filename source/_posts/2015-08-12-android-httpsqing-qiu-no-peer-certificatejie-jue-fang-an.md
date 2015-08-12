---
layout: post
title: "android https请求no peer certificate解决方案"
date: 2015-08-12 16:30:44 +0800
comments: true
categories: android
---

###解决方案一
---

写了一个自定义类继承SSLSocketFactory：

	import java.io.IOException;
	import java.net.Socket;
	import java.net.UnknownHostException;
	import java.security.KeyManagementException;
	import java.security.KeyStore;
	import java.security.KeyStoreException;
	import java.security.NoSuchAlgorithmException;
	import java.security.UnrecoverableKeyException;

	import javax.net.ssl.SSLContext;
	import javax.net.ssl.TrustManager;
	import javax.net.ssl.X509TrustManager;

	import org.apache.http.conn.ssl.SSLSocketFactory;

	public class SSLSocketFactoryEx extends SSLSocketFactory {
        
        SSLContext sslContext = SSLContext.getInstance("TLS");
        
        public SSLSocketFactoryEx(KeyStore truststore) 
                        throws NoSuchAlgorithmException, KeyManagementException,
                        KeyStoreException, UnrecoverableKeyException {
                super(truststore);
                
                TrustManager tm = new X509TrustManager() {
                        public java.security.cert.X509Certificate[] getAcceptedIssuers() {return null;}  
    
            @Override  
            public void checkClientTrusted(
                            java.security.cert.X509Certificate[] chain, String authType)
                                            throws java.security.cert.CertificateException {}  
    
            @Override  
            public void checkServerTrusted(
                            java.security.cert.X509Certificate[] chain, String authType)
                                            throws java.security.cert.CertificateException {}
        };  
        sslContext.init(null, new TrustManager[] { tm }, null);  
    }  
    
    @Override  
    public Socket createSocket(Socket socket, String host, int port,boolean autoClose) throws IOException, UnknownHostException {  
            return sslContext.getSocketFactory().createSocket(socket, host, port,autoClose);  
    }  
    
    @Override  
    public Socket createSocket() throws IOException {  
        return sslContext.getSocketFactory().createSocket();  
    }  
	}

<!--more-->

再来看看如何做回调：

	public static HttpClient getNewHttpClient() {  
        try {  
            KeyStore trustStore = KeyStore.getInstance(KeyStore.getDefaultType());  
            trustStore.load(null, null);  
            
            SSLSocketFactory sf = new SSLSocketFactoryEx(trustStore);  
            sf.setHostnameVerifier(SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);  
    
            HttpParams params = new BasicHttpParams();  
            HttpProtocolParams.setVersion(params, HttpVersion.HTTP_1_1);  
            HttpProtocolParams.setContentCharset(params, HTTP.UTF_8);  
    
            SchemeRegistry registry = new SchemeRegistry();  
            registry.register(new Scheme("http", PlainSocketFactory.getSocketFactory(), 80));  
            registry.register(new Scheme("https", sf, 443));  
    
            ClientConnectionManager ccm = new ThreadSafeClientConnManager(params, registry);  
    
            return new DefaultHttpClient(ccm, params);  
        } catch (Exception e) {  
            return new DefaultHttpClient();  
        }  
    }  


现在就可以拿这个HTTPClient去请求数据了

###解决方案二
---

[http://www.cnblogs.com/P_Chou/archive/2010/12/27/https-ssl-certification.html](http://www.cnblogs.com/P_Chou/archive/2010/12/27/https-ssl-certification.html "Title")讲的非常清楚https-ssl的认证过程，膜拜下该作者

1.浏览器访问https地址，保存提示的证书到本地，放到android项目中的assets目录。

2.导入证书，代码如下。

3.把证书添加为信任。



	String requestHTTPSPage(String mUrl) {
        InputStream ins = null;
        String result = "";
        try {
            ins = context.getAssets().open("app_pay.cer"); //下载的证书放到项目中的assets目录中
            CertificateFactory cerFactory = CertificateFactory
                    .getInstance("X.509");
            Certificate cer = cerFactory.generateCertificate(ins);
            KeyStore keyStore = KeyStore.getInstance("PKCS12", "BC");
            keyStore.load(null, null);
            keyStore.setCertificateEntry("trust", cer);
 
            SSLSocketFactory socketFactory = new SSLSocketFactory(keyStore);
            Scheme sch = new Scheme("https", socketFactory, 443);
            HttpClient mHttpClient = new DefaultHttpClient();
            mHttpClient.getConnectionManager().getSchemeRegistry()
                    .register(sch);
 
            BufferedReader reader = null;
            try {
                Log.d(TAG, "executeGet is in,murl:" + mUrl);
                HttpGet request = new HttpGet();
                request.setURI(new URI(mUrl));
                HttpResponse response = mHttpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != 200) {
                    request.abort();
                    return result;
                }
 
                reader = new BufferedReader(new InputStreamReader(response
                        .getEntity().getContent()));
                StringBuffer buffer = new StringBuffer();
                String line = null;
                while ((line = reader.readLine()) != null) {
                    buffer.append(line);
                }
                result = buffer.toString();
                Log.d(TAG, "mUrl=" + mUrl + "\nresult = " + result);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                if (reader != null) {
                    reader.close();
                }
            }
        } catch (Exception e) {
            // TODO: handle exception
        } finally {
            try {
                if (ins != null)
                    ins.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return result;
    }