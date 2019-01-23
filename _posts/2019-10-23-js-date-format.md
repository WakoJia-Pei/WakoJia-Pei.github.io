---

title: "js 对日期的格式化处理"
layout: post
category: note
tags: [js]
excerpt: "js 对日期的格式化处理"

---

将以下代码复制粘贴到一个后缀名为js的文件中, 使用node即可运行, 测试输出结果

```
let date_var = new Date();
// console.log("日期参数类型输入=>> ", typeof date_var, date_var)
console.log("①日期参数格式化结果=>> ", dateToString(date_var, 'yyyy-MM-dd'))

/*
 *@params str:源字符串; 
 *@description 不够位数的,左侧用"0"补位
 */
function padLeftZero(str) {
  return ('00' + str).substr(str.length)
}

/*
 *@params date:Date格式的日期; fmt:目标格式 'yyyy-MM-dd'
 *@description Date格式的日期转为字符串格式日期
 */
function dateToString(date, fmt) {
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  let o = {
    'M+': date.getMonth() + 1,
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds()
  }
  for (let k in o) {
    if (new RegExp(`(${k})`).test(fmt)) {
      let str = o[k] + ''
      fmt = fmt.replace(RegExp.$1, RegExp.$1.length === 1 ? str : padLeftZero(str)) 
    }
  }
  return fmt
}

/*
 *@params dateStr:源字符串; separator:年月日的分离连接符号
 *@description 字符串格式的日期转为日期格式
 */
function stringToDate(dateStr, separator) {
  if (!separator) {
    separator = "-";
  }
  let dateArr = dateStr.split(separator);
  let year = parseInt(dateArr[0]);
  let month;
  //处理月份为04这样的情况                         
  if (dateArr[1].indexOf("0") == 0) {
    month = parseInt(dateArr[1].substring(1));
  } else {
    month = parseInt(dateArr[1]);
  }
  let day = parseInt(dateArr[2]);
  let date = new Date(year, month - 1, day);
  return date;
}

/*
 *第二种将日期转化为字符串的方法
 */
let SIGN_REGEXP = /([yMdhsm])(\1*)/g;
let DEFAULT_PATTERN = 'yyyy-MM-dd';
function padding(str, len) {
  let len = len - (str + '').length;
  for (let i = 0; i < len; i++) { str = '0' + str; }
  return str;
}
function format(date, pattern) {
  pattern = pattern || DEFAULT_PATTERN;
  return pattern.replace(SIGN_REGEXP, function ($0) {
    switch ($0.charAt(0)) {
      case 'y': return padding(date.getFullYear(), $0.length);
      case 'M': return padding(date.getMonth() + 1, $0.length);
      case 'd': return padding(date.getDate(), $0.length);
      case 'w': return date.getDay() + 1;
      case 'h': return padding(date.getHours(), $0.length);
      case 'm': return padding(date.getMinutes(), $0.length);
      case 's': return padding(date.getSeconds(), $0.length);
    }
  });
}
console.log("②日期参数格式化结果=>> ", format(date_var))

/*
 *js替换字符串中指定字符示例
 */
let date1 = "2019年01月01日"
date1 = date1.replace(new RegExp("年", 'g'), "-")
date1 = date1.replace(new RegExp("月", 'g'), "-")
date1 = date1.replace(/日/g, "")

```