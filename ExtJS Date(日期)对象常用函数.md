
# ExtJS Date(日期)对象常用函数 #

Ext.Date是一个单例类，封装了一系列日期操作函数，扩展JavaScript Date的功能，下面列出一些常用的功能。

Ext.Date.add(date, interval, value) 给date增加或减少时间，这个函数不改变原有Date对象的值，而是返回一个新的Date对象。

Ext.Date.between(date, start, end) 判断date是否在start和end之间。

Ext.Date.clearTime(date, clone) 把date的时间设置成00小时00分00秒000毫秒。

Ext.Date.clone(date) 克隆date的一个副本。

Ext.Date.format(date, format)把日期格式化，返回格式化后的字符串。

Ext.Date.getDayOfYear(date)获取date是年中的第几天。

Ext.Date.getDaysInMonth(date)获取date是月中的第几天。

Ext.Date.getFirstDateOfMonth(date)获取date所在月份的第一天。

Ext.Date.getFirstDayOfMonth(date)获取date所在月份第一天的星期。

Ext.Date.getLastDateOfMonth(date)获取date所在月份的最后一天。

Ext.Date.getLastDayOfMonth(date)获取date所在月份最后一天的星期。

Ext.Date.getWeekOfYear(date) 获取date所在年中的第几个星期。

Ext.Date.isLeapYear(date)date所在年份是否闰年。

Ext.Date.now() 返回当前时间到1970年1月1日的毫秒数。在chrome、ie9和ie10中已经有Date.now()实现相同的功能。

Ext.Date.parse(input, format, strict)根据输入的字符串创建日期，Date.parse()有类似的功能。

```
// Ext.Date.add(date, interval, value) 给date增加或减少时间，这个函数不改变原有Date对象的值，而是返回一个新的Date对象。
// @param   {Date}    date      原日期对象。
// @param   {String}  interval  value的单位，可以选Ext.Date.DAY、Ext.Date.HOUR、Ext.Date.MINUTE、Ext.Date.MONTH、
//                              Ext.Date.SECOND、Ext.Date.YEAR、Ext.Date.MILLI。
// @param   {number}  value     日期对象需要增加的值。
// @return  {Date}              返回增加值后的Date对象。
// Example
var date = Ext.Date.add(new Date('10/29/2006'), Ext.Date.DAY, 5); //增加5天
console.log(date); //返回结果 Fri Nov 03 2006 00:00:00 GMT+0800 (中国标准时间)
 
var date = Ext.Date.add(new Date('10/29/2006'), Ext.Date.DAY, -5); //减少5天，如果值是负数，则减少。
console.log(date); //返回结果 Tue Oct 24 2006 00:00:00 GMT+0800 (中国标准时间)
 
var date = Ext.Date.add(new Date('10/29/2006'), Ext.Date.YEAR, 2); //增加2年
console.log(date); //返回结果 Wed Oct 29 2008 00:00:00 GMT+0800 (中国标准时间)
 
 
// Ext.Date.between(date, start, end)  判断date是否在start和end之间。
// @param   {Date}     date   要判断的日期。
// @param   {Date}     start 
// @param   {Date}     end
// @return  {Boolean}         如果date在start和end之间返回true，否则返回false。
// Example
var date = new Date('10/29/2006');
var start = new Date('10/5/2006');
var end = new Date('11/15/2006');
Ext.Date.between(date, start, end); //返回true
 
 
// Ext.Date.clearTime(date, clone)  把date的时间设置成00小时00分00秒000毫秒。
// @param   {Date}     date 
// @param   {Bollean}  clone  可选参数。如果为true则返回date的一个副本，如果为false则返回date本身，默认为false。
// @return  {Date}            返回设置后的日期。
// Example
var date = new Date('10/30/2012 14:30:00');
Ext.Date.clearTime(date);  //返回 Tue Oct 30 2012 00:00:00 GMT+0800 (中国标准时间)
 
 
// Ext.Date.clone(date)  克隆date的一个副本。
// @param   {Date}  date
// @return  {Date}  返回克隆后的Date。
// Example
var orig = new Date('10/30/2012');
var copy = Ext.Date.clone(orig); //克隆一个值
 
 
// Ext.Date.format(date, format)  把日期格式化，返回格式化后的字符串。
// @param   {Date}    date    日期。
// @param   {String}  format  日期格式，Y-年，m-月，d-日，H-24小时，i-分钟，s-秒
// @return  {String}          返回格式化后的字符串。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.format(date, 'Y-m-d H:i:s');        // 2012-10-20 14:30:00
Ext.Date.format(date, 'Y年m月d日 H:i:s');    // 2012年10月20日 14:30:00
 
 
// Ext.Date.getDayOfYear(date)  获取date是年中的第几天
// @param   {Date}    date  日期。
// @return  {Number}        返回天数，取值范围0~364，如果是闰年则有365。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getDayOfYear(date); //返回 293
 
 
// Ext.Date.getDaysInMonth(date)  获取date是月中的第几天
// @param   {Date}    date  日期。
// @return  {Number}        返回天数。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getDayOfYear(date); //返回 31
 
 
// Ext.Date.getFirstDateOfMonth(date)  获取date所在月份的第一天
// @param   {Date}  date  日期。
// @return  {Date}        返回所在月份的第一天。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getFirstDateOfMonth(date); //返回 Mon Oct 01 2012 00:00:00 GMT+0800 (中国标准时间)
 
 
// Ext.Date.getFirstDayOfMonth(date)  获取date所在月份第一天的星期
// @param   {Date}    date  日期。
// @return  {Number}        返回所在月份第一天的星期，取值范围0~6。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getFirstDayOfMonth(date); //返回 1，表示星期一
 
 
// Ext.Date.getLastDateOfMonth(date)  获取date所在月份的最后一天
// @param   {Date}  date  日期。
// @return  {Date}        返回所在月份的最后一天。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getLastDateOfMonth(date); //返回 Wed Oct 31 2012 00:00:00 GMT+0800 (中国标准时间)
 
 
// Ext.Date.getLastDayOfMonth(date)  获取date所在月份最后一天的星期
// @param   {Date}    date  日期。
// @return  {Number}        返回所在月份最后一天的星期，取值范围0~6。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getLastDayOfMonth(date); //返回 3，表示星期三
 
 
// Ext.Date.getWeekOfYear(date)  获取date所在年中的第几个星期
// @param   {Date}    date  日期。
// @return  {Number}        返回所在年中的第几个星期，取值范围1~53。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.getWeekOfYear(date); //返回 42
 
 
// Ext.Date.isLeapYear(date)  date所在年份是否闰年
// @param   {Date}     date  日期。
// @return  {Boolean}        true表示闰年，false表示平年。
// Example
var date = new Date('10/20/2012 14:30:00');
Ext.Date.isLeapYear(date); //返回 true
 
 
// Ext.Date.now()     返回当前时间到1970年1月1日的毫秒数。
//                    在chrome、ie9和ie10中已经有Date.now()实现相同的功能。
// @return  {Number}  返回毫秒数。
// Example
var timestamp = Ext.Date.now();  //1351666679575
var date = new Date(timestamp);  //Wed Oct 31 2012 14:57:59 GMT+0800 (中国标准时间)
 
 
// Ext.Date.parse(input, format, strict)  根据输入的字符串创建日期，Date.parse()有类似的功能。
// @param {String}   input   日期字符串。
// @param {String}   format  日期格式。
// @param {Boolean}  strict  验证input字符串的有效性，默认是false。
// @param {Date}             返回创建的日期。
// Example
var input = '2012年10月31日 14:30:00';
var format = 'Y年m月d日 H:i:s';
var date = Ext.Date.parse(input, format, true);  //Wed Oct 31 2012 14:30:00 GMT+0800 (中国标准时间)

```

