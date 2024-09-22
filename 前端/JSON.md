###### 嵌套json的访问方法
`myObj = { "name":"runoob",` 
`"alexa":10000,`
`"sites": {` 
`"site1":"www.runoob.com",` 
`"site2":"m.runoob.com",`
`"site3":"c.runoob.com"`
    `}`
`}`

x = myObj.sites.site1; // 或者 
x = myObj.sites["site1"];

###### json数组的访问方式

myObj = {
	"name":"网站",
	"num":3,
	"sites":[ "Google", "Runoob", "Taobao" ]
}
x = myObj.sites[0];
###### json.parse()
将字符串转换成json数据
var obj = JSON.parse('{ "name":"runoob", "alexa":10000, "site":"www.runoob.com" }');

###### json.stringnify()
将json数据转换成字符串
var obj = { "name":"runoob", "alexa":10000, "site":"www.runoob.com"}; 
var myJSON = JSON.stringify(obj);