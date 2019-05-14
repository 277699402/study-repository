# 通用原生Javascript 实现AJAX

    var XMLHTTP = {};
    /*
     * 通用原生Javascript 实现AJAX
     * method:  请求的类型；GET或POST
     * url:请求地址
     * callback:回调函数
     * key:xhr对象key
     * data:POST数据(可选)
     */
    function loadAjax(method, url, callback, key, data) {
        if (!data) {
            data = "";
        }
        if (window.XMLHttpRequest) { // code for IE7+, Firefox, Chrome, Opera, Safari
            XMLHTTP[key] = new XMLHttpRequest();
        } else { // code for IE6, IE5
            XMLHTTP[key] = new ActiveXObject("Microsoft.XMLHTTP");
        }
        XMLHTTP[key].open(method, url, true);
        if (method == 'POST') {
            XMLHTTP[key].setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        }
        XMLHTTP[key].onreadystatechange = callback;
        XMLHTTP[key].send(data);
    }



