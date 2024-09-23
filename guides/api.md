# API

APIUtil allow to call OpenAPI format API calls. 

## API Calls from back-end
## API Calls from back-end

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
//This sample is POST by x-www-form-urlencoded form. 
private String userId = "";
public void getUserInfo(){
    try{
        ApiUtil apiUtil = new ApiUtil();
        HashMap<String,String> header = new HashMap<String,String>();
        HashMap<String,String> body = new HashMap<String,String>();
        header.put("accept","application/json");
        header.put("content-type","application/x-www-form-urlencoded");
        body.put("userId", userId);
        String results = apiCall( "post", "https://zeus.sarawak.gov.my/aquilaframework-falcon-v2/ApiListener_userInfo", header, body );
    } catch (Exception ex){
        ex.printStackTrace();
    }
}

public String getUserId(){
    return userId;
}

public void setUserId(String userId){
    this.userId = userId;
}
```

- **apiCall( String method, String url, HashMap<String,String> header, String body )**  
    **Description:** API call methods using String, frequently use to call API with raw JSON String body. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** Basically even if the parameter is in String, it will be converted to HttpEntity in the end. It just that keep it in string for JSON String parser. 

```java
//This sample is POST by x-www-form-urlencoded form. 
private String userId = "";
public void getUserInfo(){
    try{
        ApiUtil apiUtil = new ApiUtil();
        HashMap<String,String> header = new HashMap<String,String>();
        JSONObject json = new JSONObject();
        json.put("userId", userId);
        json.put("grant_type","credentials");
        json.put("status","Y");
        header.put("accept","application/json");
        header.put("content-type","application/x-www-form-urlencoded");

        String results = apiCall( "post", "https://zeus.sarawak.gov.my/aquilaframework-falcon-v2/ApiListener_userInfo", header, json.toJSONString() );
    } catch (Exception ex){
        ex.printStackTrace();
    }
}

public String getUserId(){
    return userId;
}

public void setUserId(String userId){
    this.userId = userId;
}

```

- **apiCall( String method, String url, HashMap<String,String> header, HashMap<String,String> body, HashMap<String,Object> binaryBody )**  
    **Description:** API call methods using HashMap, but with attachment file to be pass. Normally it use to send attachment to save by API calls. The return value of this method is string value.
    **method:** get,post,put,patch,delete in lower-case
    **url:** API URL to call
    **header:** contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
    **body:** Basically even if the parameter is in String, it will be converted to HttpEntity in the end. It just that keep it in string for JSON String parser. 
    **binaryBody:** This is HashMap<String,Object>. String will be the object name, while Object is BinaryPart class you can found it in ApiUtil as well. 
    
```java
//This sample is POST by x-www-form-urlencoded form. 
private String userId = "";
private File attachmentFile = null;
private String attachmentFileContentType;
private String attachmentFileFileName;
public void getUserInfo() throws Exception{
    try{
        ApiUtil apiUtil = new ApiUtil();
        HashMap<String,String> header = new HashMap<String,String>();
        HashMap<String,String> body = new HashMap<String,String>();
		HashMap<String,Object> binaryBody = new HashMap<String,Object>();
		if(attachmentFileFileName==null){
			new Exception("[attachmentFileFileName] cannot be empty");
		}
		if(attachmentFile==null){
			new Exception("[attachmentFile] cannot be empty");
		}
		if(attachmentFileContentType==null){
			new Exception("[attachmentFileContentType] cannot be empty");
		}
		ApiUtil.BinaryPart binaryPart;
		binaryPart = apiUtil.new BinaryPart(attachmentFileFileName,attachmentFile,attachmentFileContentType);
		binaryBody.put("file", binaryPart);
        header.put("accept","application/json");
        header.put("content-type","application/x-www-form-urlencoded");
        body.put("userId", userId);
        String results = apiCall( "post", "https://zeus.sarawak.gov.my/aquilaframework-falcon-v2/ApiListener_userInfo", header, body, binaryBody );
    } catch (Exception ex){
        ex.printStackTrace();
    }
}

public String getUserId(){
    return userId;
}

public void setUserId(String userId){
    this.userId = userId;
}

public File getAttachmentFile() {
    return attachmentFile;
}

public void setAttachmentFile(File attachmentFile) {
    this.attachmentFile = attachmentFile;
}

public String getAttachmentFileContentType() {
    return attachmentFileContentType;
}

public void setAttachmentFileContentType(String attachmentFileContentType) {
    this.attachmentFileContentType = attachmentFileContentType;
}

public String getAttachmentFileFileName() {
    return attachmentFileFileName;
}

public void setAttachmentFileFileName(String attachmentFileFileName) {
    this.attachmentFileFileName = attachmentFileFileName;
}
```

## API Calls from back-end using JSON String

```java
public void getUserInfo() throws Exception{
    HttpServletRequest request = (HttpServletRequest) ActionContext.getContext().get(ServletActionContext.HTTP_REQUEST);
	try{
        String jsonData = request.getReader().lines().reduce("", (accumulator, actual) -> accumulator + actual);
        JSONParser parser = new JSONParser();
        JSONObject jsonRequest = (JSONObject) parser.parse(jsonData);

        ApiUtil apiUtil = new ApiUtil();
        HashMap<String,String> header = new HashMap<String,String>();
        JSONObject json = new JSONObject();
        String userId = "";
        if(jsonRequest.get("userId")!=null){
           userId =  jsonRequest.get("userId").toString();
        } else {
            throw new Exception("userId cannot be empty!.");
        }
        json.put("userId", userId);
        json.put("grant_type","credentials");
        json.put("status","Y");
        header.put("accept","application/json");
        header.put("content-type","application/x-www-form-urlencoded");

        String results = apiCall( "post", "https://zeus.sarawak.gov.my/aquilaframework-falcon-v2/ApiListener_userInfo", header, json.toJSONString() );
    } catch (Exception ex){
        ex.printStackTrace();
    }
}
```
