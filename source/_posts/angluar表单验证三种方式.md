title: angluar表单验证三种方式
date: 2016-2-25 00:00:00
categories: angular
tags: [angular,表单验证]
 
---
# 方式一: Html正则&校验
**html**
> 使用angluar插件`ng-messages`(非必须): https://github.com/angular/bower-angular-messages
```
<form name="oneLoginForm" novalidate ng-submit="login(oneLoginForm)">
 
  <div class="list">
 
    <label class="item item-input">
      <span class="input-label">手机号码</span>
      <input type="tel" name="mobile" placeholder="输入手机号码" ng-model="oneLogin.mobile"
             ng-pattern="/(^1[3|4|5|7|8][0-9]{9}$)/" required>
    </label>
 
    <div class="error-container" ng-show="oneLoginForm.mobile.$error && oneLoginForm.mobile.$dirty"
         ng-messages="oneLoginForm.mobile.$error">
      <div ng-messages-include="templates/common/error-list.html"></div>
    </div>
 
    <div class="item item-input-inset">
      <label class="item-input-wrapper" style="background-color:transparent;">
        <input type="tel" name="randomCode" placeholder="输入短信验证码" ng-model="oneLogin.randomCode" required></label>
      <count-Down ng-click="getCode()"></count-Down>
    </div>
 
    <div class="error-container last-error-container"
         ng-show="oneLoginForm.randomCode.$error && oneLoginForm.$submitted"
         ng-messages="oneLoginForm.randomCode.$error">
      <div ng-messages-include="templates/common/error-list.html"></div>
    </div>
 
  </div>
 
  <!-- ng-disabled="oneLoginForm.$invalid" 作用:表单验证通过,才开启按钮可用 -->
  <button class="button button-positive register-btn" type="submit" ng-disabled="oneLoginForm.$invalid">登录</button>
</form>
```
 
**templates/common/error-list.html**
```
<div class="error" ng-message="required"> <i class="ion-information-circled"></i>
    请填写该项信息喔
</div>
<div class="error" ng-message="minlength"> <i class="ion-information-circled"></i>
    Minimum length of this field is 5 characters!
</div>
<div class="error" ng-message="maxlength">
    <i class="ion-information-circled"></i>
    Maximum length of this field is 20 characters!
</div>
<div class="error" ng-message="pattern"> <i class="ion-information-circled"></i>
    手机号格式不正确
</div>
```
 
**javaScript**
```
function login(form) {
  if(form.$valid) {
    UserService.oneLogin($scope.oneLogin.mobile,$scope.oneLogin.randomCode)
      .then(function (response) {
        if (response.data.result == 0) {
          warningForm(null, '登录成功');
          UserService.setCurrentUser(response.data.object);
          localStorageService.set('token', response.data.object.token);
          $state.go('***');
        } else {
          warningForm(null, response.data.message);
        }
        $scope.clickedLogin = false;
      }, function (response) {
        $scope.clickedLogin = false;
      });
  }
};
```
 
# 方式二: html正则,javaScript校验
 
**html**
```
<form name="loginForm" novalidate>
  <label class="item item-input item-icon-left item-login">
   <!-- <i class="icon ion-person login-icon"></i> -->
   <img src="img/icon/selfCenter.png">
    <input type="text" name="loginName" placeholder="请输入账号" ng-model="loginData.loginName" class="login-input" required></label>
 
  <label class="item item-input item-icon-left item-login">
    <!-- <i class="icon ion-ios-locked-outline login-icon"></i> -->
    <img src="img/icon/password.png">
    <input type="password" autocomplete="off" placeholder="请输入登录密码" name="password" ng-model="loginData.password" class="login-input" required></label>
 
  <!-- <div class="forgetPass" ui-sref="forgetPass">忘记密码？</div> -->
</form>
```
 
**javaScript**
```
$scope.doLogin = function () {
 
  var loginName = $scope.loginData.loginName;
  var password = $scope.loginData.password;
 
  var loginNameClass = loginForm.loginName.classList;
  var passwordClass = loginForm.password.classList;
 
  if (loginNameClass.contains(ERROR.notRequired)) {
    warningForm(null, ERROR.mobile);
    return false;
  }
 
  if (passwordClass.contains(ERROR.notRequired)) {
    warningForm(null, '请输入密码');
    return false;
  }
 
  $scope.clickedLogin = true;
 
  UserService.login(loginName, password).then(function (response) {
    if (response.data.result == 0) {
      warningForm(null, '登录成功');
      ...
    } else {
      warningForm(null, response.data.message);
    }
    $scope.clickedLogin = false;
  }, function (response) {
    $scope.clickedLogin = false;
  });
};
```
 
# 方式三: javaScript正则&校验
**html**
```
<form name="registerForm" novalidate>
 
  <div class="item item-input-inset">
    <label class="item-input-wrapper" style="background-color:transparent;">
      <input type="tel" name="mobile" placeholder="请输入您的手机号" ng-model="registerData.mobile" required>
    </label>
  </div>
 
  <div class="item item-input-inset">
    <label class="item-input-wrapper" style="background-color:transparent;">
      <input type="tel" name="randomCode" placeholder="输入短信验证码" ng-model="registerData.randomCode" required>
    </label>
    <count-Down ng-click="getCode()"></count-Down>
  </div>
 
  <div class="item item-input-inset">
    <label class="item-input-wrapper" style="background-color:transparent;">
      <input type="password" autocomplete="off" placeholder="6~12位的登录密码" name="password" ng-model="registerData.password" ng-minlength="6" ng-maxlength="12" required>
    </label>
  </div>
 
  <p ng-show="registerForm.password.$error.minlength || registerForm.password.$error.maxlength"
  class="padding assertive">密码为6~12位的数字或字母</p>
 
  <div class="item item-input-inset">
    <label class="item-input-wrapper" style="background-color:transparent;">
      <input type="password" placeholder="确认密码" name="repassword" ng-model="registerData.repassword" required>
    </label>
  </div>
 
  <p ng-show="registerForm.password.$dirty && registerForm.repassword.$dirty && registerData.password!==registerData.repassword"
       class="padding assertive">两次输入的密码不相同</p>
</form>
```
 
**javaScript**
```
$scope.register = function () {
  var mobile = $scope.registerData.mobile;
  var password = $scope.registerData.password;
  var randomCode = registerForm.randomCode.value;
 
  if (!RegUtil.mobileReg(mobile)) {
    return false;
  }
 
  if (!RegUtil.password(password)) {
    return false;
  }
 
  UserService.register(mobile, randomCode, password)
    .then(function (res) {
      if (res.data.result == 0) {
        UserService.setCurrentUser(res.data.object);
        localStorageService.set('token', res.data.object.token);
        warningForm(null, '注册成功');
        $state.go("registerDetail");
      } else {
        warningForm(null, res.data.message);
      }
    });
}
```
 
**RegUtil.mobileReg(mobile)**
```
return {
  mobileReg:function(mobile){
    var mobileReg = new RegExp(/(^1[3|4|5|7|8][0-9]{9}$)/);
 
    if(!mobileReg.test(mobile)){
      warningForm(null,'手机号格式不正确喔');
      return false;
    }else{
      return true;
    }
  },
  contact: function(contact){
    var contactReg = new RegExp('^[0-9]*$');
 
    if(!contactReg.test(contact)){
      warningForm(null,'电话号码格式不正确喔');
      return false;
    }else{
      return true;
    }
  },
  ...
```
 
<!-- more -->