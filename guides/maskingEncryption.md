# Masking Encryption Guide

## SessionFactoryImpl.java

### Initialization Method

Under the `init()` method, look for the code with `String a, b, c = "";`. Uncomment or create the code as follows:

Use this if you are using system.properties

```java
String a, b, c = "";
a = "1iux1tvn1vfx1sar1xfn1xff1saj1vgx1tvf1irp";
b = "1hv81wu81wu61vno1w1c1san1w261vne1wu81wu61hsa";
c = "1v2j1uum1xtv1zej1zer1xtn1uvk1v1v";
a = str(a);b = str(b);c = str(c);
if (Validator.isEmpty(SysConf.get("hbUrl"))) {
    config.setProperty(a+b+c, str("your encrypted password"));
} else {
    if (SysConf.get("hbPass").startsWith("Enc_")) {
        config.setProperty(a+b+c, str(SysConf.get("hbPass").substring(4)));
    } else {
        config.setProperty(a+b+c, SysConf.get("hbPass"));
    }
    config.setProperty("hibernate.connection.username", SysConf.get("hbUser"));
}
```

or follow this if you are using appConfig.properties

```java
String a, b, c = "";
a = "1iux1tvn1vfx1sar1xfn1xff1saj1vgx1tvf1irp";
b = "1hv81wu81wu61vno1w1c1san1w261vne1wu81wu61hsa";
c = "1v2j1uum1xtv1zej1zer1xtn1uvk1v1v";
a = str(a);
b = str(b);
c = str(c);
config.setProperty(a + b + c, str(AppProperties.getAppConf("hbPass")));
```

The masking password can be obtained from Anthony or encrypted using Codec.jar if you have it.

The important part for decryption is under str(), as you can check from the str() methods

### str(String encString)

```java
public static String str(String encString) {
    if (encString.startsWith("OBF:")) {
        encString = encString.substring(4);
    }

    byte[] b = new byte[encString.length() / 2];
    int l = 0;
    for (int i = 0; i < encString.length(); i += 4) {
        String x = encString.substring(i, i + 4);
        int i0 = Integer.parseInt(x, 36);
        int i1 = (i0 / 256);
        int i2 = (i0 % 256);
        b[l++] = (byte) ((i1 + i2 - 254) / 2);
    }

    return new String(b, 0, l);
}
```
