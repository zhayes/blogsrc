---
title: 日历插件 - ZCalendar.js
date: 2018-07-11 13:02:15
tags: javascript
category: 前端
---
![](https://images.unsplash.com/photo-1493934558415-9d19f0b2b4d2?ixlib=rb-1.2.1&auto=format&fit=crop&w=1200&q=10)
<!-- more -->



## ZCalendar.js
本想给公司写个专门的日历插件，结果写到中间，说不需要了，遂终！

用法及其简单，当然目前只具备展示的功能，其他功能尚待完善

完善日期未知……

我的设想是不依托于其他 jQ或 moment.js 等任何js库，力求精简
#### Demo

```html
    
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="./ZCalendar.css">
</head>
<body>
    <div id='calendar' ></div>
    <script src='./ZCalendar.js'></script>
    <script>
        var ZCalendar = new ZCalendar({
            container:'#calendar'
        })
    </script>
</body>
</html>

```
不传 y[Number]，m[Number] 的情况下默认为当前时间

源码如下

```javascript
    
/*
*********************************************************************************
****  config [object | {container[String] , y[Number], m[Number]}];
*********************************************************************************
*/

function ZCalendar(config) {
    this.presentTime = new Date();


    this.container = config.container;

    this.currentMoment = new Date(config.y?config.y: this.presentTime.getFullYear() , config.m?config.m-1 : this.presentTime.getMonth() );

    this.year = this.currentMoment.getFullYear();
    this.month = this.currentMoment.getMonth() + 1;

    this.date = new Date().getDate();

    this.init();
}

ZCalendar.prototype = {
    init: function () {
        this.startDay = this.getStartDay();//该月1号为星期几；
        this.endDay = this.getEnDay();//该月最后一天为星期几；

        this.startDate = 1,//该月从 1 号开始；
            this.endDate = this.getEndDate();//该月 最后一天为几号

        this.totalDays = this.endDate;//总天数

        this.renderCalendar();

        document.getElementById('zc-prev').onclick = this.prevClick.bind(this);
        document.getElementById('zc-next').onclick = this.nextClick.bind(this);

    },

    getStartDay: function () {//获取 该月 1 号为星期几；
        return this.currentMoment.getDay();
    },

    getEndDate: function () {// 获取 该月 最后 一日 为 几号；
        return new Date(this.year, this.month, 0).getDate();
    },

    getEnDay: function () {// 获取 该月 最后 一日 为 星期几;
        return new Date(this.year, this.month - 1, this.getEndDate()).getDay();
    },

    daysAlias: [ //中文星期几的别名
        '周日', '周一', '周二', '周三', '周四', '周五', '周六'
    ],

    getChineseAliasDays: function (day) {//获取中文对应的星期几别名；
        return daysAlias[day];
    },

    renderDateTd: function (date) {//渲染每个日期单元；

        return '<td class="currentTime' + ((date == this.date) && (this.year == this.presentTime.getFullYear() && (this.month == this.presentTime.getMonth()+1) ) ? ' active' : '') + '" >' + date + '</td>';
    },

    renderStartEmptyTd: function () {//渲染 每月开始之前的 多余的 周几 的空格 占位符；
        return '<td></td>';
    },

    renderEndEmptyTd: function () {

    },

    renderCalendarBody: function () {
        var calendarTds = '';

        for (var j = 0; j < 6; j++) {
            calendarTds += '<tr>';

            for (var i = 0; i < 7; i++) {
                var index = j * 7 + i;

                if (index < this.startDay || index >= (this.endDate + this.startDay)) {
                    calendarTds += this.renderStartEmptyTd();
                }

                if (index >= this.startDay && index < (this.endDate + this.startDay)) {
                    calendarTds += this.renderDateTd(index - this.startDay + 1);
                }

            }

            calendarTds += '</tr>'
        }

        return '<tbody>' + calendarTds + '</tbody>';
    },

    renderCalendarHead: function () {
        var head = '<thead><tr>'
        for (var i = 0; i < this.daysAlias.length; i++) {
            head += this.renderDateTd(this.daysAlias[i]);
        }
        head += '</tr></thead>'
        return head;
    },

    renderTimeShowBox: function () {
        var timeBox = '<div class="zc-timebox">' +
            '<div class="zc-time">' +
            '<span id="zc-prev"> < </span>' +
            '<span class="zc-time-item">' + this.year + '</span>年' +
            '<span class="zc-time-item">' + this.month + '</span>月' +
            '<span id="zc-next"> > </span>' +
            '</div>' +
            '</div>';
        return timeBox;
    },

    renderCalendarTable: function () {
        var tableHead = this.renderCalendarHead();
        var tableBody = this.renderCalendarBody();

        var calendarTabel = '<table class="zc-table" width="100%" border=0 cellspacing=0 >' +
            tableHead + tableBody + '</table>';
        return calendarTabel;
    },

    renderCalendar: function () {
        var calender = '';
        calender += this.renderTimeShowBox() + this.renderCalendarTable();

        calender = '<div>' + calender + '</div>';

        document.querySelector(this.container).innerHTML = calender;
    },


    prevClick: function () {
        this.currentMoment = new Date(this.year, this.month - 2);

        this.year = this.currentMoment.getFullYear();
        this.month = this.currentMoment.getMonth() + 1;

        this.date = new Date().getDate();

        this.init();
    },

    nextClick: function () {
        this.currentMoment = new Date(this.year, this.month);

        this.year = this.currentMoment.getFullYear();
        this.month = this.currentMoment.getMonth() + 1;

        this.date = new Date().getDate();

        this.init();
    }
}
    
```