# Excel

## 替换 TRUE 和 FALSE 为字符串

`=IF(D2,"a","b")`

如果要判断等于字符串，可以使用 `=IF(D2="target","a","b")`

## 转换 2018-09-07T13:59:54.879Z

`=DATEVALUE(MID(A1,1,10))+TIMEVALUE(MID(A1,12,8))` 日期类型
`=DATEVALUE(MID(A2,1,10))+TIMEVALUE(MID(A2,12,5))+TIME(MID(A2,18,2),0,0)` 有具体时间

之后需要右键->Format cells->Custom 在输入框中输入 `yyyy-mm-dd hh:mm:ss;@`
