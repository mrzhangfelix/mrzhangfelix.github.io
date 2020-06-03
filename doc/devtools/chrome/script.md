# hiddenZhihu

隐藏知乎的一些广告，logo，各种导航等无用模块，其他网站也可以类似处理
```javascript
// ==UserScript==
// @name         hiddenZhihu
// @namespace    http://tampermonkey.net/
// @version      0.2
// @description  隐藏知乎无用模块
// @author       felixZhang
// @match        https://www.zhihu.com/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Your code here...
    function removeZhihu() {
        //知乎logo
        let zhihulogo = document.getElementsByClassName("ZhihuLogo");
        for(var logo of zhihulogo){
            logo.style.display='none'
        }
        //主页的侧边栏
        let GlobalSideBar = document.getElementsByClassName("GlobalSideBar");
        for(var side of GlobalSideBar){
            side.style.display='none'
        }
        //头部导航栏
        //let AppHeader = document.getElementsByClassName("AppHeader");
        //for(var head of AppHeader){
        //    head.style.display='none'
        //}
        //问题详情侧边栏
        let questionSideColumn = document.getElementsByClassName("Question-sideColumn");
        for(var sidecol of questionSideColumn){
            sidecol.style.display='none'
        }
    }
    function removeZhihuHot(){
        let HotItemImg = document.getElementsByClassName("HotItem-img");
        for(var hotimg of HotItemImg){
            hotimg.style.display='none'
        }
        let HotItemIndex = document.getElementsByClassName("HotItem-index");
        for(var hotindex of HotItemIndex){
            hotindex.style.display='none'
        }

        let HotItemMetricsBottom = document.getElementsByClassName("HotItem-metrics");
        for(var hotbottom of HotItemMetricsBottom){
            hotbottom.style.display='none'
        }
    }
    function changeTheme() {
        //知乎主题改为黑色
        let htmlzhihu = document.getElementsByTagName("HTML")[0];
        htmlzhihu.dataset.theme="dark"
    }
    function changeSize() {
        //更改阅读框大小
        let mainColumn = document.getElementsByClassName("Topstory-mainColumn")[0];
        mainColumn.style.width="800px"
    }
    function removeCard(){
        //内容图片
        let RichContentCover = document.getElementsByClassName("RichContent-cover");
        for(var cover of RichContentCover){
            cover.style.display='none'
        }
        //svg
        //let svgs = document.getElementsByTagName("svg")
        //for(var svg of svgs){
        //    svg.style.display='none'
        //}
        //广告
        let TopstoryItemAdvertCard = document.getElementsByClassName("TopstoryItem--advertCard");
        for(var card of TopstoryItemAdvertCard){
            card.style.display='none'
        }
        // 转发评论等按钮
        let ContentItemActions = document.getElementsByClassName("ContentItem-actions");
        for(var action of ContentItemActions){

           action.getElementsByClassName("VoteButton--down")[0].style.display='none'
           var Button = action.getElementsByClassName("Button")
           for(var i=1;i<Button.length;i++){
               if(Button[i].textContent=="​分享"||Button[i].textContent=="​感谢"||Button[i].innerText=="举报"){
                    Button[i].style.display='none'
               }
           }
        }
    }
    // changeTheme();
    changeSize();
    removeZhihu();
    removeZhihuHot();

    let TopstoryContent = document.getElementsByClassName("Topstory-content");
    for(var content of TopstoryContent){
        content.addEventListener('DOMNodeInserted',function () {
            removeCard();
            removeZhihuHot();
        })
    }

})();
```
