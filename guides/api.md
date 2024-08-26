# API

APIUtil allow to call OpenAPI format API calls. 

## API Calls

This documentation will briefly explain the most frequent use methods to call API from non smarkXChange and smartXChange API methods.

- **apiCall(String method,String url,HashMap<String,String> header,HttpEntity body)**  
    **Description:** API call methods using HashMap. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** The parameters you may post in form-data,x-www-form-urlencode,raw,binary.

- **apiCall(String method,String url,HashMap<String,String> header,HashMap<String,String> body)**  
    **Description:** API call methods using HashMap. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** Basically even if the parameter is in HashMap<String,String>, it will be converted to HttpEntity in the end. It just that HashMap<String,String> will be easier to use for parameters.

```java
HttpServletRequest request = (HttpServletRequest) ActionContext.getContext().get(ServletActionContext.HTTP_REQUEST);
try{
    ApiUtil apiUtil = new ApiUtil();
    HashMap<String,String> header = new HashMap<String,String>();
    HashMap<String,String> body = new HashMap<String,String>();
    String userId = request.getParameter("userId");
    header.put("accept","application/json");
    header.put("content-type","application/x-www-form-urlencoded");
    body.put("userId", userId);
    String results = apiCall( "post", String "https://zeus.sarawak.gov.my/aquilaframework-falcon-v2/ApiListener_userInfo", header, body )
} catch (Exception ex){
    ex.printStackTrace();
}
```

- **apiCall( String method, String url, HashMap<String,String> header, String body )**  
    **Description:** API call methods using String, frequently use to call API with raw JSON String body. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** Basically even if the parameter is in String, it will be converted to HttpEntity in the end. It just that keep it in string for JSON String parser. 

- **apiCall( String method, String url, HashMap<String,String> header, HashMap<String,String> body, HashMap<String,Object> binaryBody )**  
    **Description:** API call methods using HashMap, but with attachment file to be pass. Normally it use to send attachment to save by API calls. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** Basically even if the parameter is in String, it will be converted to HttpEntity in the end. It just that keep it in string for JSON String parser. 
    **binaryBody:** This is HashMap<String,Object>. String will be the object name, while Object is BinaryPart class you can found it in ApiUtil as well. 
    