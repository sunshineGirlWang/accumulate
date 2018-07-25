  ### 正则表达式获取url参数
  
    /**
    * 获取当前URL参数值
    * @param name	参数名称
    * @return	参数值
    */
    getUrlParam(name) {
      var reg = new RegExp('[?&]'+name+'=([^&#]+)');
      var r = window.location.hash.substr(1).match(reg);
      if (r != null) return unescape(r[1]);
      return "";
    }

形如：http://localhost:8080/#/wqq/res/resList/detail?resGroupId=151#1

参考资料：https://blog.csdn.net/wqq1018893786/article/details/73895250
