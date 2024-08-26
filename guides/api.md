# API

## APIUtil

APIUtil allow to call OpenAPI format API calls. 

### API Calls

This documentation will briefly explain the most frequent use methods to call API from non smarkXChange and smartXChange API methods.

```Java
/* Parameter Description for apiCall() method
method - get,post,put,patch,delete in lower-case
url - API URL to call
header - contents of header for the API site you are calling. Sometime you may need to specify content-type of application/json, application/xml, appId, authorization, and even authorization-app
body - the parameters you may post in form-data,x-www-form-urlencode,raw,binary. Body can be HashMap<String,String>, HttpEntity, and even String (Posting a raw json string). 
Return of this method will be String
*/
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
