必须写event.preventDefault();不然第一次登录会无效，原因是点击button后，form自己会提交一次，之后ajax提交的就是空的，所以需要这行代码阻止默认行为

`$(document).ready(function(){`  
    `$('button').click(function(event){`  
        `event.preventDefault();`  
        `//Step2.获取表单内所有的数据【各个input 一定要有name属性！】`  
        `var str = $('form').serialize();`  
        `alert("点击button")`  
        `//Step3.发送Ajax请求`  
        `$.ajax({`  
            `url: "/login", //后端地址`  
            `type: "text",       //提交方式`  
            `method: "POST",`  
            `data: str,`  
            `async: true,`  
            `success: function (response) {`  
                `alert("成功")`  
                `//请求成功之后进入该方法，data为成功后返回的数据`  
                `alert(response)`  
                `if(response == '2'){`  
             
                    `window.location.href = '/user/login'; // 跳转到仪表盘页面`  
                `}else{`  
                    `alert("用户名或密码错误");`  
                `}`  
            `},`  
            `error: function (errorMsg) {`  
                `console.log(errorMsg);`  
                `console.log("发送ajax失败");`  
            `}`  
        `});`  
    `})`  
  
`})`