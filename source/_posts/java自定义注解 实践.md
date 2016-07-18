title: java自定义注解 实践
date: 2015-10-07 02:00:00 #发表日期，一般不改动
categories: coding #文章文类
tags: [java,注解] #文章标签，多于一项时用这种格式


---


## 可以使用自定义注解解决的场景需求，有哪些？
1. 程序在编写时，对类或方法就已经知道（或准备赋予）的行为或数据初始化，就可以考虑使用自定义注解进行标记。
2. 代码的通用性，封装代码的一种手段。同类行为定义一个自定义注解，调用时使用前置切面处理自定义注解的业务实现。


## 怎么使用自定义注解？
示例：存在注解方法@annotation a() （或 注解类@annotation class Test）
1. 【常规】调用方法a()时，执行自定义注解业务。需要使用前置切面处理自定义注解的业务实现。


示例：存在注解类 @annotation class Test，和常规类 class Test2
2. 【少见】在常规类Test2的方法中接收到Test（注解类）时，可以通过clazz.getAnnotation(Description.class); 获取注解，
    完成使用注解类时的业务处理。


## 我的实践：
常规实体类接口EntityService中，使用自定义注解@ForSimpleHander 完成接口服务，程序员只需要定义，不需要写一行代码
去完成业务。


<!-- more -->


注解类：
```java
import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;


@Target({java.lang.annotation.ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ForSimpleHander {

String entity() default "";

String[] params() default {};

String[] paramsStatic() default {};
}
```


切面处理类：
```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Arrays;
import java.util.Date;


import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.stereotype.Component;


import com.wosai.teach.annotation.ForSimpleHander;


@Component // 初始化Bean
@Aspect
public class RestFulAspect {

// 测试示例：http://localhost:8080/teach/service/aspect


/**
* 必须为final String类型的,注解里要使用的变量只能是静态常量类型的
*/
public static final String EDP0 = "execution(* com.wosai.teach.restful.wosaiTeach2.*(..))";
public static final String EDP1 = "execution(* com.wosai.teach.restful.EntityService.*(..))";
public static final String EDP2 = "execution(* com.wosai.teach.restful.EntityChangeService.*(..))";
public static final String EDP = EDP0 + " || " + EDP1 + " || " + EDP2;// 或  EDP1 + " or " + EDP2

@Before(EDP)// spring中Before通知
public void logBefore(JoinPoint joinPoint) {
// System.out.println("logBefore:现在时间是:"+new Date());

Object target = joinPoint.getTarget();
MethodSignature methodSignature = (MethodSignature)joinPoint.getSignature();

String clazzName = target.getClass().getName();
String methodName = methodSignature.getName();
Arrays.toString(joinPoint.getArgs());
Object[] args = joinPoint.getArgs();


HttpServletRequest request = null;
HttpServletResponse response = null;


for (Object arg : args) {
if (arg instanceof HttpServletRequest) request = (HttpServletRequest) arg;
if (arg instanceof HttpServletResponse) response = (HttpServletResponse) arg;
}


/** 日志 */
System.out.println(">>> " + clazzName + " " + methodName + "() request:"+ request.getRequestURL().toString());

/** 自定义注解处理 - anootationHandle */
ForSimpleHander forSimpleHander = methodSignature.getMethod().getAnnotation(ForSimpleHander.class);
if (forSimpleHander != null) {
Method m = getForSimpleHanderMethod(target.getClass());

if(m == null) return;

try {
String argEntity = forSimpleHander.entity();
String[] argParams = forSimpleHander.params();
String[] argParamsStatics = forSimpleHander.paramsStatic();

Object entity = Class.forName("com.wosai.teach.entity."+argEntity).newInstance();

Object[][] params = new Object[argParams.length + argParamsStatics.length][2];

// 动态参数
for (int i = 0; i < argParams.length; i++) {
String argParam = argParams[i];
params[i] = new Object[]{argParam,args[i]};
}

// 静态参数
for (int i = 0; i < argParamsStatics.length; i++) {
String[] argParamsStatic = argParamsStatics[i].split(",");
params[i+argParams.length] = new Object[] { argParamsStatic[0], argParamsStatic[1] };
}

m.invoke(target, request,response,entity,params);
} catch (IllegalAccessException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (IllegalArgumentException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (InstantiationException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (ClassNotFoundException e) {
// TODO Auto-generated catch block
e.printStackTrace();
} catch (InvocationTargetException e) {
// TODO Auto-generated catch block
e.printStackTrace();
}
}
}

public static Method getForSimpleHanderMethod(Class clazz){
try {
Method m1 = clazz.getDeclaredMethod("simpleHander",
HttpServletRequest.class, HttpServletResponse.class,Object.class,Object[][].class);
System.out.println("ok");
return m1;
} catch (NoSuchMethodException e) {
// e.printStackTrace();
return getForSimpleHanderMethod(clazz.getSuperclass());
} 
}

@After(EDP) // spring中After通知
public void logAfter(JoinPoint joinPoint) {
// System.out.println("logAfter:现在时间是:" + new Date());


String clazzName = joinPoint.getTarget().getClass().getName();
String methodName = joinPoint.getSignature().getName();
Arrays.toString(joinPoint.getArgs());
Object[] args = joinPoint.getArgs();

HttpServletRequest request = null;
HttpServletResponse response = null;

for (Object arg : args) {
if (arg instanceof HttpServletRequest) request = (HttpServletRequest) arg;
if (arg instanceof HttpServletResponse) response = (HttpServletResponse) arg;
}

System.out.println("<<< " + clazzName + " " + methodName + "() response:" + request.getAttribute("result"));
}


// @Around(EDP) //spring中Around通知
public Object logAround(ProceedingJoinPoint joinPoint) {
System.out.println("logAround开始:现在时间是:" + new Date()); // 方法执行前的代理处理
Object[] args = joinPoint.getArgs();
Object obj = null;
try {
obj = joinPoint.proceed(args);
} catch (Throwable e) {
e.printStackTrace();
}
System.out.println("logAround结束:现在时间是:" + new Date()); // 方法执行后的代理处理
return obj;
}


}
```


声明类：
```java
import java.io.IOException;
import java.lang.reflect.Method;
import java.util.List;
import java.util.Map;


import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;


import com.wosai.teach.annotation.ForSimpleHander;


@Controller
@RequestMapping("/service/entity")
public class EntityService extends RestFulBase{


/** 自定义注解 ****************************************************************************************/

@ForSimpleHander(entity="AreaProvince",params = {})
@RequestMapping(method = RequestMethod.GET , value="/areaProvinceFor")
public void areaProvince_for(HttpServletRequest request, HttpServletResponse response){}

@ForSimpleHander(entity="AreaCity",params = {"provinceId"})
@RequestMapping(method = RequestMethod.GET , value="/areaCityFor/{provinceId}")
public void areaCity_for(HttpServletRequest request, HttpServletResponse response,@PathVariable Integer provinceId){}

/** 非注解处理方式 - 无切面前 ****************************************************************************************/

/**
* 学校，专业，班级相关接口：获取地域，获取地域校园
* 学校，专业，班级相关接口：获取专业，获取班级
* 账户相关接口:获取用户账户信息，我的金币
* 账户相关接口:获取金币获取明细
**/
@RequestMapping(method = RequestMethod.GET , value="/aspect")
public void aspect(HttpServletRequest request, HttpServletResponse response) throws IOException {

// 获取数据
PageBean pageBean = initPageBean(request);
Map<String, Object> condition = initCondition(pageBean,prams("",""),prams("",""));
List<School> schoolList = baseListHander(new School(), emptyMap(condition));

// 整理响应
JsonResult result = initJsonResult(schoolList, pageBean.getRecordCount(), schoolList.size(), C.JSON_RESULT_SUCCESS, null);
responseWriter(request, response, result);

}


/** 非注解处理方式 - 切面后
* @throws IOException ****************************************************************************************/

@RequestMapping(method = RequestMethod.GET , value="/areaProvinceSimple")
public void areaProvince_simple(HttpServletRequest request, HttpServletResponse response) throws IOException { 

/** simpleHander */
simpleHander(request, response, new AreaProvince(),prams("",""));
}

@RequestMapping(method = RequestMethod.GET , value="/areaCitySimple/{provinceId}")
public void areaCity_simple(HttpServletRequest request, HttpServletResponse response,@PathVariable Integer provinceId) throws IOException { 

/** simpleHander */
simpleHander(request, response, new AreaProvince(),prams("provinceId",provinceId));
}

/******************************************************************************************/
```
