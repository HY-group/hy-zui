section: javascript
id: daterangepicker
description: 为文本框提供日期范围选择功能【HY新增】
icon: icon-calendar
filter: riqixuanze rqxz
---

# 日期范围选择

日期范围选择插件可以帮助用户更方便快捷的选择一定范围内的日期。

该空间是华扬在ZUI的基础上添加的控件，日期范围选择控件基于开源项目 [Date Range Picker](https://github.com/dangrossman/bootstrap-daterangepicker/) 定制开发。


## 使用前注意
必须引入如下文件：
```html
<link rel="stylesheet" href="zui/lib/daterangepicker/daterangepicker.css">
<link rel="stylesheet" href="zui/lib/daterangepicker/daterangepicker-hy-extend.css">
```

```html
<script src="zui/lib/moment.min.js"></script>
<script src="zui/lib/daterangepicker/daterangepicker.js"></script>
<script src="zui/lib/daterangepicker/daterangepicker-hy-extend.js"></script>
```
请在使用组件的页面引入如上css和js文件，根据项目路径调整引用文件路径。

hy-extend的css和js文件是我们针对daterangepicker组件进行的样式和语种扩展。


## 日期范围选择 两端独立【日期开始和结束点击左右箭头独立切换】

<div class="example">
  <input type="text" class="form-control daterange daterangeNoLinked" placeholder="选择时间范围：yyyy-MM-dd">
</div>

## 用法

```js
// 日期范围选择 两端独立【日期开始和结束点击左右箭头独立切换】
$('.daterangeNoLinked').daterangepicker({
    locale: {
        format: 'YYYY-MM-DD'
    },
    maxDate: yesterday_date,// 最大可选日期
    minDate: min_date,// 最小可选日期
    opens: "right",
    drops: "down",
    showDropdowns :true,
    startDate: start_date,
    endDate: end_date,
    autoApply: true,
    autoUpdateInput: true,
    linkedCalendars: false// 开始日期和结束日期点击左右箭头时是否联动
}).change(function() {
    console.log("日期改变了，可以处理数据");
});
```

## 日期范围 两端联动【日期开始和结束点击左右箭头联动切换】

<div class="example">
  <input type="text" class="form-control daterange daterangeLinked" placeholder="选择时间范围：yyyy-MM-dd">
</div>

## 用法

```js
// 日期范围选择 两端联动【日期开始和结束点击左右箭头联动切换】
$('.daterangeLinked').daterangepicker({
    locale: {
        format: 'YYYY-MM-DD'
    },
    maxDate: yesterday_date,// 最大可选日期
    minDate: min_date,// 最小可选日期
    opens: "right",
    drops: "down",
    showDropdowns :true,
    startDate: start_date,
    endDate: end_date,
    autoApply: true,
    autoUpdateInput: true,
    linkedCalendars: true// 开始日期和结束日期点击左右箭头时是否联动
}).change(function() {
    console.log("日期改变了，可以处理数据");
});
```

## 昨天、一周、一月

点击昨天、一周、一月时可以快速设置时间范围

<div class="example">
  <div class="btn-group" data-toggle="buttons">
    <span class="btn daterange-yesterday active">
      昨天
    </span>
    <span class="btn daterange-week">
      近一周
    </span>
    <span class="btn daterange-month">
      近一月
    </span>
    <span class="btn daterange-diy">
      自定义：
    </span>
    <input type="text" class="form-control daterange daterangeAppoint" placeholder="选择时间范围：yyyy-MM-dd">
  </div>
</div>

## 用法

```js
// 昨天、一周、一月
function daterangeAppoint() {
    var start_week = moment().add(-7, 'days').format("YYYY-MM-DD"),
        start_month = moment().add(-30, 'days').format("YYYY-MM-DD")
        yesterday_date = moment().add(-1, 'days').format("YYYY-MM-DD"),
        min_date = moment().add(-365, 'days').format("YYYY-MM-DD"),
        start_date = start_week,
        end_date = yesterday_date;

    // 改变日期
    $('.daterangeAppoint').daterangepicker({
        locale: {
            format: 'YYYY-MM-DD'
        },
        maxDate: yesterday_date,
        minDate: min_date,
        opens: "right",
        drops: "down",
        showDropdowns :true,
        startDate: start_date,
        endDate: end_date,
        autoApply: true,
        autoUpdateInput: true,
        linkedCalendars: false
    }).change(function() {
        updateDateState();
    });
    updateDateState();

    // 点击时统一处理时间设置
    function setDate(startDate, endDate) {
        $('.daterangeAppoint').daterangepicker({
            locale: {
                format: 'YYYY-MM-DD'
            },
            maxDate: yesterday_date,
            showDropdowns :true,
            startDate: startDate,
            endDate: endDate,
            autoApply: true,
            linkedCalendars: false
        });
    };

    $('.daterange-diy').click(function(){
        var box = $(".daterange-diy").parent();
        box.find("span").removeClass("active");
        setTimeout(function() {
          $(".daterange-diy").addClass('active');
        }, 100);
        $('.daterangeAppoint').click();
    });
    // 处理外部点击变更日历
    $(".daterange-yesterday").click(function() {
        setDate(moment().subtract(1, 'days'), moment().subtract(1,'days'));
        return false;
    });
    $(".daterange-week").click(function() {
        setDate(moment().subtract(7, 'days'), moment().subtract(1,'days'));
        return false;
    });
    $(".daterange-month").click(function() {
        setDate(moment().subtract(30, 'days'), moment().subtract(1,'days'));
        return false;
    });

    /**
     * 修正时间范围的当前状态
     */
    function updateDateState() {;
        var box = $(".daterange-diy").parent(),
            daterange = $(".daterangeAppoint").val().split(' - '),
            startDate = daterange[0],
            endDate = daterange[1];
        // 清空现有状态
        box.find("span").removeClass("active");

        // 设置当前状态
        if ( startDate == yesterday_date && endDate == yesterday_date ) {
            $(".daterange-yesterday").addClass('active');
        } else if ( startDate == start_week && endDate == yesterday_date ) {
            $(".daterange-week").addClass('active');
        } else if ( startDate == start_month && endDate == yesterday_date ) {
            $(".daterange-month").addClass('active');
        } else {
            $(".daterange-diy").addClass('active');
        }

        console.log("更新数据");
    };
};
daterangeAppoint();
```


## 单独日期选择

<div class="example">
  <input type="text" class="form-control daterange daterangeOnly" placeholder="选择一个日期：yyyy-MM-dd">
</div>

## 用法

```js
// 单独日期选择
$('.daterangeOnly').daterangepicker({
    locale: {
        format: 'YYYY-MM-DD'
    },
    singleDatePicker: true,// 单日历设置
    maxDate: yesterday_date,
    minDate: min_date,
    showDropdowns :true,
    startDate: start_date
}).change(function() {
    console.log("日期改变了，可以处理数据");
});
```

<link rel="stylesheet" href="dist/lib/daterangepicker/daterangepicker.css">
<link rel="stylesheet" href="dist/lib/daterangepicker/daterangepicker-hy-extend.css">
<script>
function onPageClose() {
    // if($.fn.datetimepicker) $('#pageBody').find('.form-date,.form-datetime,.form-time').datetimepicker('remove');
}
function onPageLoad() {
    onPageClose();
}
function afterPageLoad() {
    $.getScript('dist/lib/daterangepicker/moment.min.js', function() {
        $.getScript('dist/lib/daterangepicker/daterangepicker.js', function() {
            $.getScript('dist/lib/daterangepicker/daterangepicker-hy-extend.js', function() {
               init();
            });
        });
    });
}

function init() {
    console.log("init");
    // 初始化日历
    var start_week = moment().add(-7, 'days').format("YYYY-MM-DD"),
        yesterday_date = moment().add(-1, 'days').format("YYYY-MM-DD"),// 昨天
        min_date = moment().add(-365, 'days').format("YYYY-MM-DD"),
        start_date = start_week,
        end_date = yesterday_date;

    $('.daterangeNoLinked').daterangepicker({
        locale: {
            format: 'YYYY-MM-DD'
        },
        maxDate: yesterday_date,
        minDate: min_date,
        opens: "right",
        drops: "down",
        showDropdowns :true,
        startDate: start_date,
        endDate: end_date,
        autoApply: true,
        autoUpdateInput: true,
        linkedCalendars: false
    }).change(function() {
        console.log("日期改变了，可以处理数据");
    });

    // 日期范围选择 两端联动【日期开始和结束点击左右箭头联动切换】
    $('.daterangeLinked').daterangepicker({
        locale: {
            format: 'YYYY-MM-DD'
        },
        maxDate: yesterday_date,// 最大可选日期
        minDate: min_date,// 最小可选日期
        opens: "right",
        drops: "down",
        showDropdowns :true,
        startDate: start_date,
        endDate: end_date,
        autoApply: true,
        autoUpdateInput: true,
        linkedCalendars: true// 开始日期和结束日期点击左右箭头时是否联动
    }).change(function() {
        console.log("日期改变了，可以处理数据");
    });


    // 昨天、一周、一月
    function daterangeAppoint() {
        var start_week = moment().add(-7, 'days').format("YYYY-MM-DD"),
            start_month = moment().add(-30, 'days').format("YYYY-MM-DD")
            yesterday_date = moment().add(-1, 'days').format("YYYY-MM-DD"),
            min_date = moment().add(-365, 'days').format("YYYY-MM-DD"),
            start_date = start_week,
            end_date = yesterday_date;

        // 改变日期
        $('.daterangeAppoint').daterangepicker({
            locale: {
                format: 'YYYY-MM-DD'
            },
            maxDate: yesterday_date,
            minDate: min_date,
            opens: "right",
            drops: "down",
            showDropdowns :true,
            startDate: start_date,
            endDate: end_date,
            autoApply: true,
            autoUpdateInput: true,
            linkedCalendars: false
        }).change(function() {
            updateDateState();
        });
        updateDateState();

        // 点击时统一处理时间设置
        function setDate(startDate, endDate) {
            $('.daterangeAppoint').daterangepicker({
                locale: {
                    format: 'YYYY-MM-DD'
                },
                maxDate: yesterday_date,
                showDropdowns :true,
                startDate: startDate,
                endDate: endDate,
                autoApply: true,
                linkedCalendars: false
            });
        };

        $('.daterange-diy').click(function(){
            var box = $(".daterange-diy").parent();
            box.find("span").removeClass("active");
            setTimeout(function() {
              $(".daterange-diy").addClass('active');
            }, 100);
            $('.daterangeAppoint').click();
        });
        // 处理外部点击变更日历
        $(".daterange-yesterday").click(function() {
            setDate(moment().subtract(1, 'days'), moment().subtract(1,'days'));
            return false;
        });
        $(".daterange-week").click(function() {
            setDate(moment().subtract(7, 'days'), moment().subtract(1,'days'));
            return false;
        });
        $(".daterange-month").click(function() {
            setDate(moment().subtract(30, 'days'), moment().subtract(1,'days'));
            return false;
        });

        /**
         * 修正时间范围的当前状态
         */
        function updateDateState() {;
            var box = $(".daterange-diy").parent(),
                daterange = $(".daterangeAppoint").val().split(' - '),
                startDate = daterange[0],
                endDate = daterange[1];
            // 清空现有状态
            box.find("span").removeClass("active");

            // 设置当前状态
            if ( startDate == yesterday_date && endDate == yesterday_date ) {
                $(".daterange-yesterday").addClass('active');
            } else if ( startDate == start_week && endDate == yesterday_date ) {
                $(".daterange-week").addClass('active');
            } else if ( startDate == start_month && endDate == yesterday_date ) {
                $(".daterange-month").addClass('active');
            } else {
                $(".daterange-diy").addClass('active');
            }

            console.log("更新数据");
        };
    };
    daterangeAppoint();



    // 单独日期选择
    $('.daterangeOnly').daterangepicker({
        locale: {
            format: 'YYYY-MM-DD'
        },
        singleDatePicker: true,// 单日历设置
        maxDate: yesterday_date,
        minDate: min_date,
        showDropdowns :true,
        startDate: start_date
    }).change(function() {
        console.log("日期改变了，可以处理数据");
    });
}
</script>
