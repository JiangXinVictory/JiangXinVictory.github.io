---
title: moment获取时间段
date: 2019-12-26 17:13:52
tags:
    - javaScript
---

使用moment获取一段时间，常用在日期选择器控件内的快捷选项，例如 今天，过去7天，过去1个月，当月 等
<!-- more -->

获取选中时间， 包含当天
```
export function getReleaseTime (times) {
  let st
  let et
  if (times.includes('今日')) {
    st = moment().format('YYYY-MM-DD')
    et = st
  } else if (times.includes('昨日')) {
    st = moment().subtract(1, 'days').format('YYYY-MM-DD')
    et = st
  } else if (times.includes('本周')) {
    st = moment().startOf('week').format('YYYY-MM-DD')
    et = moment().format('YYYY-MM-DD')
  } else if (times.includes('最近7天')) {
    st = moment().subtract(6, 'days').format('YYYY-MM-DD')
    et = moment().subtract(0, 'days').format('YYYY-MM-DD')
  } else if (times.includes('上周')) {
    let lastWeek = moment().subtract(1, 'week')
    st = lastWeek.startOf('week').format('YYYY-MM-DD')
    et = lastWeek.endOf('week').format('YYYY-MM-DD')
  } else if (times.includes('过去14天')) {
    st = moment().subtract(13, 'days').format('YYYY-MM-DD')
    et = moment().subtract(0, 'days').format('YYYY-MM-DD')
  } else if (times.includes('本月')) {
    st = moment().startOf('month').format('YYYY-MM-DD')
    et = moment().format('YYYY-MM-DD')
  } else if (times.includes('最近30天')) {
    st = moment().subtract(29, 'days').format('YYYY-MM-DD')
    et = moment().subtract(0, 'days').format('YYYY-MM-DD')
  } else if (times.includes('上月')) {
    st = moment().subtract(1, 'month').startOf('month').format('YYYY-MM-DD')
    et = moment().subtract(1, 'month').endOf('month').format('YYYY-MM-DD')
  } else if (times.includes('今年')) {
    st = moment().startOf('year').format('YYYY-MM-DD')
    et = moment().format('YYYY-MM-DD')
  } else if (times.includes('去年')) {
    st = moment().subtract(1, 'years').startOf('year').format('YYYY-MM-DD')
    et = moment().subtract(1, 'years').endOf('year').format('YYYY-MM-DD')
  } else {
    let regExp = /\d{4}-\d{2}-\d{2}/
    if (times[0].length === 10 && times[1].length === 10 && regExp.test(times[0]) && regExp.test(times[1])) {
      st = times[0]
      et = times[1]
    } else {
      st = moment().format('YYYY-MM-DD')
      et = st
    }
  }
  return [st, et]
}
```
