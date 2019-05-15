### JS浏览器关闭时清空cookie

    **设置cookie过期时间为0**

    function addCookie(objName,objValue,objHours) {
        var str = objName + "=" + escape(objValue);
        if(objHours > 0){//等于0时，关闭浏览器自动清除Cookies.
         var date = new Date();
         var ms = objHours*3600*1000;
         date.setTime(date.getTime() + ms);
         str += "; expires=" + date.toGMTString();
        }
        document.cookie = str;
    }

