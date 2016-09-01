/**
 * Created by 32921 on 16-8-17.
 */

function request(path, strData, cbSuccess, cbError) {
    var domain = get_url('domain')
    var url="http://"+domain+"/civil/live/";
    var data={};
    if(path=="getActivityById"){
        var urldata="?activityId="+strData.activityId;
    }else if(path=="queryComments"){
        var urldata="?activityId="+strData.activityId+"&commentId="+strData.commentId+"&count="+strData.count;
    }else{
        var urldata="";
        data=strData;
    }
    $.jsonp({
        data : data,
        type: "get",
        url:url+path+urldata,
        dataType: "jsonp",
        contentType:"application/jsonp; charset=UTF-8",
        callbackParameter: 'callback',
        jsonp: "jsonpcallback",
        success: function (json) {
            cbSuccess(json);
        },
        error: function (result) {
            cbError(result);
        }
    });

}
function getActivityById(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId
    };
    request("getActivityById", options, cbSuccess, cbError)
}
function clickLike(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId,
        token:options.token
    };
    request("clickLike", options, cbSuccess, cbError)
}
function queryLike(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId,
        token:options.token
    };
    request("queryLike", options, cbSuccess, cbError)
}
function getCode(options,cbSuccess, cbError){
    var options={
        code:options.code
    };
    request("getCode", options, cbSuccess, cbError)
}
function updatePlayTimes(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId,
        token:options.token
    };
    request("updatePlayTimes", options, cbSuccess, cbError)
}
function publishComments(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId,
        token:options.token,
        content: options.content,
        replyUserId:options.replyUserId,
        replyUserType:options.replyUserType
    };
    request("publishComments", options, cbSuccess, cbError)
}
function queryComments(options,cbSuccess, cbError){
    var options={
        activityId:options.activityId,
        commentId:options.commentId,
        count:options.count
    };
    request("queryComments", options, cbSuccess, cbError);
}
function getlivedetail(data){
    /*更改标题*/
    document.title=data.name;
    var time=new Date(parseInt(data.beginTime*1000));
    var timerString=time.getFullYear()+"年"+(parseInt(time.getMonth())+1)+"月"+time.getDate()+"日";
    $('.lookeye').html(data.playTimes);
    $('.lookhand').html(data.likeTimes);
    $('.livestartime-detail').html(timerString);
    $('.liveaddress-detail').html(data.address);
    /*初始化tab页默认最多4个tab*/
    if(data.description.length>3){
        var descriptionLength=3;
    }else{
        var descriptionLength=data.description.length;
    }
    for(var i=0;i<descriptionLength;i++){
        var _navlink=$('<div class="navlink">'+data.description[i].name+'</div>');
        $('.say').before(_navlink);
        if(i==0){
            var _div=$(data.description[0].content);
            $('.live-detail-describe').append(_div)
        }else{
            var _div=$('<div class="live-tab">'+data.description[i].content+'</div>');
            $('.live-say').before(_div);
        }
    }
    $('.live-tab').hide();
    $('.live-detail').show();
    $('.navlink').eq(0).addClass('navlinkon detail');
    /*tab按钮切换*/
    $('.navlink').click(function(){
        $('.navlink').removeClass('navlinkon');
        $(this).addClass('navlinkon');
        $('.navlink').each(function(i,n){
            if($(this).attr('class').indexOf('navlinkon')>-1){
                $('.live-tab').hide();
                $('.live-tab').eq(i).show();
                $('.channels').hide();
                if($(this).attr('class').indexOf('detail')>-1){
                    $('.sayinput').hide();
                    if($('.swiper-slide').length>=2){
                        $('.channels').show();
                    }
                }else if($(this).attr('class').indexOf('say')>-1){
                    var options={
                        activityId:activityId,
                        commentId:-1,
                        count:10
                    };
                    queryComments(options,function(data){
                        if(data.data.comments.length!=0){
                            $('.live-say-ul').empty();
                            getsaylist(data.data);
                        }
                    }, function(){
                        alert('网络异常，请稍后重试！');
                    });
                    if(browser=="weichat"){
                        $('.sayinput').css('display','-webkit-box');
                        $('.sayinput').css('display','-webkit-flex');
                        $('.sayinput').css('display','flex');
                        $('.clicklike').show();
                    }
                    $('.channels').hide();
                }
            }
        })
    });
    /*判断是否是微信重定向过来的页面触发评论页面的点击事件*/
    if(get_url("code").length>0){
        $('.say').click();
    }
    /*只有微信客户端才显示点赞*/
    if(browser=="weichat"){
        $('.clicklike').show();
    }
    /*渲染通道*/
    var _imgSrc="img/live_banner_default.jpg";
    for(var i=0;i<data.lives.length;i++){
        var _slidediv=$('<div class="swiper-slide" data-num='+i+' data-hash='+data.lives[i].streams[0].url+' data-token='+data.lives[i].token+'>' +
            '<p class="swiper-slide-look"><span class="lookeye">'+data.lives[i].playTimes+'</span></p>' +
            '<p class="swiper-slide-title">'+data.lives[i].title+'</p>' +
            '<div class="swiper-slide-onlive"></div>' +
            '<img class="livecover" src='+data.lives[i].streams[0].coverUrl+'>'+
            '</div>')
        $('.swiper-wrapper').append(_slidediv);
        if(i==0){
            $('.swiper-slide').addClass('swiper-slide-on');
        }
        /*img加载失败添加默认图片*/
        $('img').bind("error",function(){
            $(this).attr("src",_imgSrc);
        })
    }
    /*如果直播通道少于两个隐藏通道*/
    if(data.lives.length<2){
        $('.channels').hide();
    }else{
        $('.swiper-slide-onlive').eq(0).show();
    }
    var mySwiper = new Swiper ('.swiper-container', {
        direction: 'horizontal',
        slidesPerView:2.3,
        freeMode:true
    });
    /*绑定点击通道切换事件*/
    $('.swiper-slide').click(function () {
        initvideo();
        var num = $(this).data('num');
        updateplaytimes(num,activityId);
        $('.swiper-slide-onlive').hide();
        $(this).addClass('swiper-slide-on').siblings().removeClass('swiper-slide-on');
        $(this).children('.swiper-slide-onlive').show();
        $('video').attr('src', $(this).data('hash'));
        $('video').data('token', $(this).data('token'));
        $('video').attr('poster', $(this).children('img').attr('src'));
        $('video').css('background-image','url('+$(this).children('img').attr('src')+')');
        if(num!=0){
            $('video')[0].play();
        };
    });
    /*触发第一个通道点击事假给video标签添加默认背景和poster*/
    $('.swiper-slide-on').click();
    /*第一次进来更新播放次数*/
    updateplaytimes(0,activityId);
}
function getsaylist(data){
    var _imgSrc="img/login_default_avatar_grey@2x.png";
    if(data.comments.length==0){
        alert('暂无更多评论');
    }
    if(data.comments.length!=0){
        for(var i=0;i<data.comments.length;i++){
            var content="";
            var nowtime=new Date();
                nowtime=nowtime.toLocaleDateString()
            var time=new Date(parseInt(data.comments[i].commentTime*1000));
            var timestring=time.toLocaleDateString();
            if(nowtime==timestring){
                timestring=time.getHours()+":"+time.getMinutes();
            }else{
                timestring=time.getMonth()+1+"/"+time.getDate();
            }
            /*判断是否是回复别人*/
            if(data.comments[i].reply){
                content="@"+data.comments[i].reply.nickname+":"+data.comments[i].content
            }else{
                content=data.comments[i].content
            }
            var _li=$('<li class="usersayli" data-hash="'+data.comments[i].commentId+'" data-type="'+data.comments[i].userType+'"><div class="usersaytitle">' +
                '<img src="'+data.comments[i].userIconUrl+'"data-id='+data.comments[i].userId+'>' +
                '<span class="username">'+data.comments[i].nickname+'</span>' +
                '<span class="usertime">'+timestring+'</span></div>' +
                '<div class="usersaycontent"><p>'+content+'</p></div>' +
                '</li>')
            $('.live-say-ul').append(_li);
        }
        /*img加载失败添加默认图片*/
        $('img').bind("error",function(){
            $(this).attr("src",_imgSrc);
        })
        /*长按事件保存用户名称和昵称*/
        var timeOutEvent=0;
        function longtouch(_this) {
            timeOutEvent = 0;
            if (browser == "weichat") {
                $('.sayinput').addClass('backsomeone');
                var userid = $(_this).data('id');
                var nikename = $(_this).parent().find(".username").html();
                var usertype = $(_this).parent().parent().data('type');
                $('.callback i').html("");
                $('.callback i').html("@" + nikename + ":");
                var _left = parseInt($('.callback').width()) + 20;
                $('.sayinput').css('display', 'flex');
                $('.sayinput').find('input').attr({'placeholder': '', 'data-hash': userid, 'data-type': usertype}).css('padding-left', _left);
            }
        }
        $(function(){
            $('.usersayli img').on({
                touchstart: function(e){
                    var _this=this;
                    timeOutEvent = setTimeout(longtouch(_this),500);
                    e.preventDefault();
                },
                touchmove: function(){
                    clearTimeout(timeOutEvent);
                    timeOutEvent = 0;
                },
                touchend: function(){
                    clearTimeout(timeOutEvent);
                    return false;
                }
            })
        });
    }else{
        $('.nosay').show();
    }
}
function getbrowse(){
    var browser = {
        versions: function () {
            var u = navigator.userAgent, app = navigator.appVersion;
            return {         //移动终端浏览器版本信息
                trident: u.indexOf('Trident') > -1, //IE内核
                presto: u.indexOf('Presto') > -1, //opera内核
                webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
                gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
                mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
                ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
                android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
                iPhone: u.indexOf('iPhone') > -1, //是否为iPhone或者QQHD浏览器
                iPad: u.indexOf('iPad') > -1, //是否iPad
                webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
            };
        }(),
        language: (navigator.browserLanguage || navigator.language).toLowerCase()
    };
    if (browser.versions.mobile) {//判断是否是移动设备打开。browser代码在下面
        var ua = navigator.userAgent.toLowerCase();//获取判断用的对象
        if (ua.match(/MicroMessenger/i) == "micromessenger") {
            return "weichat";
            //在微信中打开
        }
        if (ua.match(/WeiBo/i) == "weibo") {
            return "weibo";
            //在新浪微博客户端打开
        }
        if (ua.match(/QQ/i) == "qq") {
            return "qq";
            //在QQ空间打开
        }
        if (browser.versions.ios) {
            return "ios";
            //是否在IOS浏览器打开
        }
        if(browser.versions.android){
            return "android";
            //是否在安卓浏览器打开
        }
    } else {
        return "pc";
        //否则就是PC浏览器打开
    }
}
function get_url(name){
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    var r = window.location.search.substr(1).match(reg);  //获取url中"?"符后的字符串并正则匹配
    var context = "";
    if (r != null)
        context = r[2];
    reg = null;
    r = null;
    return context == null || context == "" || context == "undefined" ? "" : context;
}
function weixinauthorize(){
    var activityId = get_url("activityId");
    var domain = get_url('domain');
     window.location="https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxbaca640d419010ea&redirect_uri=http%3a%2f%2fdvl.lechange.cn%2fcivil%2flive%2flive.html%3factivityId%3d"+activityId+"%26domain%3d"+domain+"&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
//    window.location="https://open.weixin.qq.com/connect/oauth2/authorize?appid=wxbaca640d419010ea&redirect_uri=http%3a%2f%2ffuncresource.lechange.com%2fAPPLive%2flive.html&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
//    window.location="https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx0619bad2bcda6cfa&redirect_uri=http%3a%2f%2fimg-baby-test.lechange.cn%2fprojects%2fjicheng%2flive2.5%2flive.html%3factivityId%3d11%26domain%3ddvl.lechange.cn%26from%3dsinglemessage%26isappinstalled%3d0&response_type=code&scope=snsapi_userinfo&state=123#wechat_redirect"
}
/*更新播放次数*/
function updateplaytimes(i,activityId){
    var options = {
        activityId: activityId,
        token: $('.swiper-slide').eq(i).data('token')
    };
    if ($('.swiper-slide').eq(i).data('token')) {
        var token = $('.swiper-slide').eq(i).data('token');
        var sessionToken = sessionStorage.getItem('token' + token,token);
        if(!sessionToken){
            sessionStorage.setItem('token' + token,token);
            updatePlayTimes(options, function (data) {
                console.info($('.swiper-slide .lookeye').eq(i))
                $('.looksum .lookeye').html(data.data.playTimes);
                $('.swiper-slide .lookeye').eq(i).html(data.data.lives[i].playTimes)
                console.info(data.data.lives[i].playTimes)
            }, function () {
                alert('网络异常，请稍后重试！');
            });
        }
    }
}

