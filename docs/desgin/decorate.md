
装饰器模式：

装饰器的核心就是新接口继承原有的接口，对于原有的实现类，重新注入到新的实现类想

如：public interface ISigninForThirdService  extends ISigninService


装饰器模式是一种特殊的适配器模式：

比较：

装饰器模式| 适配器模式
---|---
是一种非常特别的适配器模式 | 可以不保留层级关系
装饰者和被装饰者都要实现同一个接口，主要目的是为了扩展，依旧保留OOP关系 | 适配者和被适配者没有必然的层级联系，通常采用代理或者继承形式进行包装
满足is-a的关系 |满足has-a
  注重的是覆盖、扩展 |  注重兼容、转换

```
public interface ISigninService {
     ResultMsg regist(String username, String password);

    /**
     * 登录的方法
     * @param username
     * @param password
     * @return
     */
     ResultMsg login(String username, String password);
}
public class Member {

    private String username;
    private String password;
    private String mid;
    private String info;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getMid() {
        return mid;
    }

    public void setMid(String mid) {
        this.mid = mid;
    }

    public String getInfo() {
        return info;
    }

    public void setInfo(String info) {
        this.info = info;
    }
}

public class ResultMsg {

    private int code;
    private String msg;
    private Object data;

    public ResultMsg(int code, String msg, Object data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}

public class SigninService implements ISigninService {

    public ResultMsg regist(String username,String password){
        return  new ResultMsg(200,"注册成功",new Member());
    }


    /**
     * 登录的方法
     * @param username
     * @param password
     * @return
     */
    public ResultMsg login(String username,String password){
        return null;
    }
}
public interface ISigninForThirdService  extends ISigninService {

    public ResultMsg loginForQQ(String openId);

    public ResultMsg loginForWechat(String openId);

    public ResultMsg loginForToken(String token);

    public ResultMsg loginForTelphone(String telphone,String code);

    public ResultMsg loginForRegist(String username,String password);
}

public class SigninForThirdService implements ISigninForThirdService {

    private ISigninService service;
    public SigninForThirdService(ISigninService service){
        this.service = service;
    }


    @Override
    public ResultMsg regist(String username, String password) {
        return service.regist(username,password);
    }

    @Override
    public ResultMsg login(String username, String password) {
        return service.login(username,password);
    }


    public ResultMsg loginForQQ(String openId){
        return loginForRegist(openId,null);
    }

    public ResultMsg loginForWechat(String openId){
        return null;
    }

    public ResultMsg loginForToken(String token){
        //通过token拿到用户信息，然后再重新登陆了一次
        return  null;
    }

    public ResultMsg loginForTelphone(String telphone,String code){

        return null;
    }

    public ResultMsg loginForRegist(String username,String password){
        this.regist(username,null);
        return this.login(username,null);
    }
}
```