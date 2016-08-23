# util.js

```
import Vue from 'vue'
import helper from './helper'

export const isLogin = function() {
  return !!helper.get('token')
}

/* 弹出错误信息
    @param msg `String` 要显示的错误信息
    @param handler `Function` 显示完错误信息之后要执行的函数 - 可选
    @param duration `Number` 错误信息停留的时间（毫秒） - 可选
   Usage:
    toast(res.status.msg, function () {
      // do something...
    }, 1000)
*/
export const toast = function(msg, handler = function() {}, duration = 1000) {
  var tip = new Vue({
    data: {
      msg: msg
    },
    template: '<div class="cp-toast" transition="scale"><table><tr><td><span class="msg">{{msg}}</span></td></tr></table></div>',
    ready() {
      var _this = this;
      if (duration != 'forever') {
        setTimeout(function() {
          _this.$destroy(true);
        }, duration);
      }
    },
    destroyed() {
      handler();
    }
  });
  tip.$mount().$appendTo('body');
}


/*获取本周、本月、本年起止日期
 * return {}
 */
export const getNowDate = function() {

  var now = new Date(); //当前日期
  var nowDayOfWeek = now.getDay(); //今天本周的第几天
  var nowDay = now.getDate(); //当前日
  var nowMonth = now.getMonth(); //当前月
  var nowYear = now.getYear(); //当前年
  nowYear += (nowYear < 2000) ? 1900 : 0; //

  //格式化日期：yyyy-MM-dd
  function formatDate(date) {
    var myyear = date.getFullYear();
    var mymonth = date.getMonth() + 1;
    var myweekday = date.getDate();

    if (mymonth < 10) {
      mymonth = "0" + mymonth;
    }
    if (myweekday < 10) {
      myweekday = "0" + myweekday;
    }
    return (myyear + "-" + mymonth + "-" + myweekday);
  }

  //获得某月的天数
  function getMonthDays(myMonth) {
    var monthStartDate = new Date(nowYear, myMonth, 1);
    var monthEndDate = new Date(nowYear, myMonth + 1, 1);
    var days = (monthEndDate - monthStartDate) / (1000 * 60 * 60 * 24);
    return days;
  }



  return {
    nowYearStart: formatDate(new Date(nowYear, 0, 1)),
    nowYearEnd: formatDate(new Date(nowYear, 11, 31)),
    nowMonthStart: formatDate(new Date(nowYear, nowMonth, 1)),
    nowMonthEnd: formatDate(new Date(nowYear, nowMonth, getMonthDays(nowMonth))),
    nowWeekStart: formatDate(new Date(nowYear, nowMonth, nowDay - nowDayOfWeek + 1)),
    nowWeekEnd: formatDate(new Date(nowYear, nowMonth, nowDay + (6 - nowDayOfWeek + 1)))
  }
}


export const browser = {
  v: function() {
    var u = navigator.userAgent,
      app = navigator.appVersion;
    return { //移动终端浏览器版本信息
      trident: u.indexOf('Trident') > -1, //IE内核
      presto: u.indexOf('Presto') > -1, //opera内核
      webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
      gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
      mobile: !!u.match(/AppleWebKit.*Mobile.*/) || !!u.match(/AppleWebKit/), //是否为移动终端
      ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
      android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
      iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQ HD浏览器
      iPad: u.indexOf('iPad') > -1, //是否iPad
      webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
    };
  }(),
  language: (navigator.browserLanguage || navigator.language).toLowerCase()
}


```
