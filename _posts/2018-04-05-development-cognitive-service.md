---
layout: post
title: "[부연설명]DingDong의 Cognitive Service 사용"
categories:
  - Development
tags:
  - DingDong
  - cognitive service
  - Face API
---

<br>

### Cognitive Service 사용 방법에 대해 알아보겠습니다.

<br>

저는 Microsoft Cognitive Service 중 Face API를 사용하였습니다.<br>
사람의 얼굴을 분석해서 나이, 성별, 피부색 등을 분석해주는 서비스입니다.

<br>

[Face API](https://docs.microsoft.com/ko-kr/azure/cognitive-services/face/quickstarts/java/ "Face API")를 보시면 Java로
사용하는 방법에 대해 자세히 나와있습니다.

<br>

#### 공식 홈페이지에서 제공하는 Java 코드 입니다.

```java
// This sample uses the Apache HTTP client library(org.apache.httpcomponents:httpclient:4.2.4)
// and the org.json library (org.json:json:20170516).

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;

public class Main
{
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "<Subscription Key>";

    // Replace or verify the region.
    //
    // You must use the same region in your REST API call as you used to obtain your subscription keys.
    // For example, if you obtained your subscription keys from the westus region, replace
    // "westcentralus" in the URI below with "westus".
    //
    // NOTE: Free trial subscription keys are generated in the westcentralus region, so if you are using
    // a free trial subscription key, you should not need to change this region.
    public static final String uriBase = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";


    public static void main(String[] args)
    {
        HttpClient httpclient = new DefaultHttpClient();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

`builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile...");`<br>
부분을 보시면 불필요하게 많은 정보를 요청하게 됩니다.<br>
그래서 저는 `builder.setParameter("returnFaceAttributes", "age,gender");` 로 수정하여 사용하였습니다.

또한 위의 코드는 웹상 올려져 있는 사진을 보내는 코드였고<br>
제가 사용해야 할 사진은 서버 컴퓨터에 저장되어 있었기 때문에

```java
// Request body.
StringEntity reqEntity = new StringEntity("{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}");
request.setEntity(reqEntity);
```

이 부분을 ByteArrayEntity 클래스를 이용하여 서버에 있는 사진을 보내도록 하였습니다.

```java
ByteArrayEntity bae = null;
try {
	BufferedImage img = ImageIO.read(new File(filePath));             
	ByteArrayOutputStream baos = new ByteArrayOutputStream();
	ImageIO.write(img, "jpg", baos);
	baos.flush();
	baos.close();
	
	bae = new ByteArrayEntity(baos.toByteArray());
} catch(Exception e){
	e.printStackTrace();
}

request.setEntity(bae);
```
#### 수정된 코드입니다.

```java
package com.dd.util;

import java.awt.image.BufferedImage;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.net.URI;
import javax.imageio.ImageIO;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.entity.ByteArrayEntity;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONObject;

import com.dd.domain.Visitor;

//This sample uses the Apache HTTP client library(org.apache.httpcomponents:httpclient:4.2.4)
//and the org.json library (org.json:json:20170516).


public class CognitiveServiceUtil {
	// **********************************************
	// *** Update or verify the following values. ***
	// **********************************************
	
	// Replace the subscriptionKey string value with your valid subscription key.
	public static final String subscriptionKey = "발급 받은 키값";
	
	// Replace or verify the region.
	//
	// You must use the same region in your REST API call as you used to obtain your subscription keys.
	// For example, if you obtained your subscription keys from the westus region, replace
	// "westcentralus" in the URI below with "westus".
	//
	// NOTE: Free trial subscription keys are generated in the westcentralus region, so if you are using
	// a free trial subscription key, you should not need to change this region.
	public static final String uriBase = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

	public Visitor cognitiveService(String filePath, Visitor visitor){
		
	    HttpClient httpclient = new DefaultHttpClient();
	    
	    try {
	        URIBuilder builder = new URIBuilder(uriBase);
	
	        builder.setParameter("returnFaceId", "false");
	        builder.setParameter("returnFaceLandmarks", "false");
	        builder.setParameter("returnFaceAttributes", "age,gender");
	
	        URI uri = builder.build();
	        HttpPost request = new HttpPost(uri);
	
	        request.setHeader("Content-Type", "application/octet-stream");
	        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
	
	        ByteArrayEntity bae = null;
	        try {
		        BufferedImage img = ImageIO.read(new File(filePath));             
		        ByteArrayOutputStream baos = new ByteArrayOutputStream();
		        ImageIO.write(img, "jpg", baos);
		        baos.flush();
		        baos.close();
		        
		        bae = new ByteArrayEntity(baos.toByteArray());
	        } catch(Exception e){
	        	e.printStackTrace();
	        }
	        
	        request.setEntity(bae);
	
	        HttpResponse response = httpclient.execute(request);
	        HttpEntity entity = response.getEntity();
	
	        if (entity != null){
	            String jsonString = EntityUtils.toString(entity).trim();
	            JSONObject jsonObject = new JSONObject(jsonString.replace("[", "").replace("]", ""));
	            String gender = (String)new JSONObject(jsonObject.get("faceAttributes").toString()).get("gender");
	            Double age = (Double)new JSONObject(jsonObject.get("faceAttributes").toString()).get("age");
	            Integer iAge = (int)(long)Math.round(age);
	            visitor.setGeneration(iAge / 10 * 10);
	            visitor.setAge(iAge);
	            visitor.setGender(gender);
	        }
	    }
	    catch (Exception e){
	        System.out.println(e.getMessage());
	    }
	    return visitor;
	}
}
```

