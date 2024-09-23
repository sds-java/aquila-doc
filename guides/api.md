# API

APIUtil allow to call OpenAPI format API calls. 

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

API Calls from back-end using JSON String
```java
public void getUserInfo() throws Exception{
    HttpServletRequest request = (HttpServletRequest) ActionContext.getContext().get(ServletActionContext.HTTP_REQUEST);
	try{
        String jsonData = request.getReader().lines().reduce("", (accumulator, actual) -> accumulator + actual);
        JSONParser parser = new JSONParser();
        JSONObject jsonRequest = (JSONObject) parser.parse(jsonData);
        String userId = "";
        if(jsonRequest.get("userId") != null){
            userId = jsonRequest.get("userId").toString();
        } else {
            throw new Exception("userId cannot be Empty");
        }

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
} // getter and setter methods for attachmentFileContentType,attachmentFile,attachmentFileFileName, and userId

```

## Setup API Listener
API Listener will require access token in header for validation. Will have to extends from BaseActionSupport_API. Before proceed with the coding for API Listener, please setup the API Authentication. Please refer to API Authentication segment for more information about how to setup the API Authentication to generate access token.

```java

public class ApiListenerAction extends BaseActionSupport_API{

	@Override
	public String apiApp() {
		return "API_CALL";
	}

	@Override
	public Boolean validateRights() {
		return Boolean.TRUE;
	}
	
	public ApiListenerAction(){
		model = new String();
        setInsertUpdateSuccessCode(SystemConstants.ACTION_Status.LOAD_EDIT_PAGE);
	}
	
	public void userInfo(){
		HttpServletRequest request = (HttpServletRequest) ActionContext.getContext().get(ServletActionContext.HTTP_REQUEST);
		HttpServletResponse response = (HttpServletResponse) ActionContext.getContext().get(ServletActionContext.HTTP_RESPONSE);
		BaseDAOImpl dao = new BaseDAOImpl();
		JSONObject jsonResult = new JSONObject();
		String status = "";
		try{
			String userId = request.getParameter("userId");
			if(userId!=null){
				User user = (User) dao.getSession()
					.getNamedQuery("User.findByUsUserId")
					.setParameter("us_user_id", userId)
					.uniqueResult();
				if(user!=null){
					status = "success";
					jsonResult.put("id", user.getUs_id());
					jsonResult.put("userId", user.getUs_user_id());
					jsonResult.put("name", user.getUs_user_name());
					jsonResult.put("email", user.getUs_email());
					jsonResult.put("status", user.getUs_status());
				} else {
					status = "failed";
					jsonResult.put("message", "userId '" 
						+ userId + "' not found!.");
				}
			} else {
				status = "failed";
				jsonResult.put("message", "Invalid parameter userId '" 
						+ userId + "'");
			}
			JSONObject data = new JSONObject();
			data.put("status", status);
			data.put("data", jsonResult);
			response.setContentType("application/json");
            response.getWriter().append(data.toJSONString());
            response.flushBuffer();
		} catch(Exception ex){
			status = "failed";
			jsonResult.put("message", "Request failed.");
			try{
				JSONObject data = new JSONObject();
				data.put("data", jsonResult);
				data.put("status", status);
				response.setContentType("application/json");
				response.getWriter().append(data.toJSONString());
				response.flushBuffer();
			} catch(Exception ex2) {
				
			}
		} finally {
			dao.closeSession();
			dao = null;
		}
	}
}

```


## API Authentication
Application Name: API Authentication
Module: System Administration
Table: t_api_authentication

Steps to generate secret key and customer key
Step 1: Create new API Authentication
Step 2: Input the required fields. Refer to API Authentication Screen details below.
Step 3: Save and will receive the Customer Key and Secret Key required for generate access token

API Authentication Screen details.
API Application - Application Code.
API Description - Description of what is your API do.
User - User ID of your system. Must be exists in system. 
Token Duration - Duration of the generate token count in seconds.
Customer Key - Will be auto-generated.
Secret Key - Will be auto-generated.
