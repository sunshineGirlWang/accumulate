1、利用iframe实现（下载的文件域名不好直接写）

  html：
  
    <a href="javascript:Slide.downLoadCooperationAgreement();">年度合作协议</a>
    
  js：
  
    Slide.downLoadCooperationAgreement = function(){
        var url = System.domainUrlTotUedPxy[System.testTotPxyFlag]+'/download/annualCooperationAgreement_v1.docx';
        var size = $("#cooperationAgreement_download").size();
        if (size == 0) {
           $("body").append("<iframe id = 'cooperationAgreement_download' style='display:none;' src='"+url+"'></iframe>");
        } else {
          $("#cooperationAgreement_download").attr("src", url);
        }
      };

2、利用a标签实现，download属性可以用来命名下载的文件。

    <a href="/i/w3school_logo_white.gif" download="w3logo">
      <img border="0" src="/i/w3school_logo_white.gif" alt="W3School">
    </a>

