// Ionic ionic-todo App

// angular.module is a global place for creating, registering and retrieving Angular modules
// 'ionic-todo' is the name of this angular module example (also set in a <body> attribute in index.html)
// the 2nd parameter is an array of 'requires'
var app = angular.module('ionic-todo', ['ionic', 'LocalStorageModule']);

app.run(function ($ionicPlatform) {
  $ionicPlatform.ready(function () {
    if (window.cordova && window.cordova.plugins.Keyboard) {
      // Hide the accessory bar by default (remove this to show the accessory bar above the keyboard
      // for form inputs)
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);

      // Don't remove this line unless you know what you are doing. It stops the viewport
      // from snapping when text inputs are focused. Ionic handles this internally for
      // a much nicer keyboard experience.
      cordova.plugins.Keyboard.disableScroll(true);
    }
    if (window.StatusBar) {
      StatusBar.styleDefault();
    }
  });
});

app.config(function (localStorageServiceProvider) {
  localStorageServiceProvider
    .setPrefix('ionic-todo')
});

app.controller('main', function ($scope, $ionicModal, localStorageService, $http,$timeout) {
  alert("In controller");
  var self = this;

  //-----------------------------------variables----------------------------------------

  self.localStorage;
  self.is_login = false;
  self.login_button = false;
  self.user = {};
  self.recUrl = window.localStorage;


  //---------------------------------functions--------------------------------------

  self.googleLogin = function () {
    //get auth token
    //alert("In login function");
    window.open('https://accounts.google.com/o/oauth2/auth?client_id=884260500709-fipghj8p51va803dlfmd3ka867j6lft7.apps.googleusercontent.com&login_hint=gaurav.chauhan@searcelabs.com&redirect_uri=http://check-extension.dev-happierhr.appspot.com/happierSSO/sso/oauth2callback&scope=https://www.googleapis.com/auth/plus.me https://www.googleapis.com/auth/userinfo.email&approval_prompt=force&response_type=code&access_type=offline', '_system', 'location=no');
    //self.login();

  }

  self.setToken = function (accessTokenResponse) {
    //alert("In settoken");
    //alert(accessTokenResponse);
    //alert(JSON.stringify(accessTokenResponse.data.access_token));
    localStorage.access_token = accessTokenResponse.data.access_token;
    localStorage.refresh_token = accessTokenResponse.data.refresh_token;
    //alert("Out settoken");
  }

  self.setAccessToken = function (accessTokenResponse) {
    //alert("In setAccessToken");
    localStorage.access_token = accessTokenResponse.data.access_token;
    //alert("Out setAccessToken");
  }

  self.apiCall = function () {
    var url="https://www.googleapis.com/oauth2/v1/userinfo?alt=json&access_token=" + localStorage.access_token;
    //alert(url);
    $http.get(url)
      .then(
        function (response) {
          //alert(JSON.stringify(response));
          self.user = response.data;
          //alert("Out apiCall");
          //alert('before :' + self.is_login);
          self.is_login = true;
          self.login_button = false;
          //alert('after :' + self.is_login);
          //alert("Out apiCall");
        },
        function (error) {
          //alert('error in apiCall token : ' + JSON.stringify(error));
        }
      );
  }
  self.refresh = function () {

    //alert("refreshing token : " + localStorage.refresh_token);

    var params ='&client_id=884260500709-fipghj8p51va803dlfmd3ka867j6lft7.apps.googleusercontent.com'
      + '&client_secret=g669HxbX65PCT_3jzXeSZEVG'
      + '&refresh_token=' + localStorage.refresh_token
      + '&grant_type=refresh_token';

    $http.post('https://accounts.google.com/o/oauth2/token', params, {headers: {'Content-Type':'application/x-www-form-urlencoded'}}
    ).then(
      function successCallback(response) {
        self.setAccessToken(response);
      },
      function failureCallback(error) {
        //alert('error in refreshing token : ' + JSON.stringify(error));
      }
    );
  }

  self.login = function () {
    //alert('login function');
    //self.showLoader = false;
    //alert(self.recUrl.getItem("receivedUrl"));
    if (self.recUrl.getItem("receivedUrl")) {
      //alert('login If');

      if (localStorage.access_token) {
        //alert('Access token present');
        self.refresh();
        self.apiCall();
      }
      else {
        //alert('access token absent');

        var code = self.recUrl.getItem("receivedUrl");
        code = code.split("@@")[1];


        var creds = '&code=' + code
          + '&client_id=884260500709-fipghj8p51va803dlfmd3ka867j6lft7.apps.googleusercontent.com'
          + '&client_secret=g669HxbX65PCT_3jzXeSZEVG'
          + '&redirect_uri=http://check-extension.dev-happierhr.appspot.com/happierSSO/sso/oauth2callback'
          + '&grant_type=authorization_code';

        $http.post('https://accounts.google.com/o/oauth2/token',
          creds,
          {headers: {'Content-Type':'application/x-www-form-urlencoded'}}
        ).then(
          function successCallback(response) {
            self.setToken(response);
            self.apiCall();
          },
          function failureCallback(error) {
            //alert('error in getting token : ' + JSON.stringify(error));
          }
        );
      }
    }
    else {
      //alert('login Else');
      self.googleLogin();
    }
  }
  self.refreshedLogin = function () {
    if (self.recUrl.getItem("receivedUrl")) {
      self.refresh();
      self.apiCall();
    }
  }
  self.logout = function () {
    self.is_login = false;
    self.login_button = true;
    self.user = {};
    self.recUrl.removeItem("receivedUrl");
  }

  function initController() {
    //alert('start');
    //alert(self.recUrl.getItem("receivedUrl"));
    //self.showLoader = true;self.googleLogin();

    if (self.recUrl.getItem("receivedUrl")) {
      self.login();
    }
    else{self.login_button = true;}
  }
  function doSomething()
  {
    $timeout(function(){

      alert('Cool');
      self.login();

    });
  }

  if (document.addEventListener) {
    document.addEventListener('someEvent', doSomething, false);
  } else {
    document.attachEvent('someEvent', doSomething);
  }

  initController();

});
