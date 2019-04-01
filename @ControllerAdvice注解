**目录**

1 [简介](#1)  
2 [作用](#2)    
3 [实例](#3)  




**1简介**<a name="1"></a>
  通过@ControllerAdvice注解可以将对于控制器的全局配置放在同一个位置。
  注解了@Controller的类的方法可以使用@ExceptionHandler、@InitBinder、@ModelAttribute注解到方法上。
  @ControllerAdvice注解将作用在所有注解了@RequestMapping的控制器的方法上
  @ExceptionHandler：用于全局处理控制器里的异常
 
**2作用**<a name="2"></a>
  @ControllerAdvice用于拦截Controller的接口，比如当接口抛出异常时，可以被拦截，然后返回指定的报文（如错误信息、错误码）
  
*3作用**<a name="3"></a>
//controller类
@Controller
@RequestMapping("/app")
public class DemoController {
    /**
     * @author: fjk
     * baseRequest上行
     * ResponseAdConfig 下行
     * 2019-03-26
     */
    @RequestMapping("/demo")
    @ResponseBody
    public ResultData<ResponseAdConfig> getAdConfig(@RequestBody BaseRequest baseRequest) throws AppException {
        //业务
        return new ResultData<>();;
    }

}
 
@ControllerAdvice
public class ControllerExceptionHandler {

    private static final Logger LOG = Logger.getLogger(ControllerExceptionHandler.class);

    /**
     * 控制器异常处理入口
     * AppException：自定义，只返回状态码，不返回提示语
     * ResultException：自定义，返回状态码和提示语
     * @param e 异常信息
     */
    @ExceptionHandler(value = Throwable.class)
    @ResponseBody
    @ResponseStatus(HttpStatus.OK)
    public ResultData<Object> resolveException(Exception e, HttpServletRequest request, WebRequest webRequest)  {
        ResultData<Object> resultData=new ResultData<>();
        if(e instanceof ResultException){
            LOG.warn(e.getMessage());
            ResultException re=(ResultException)e;
            resultData.setStatus(MessageCode.MESSAGE_FAIL);
            resultData.setCode(re.getCode());
            resultData.setMsg(re.getMessage());
            if (Objects.nonNull(re.getData())){
                resultData.setData(re.getData());
            }
            return resultData;
        }else if(e instanceof AppException){
            LOG.warn(e.getMessage());
            AppException re=(AppException)e;
            resultData.setStatus(MessageCode.MESSAGE_FAIL);
            resultData.setCode(MessageCode.APP_EXCEPTION_CODE);
            return resultData;
        }else{
            LOG.error(e.getMessage(),e);
            resultData.setStatus(MessageCode.MESSAGE_FAIL);
            resultData.setCode(MessageCode.FAIL[0]);
            resultData.setMsg((MessageCode.FAIL[1]));
        }
        return resultData;
    }
}

//统一提示语
public class MessageCode {

    /**
     * 注意：
     *       code以6开头：不需要给用户提示语
     *       code以4开头：需要把提示语给用户
     *       code以5开头：前端自行处理
     */

    public final static int MESSAGE_SUCCESS = 1;

    public final static int MESSAGE_FAIL = 0;

    public final static String [] FAIL = {"4000","服务器繁忙"};

    public final static String  APP_EXCEPTION_CODE = "600";
}

//异常类  ResultException和AppException一样
public class ResultException extends Exception {
    private static final long serialVersionUID = 1L;
    private String code;
    private Object data;

    public ResultException(String[] errorMessage) {
        this(errorMessage[0], errorMessage[1]);
    }

    public ResultException(String[] errorMessage, Object data) {
        this(errorMessage[0], errorMessage[1]);
        this.data = data;
    }
    
    public ResultException(String code, String msg) {
        super(msg);
        this.code = code;
    }
    
    public ResultException(String code, String msg,Object data) {
        super(msg);
        this.code = code;
        this.data = data;
    }

    public String getCode() {
        return this.code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}

//统一返回结果类
@Data
public class ResultData<T> {
    private String code = "600";
    private Integer status = 1;
    //控制接口请求的频率
    private Long interval;
    //返回的提示语
    private String msg;
    //下发的数据
    private T data;

    public ResultData(){

    }

    public void setInterval(Long interval) {
        this.interval = interval;
    }

    public ResultData(String msg){
        //对于code=4000的前端将提示语展示给用户看
        this.code = "4000";
        this.msg = msg;
    }

    public ResultData(T t){
        this.data = t;
    }

    public ResultData(String msg, T t){
        this.code = "4000";
        this.msg = msg;
        this.data = t;
    }

}
