
# Mission to Mars

## Step 1 - Scraping

### NASA Mars News


```python
#Import dependencies

from bs4 import BeautifulSoup
from splinter import Browser


import pandas as pd
import time
import requests

```


```python
def init_browser():
    #Mac Splinter Setup
    #https://splinter.readthedocs.io/en/latest/drivers/chrome.html
    !which chromedriver

    executable_path = {'executable_path': '/usr/local/bin/chromedriver'}
    browser = Browser('chrome', **executable_path, headless=False)

    # Windows Splinter setup
    #executable_path = {'executable_path': 'chromedriver.exe'}
    
    return Browser("chrome", **executable_path, headless=False)
```


```python
# Initialize browser
browser = init_browser()

# URL of page to be scraped
url = 'https://mars.nasa.gov/news/'
browser.visit(url)

# Create BeautifulSoup object; parse with 'html.parser'
html = browser.html
soup = BeautifulSoup(html, 'html.parser')

# Close browser
browser.quit()

# Print formatted version of the soup
print(soup.prettify())
```

    /usr/local/bin/chromedriver
    <!DOCTYPE html>
    <html class="no-flash cookies geolocation svg picture canvas video webgl srcdoc supports hiddenscroll no-touchevents fullscreen flexbox cssanimations flexboxlegacy no-flexboxtweener csstransforms csstransforms3d csstransitions preserve3d -webkit-" lang="en" style="" xml:lang="en" xmlns="http://www.w3.org/1999/xhtml">
     <head>
      <script src="//api-public.addthis.com/url/shares.json?url=http%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;callback=_ate.cbs.rcb_2nv10" type="text/javascript">
      </script>
      <script src="//www.reddit.com/api/info.json?url=http%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;jsonp=_ate.cbs.rcb_39gd0" type="text/javascript">
      </script>
      <script src="//graph.facebook.com/?id=http%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;callback=_ate.cbs.rcb_cl4b0" type="text/javascript">
      </script>
      <script src="//api-public.addthis.com/url/shares.json?url=https%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;callback=_ate.cbs.rcb_f8qq0" type="text/javascript">
      </script>
      <script src="//www.reddit.com/api/info.json?url=https%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;jsonp=_ate.cbs.rcb_ityo0" type="text/javascript">
      </script>
      <script src="//graph.facebook.com/?id=https%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;callback=_ate.cbs.rcb_go4f0" type="text/javascript">
      </script>
      <script src="https://bam.nr-data.net/1/5e33925808?a=59562082&amp;v=1071.385e752&amp;to=JVcPR0MLWApSRU1eAQVVEhxSC1oSUlkWbBMHXwRAHhdcCUA%3D&amp;rst=3089&amp;ref=https://mars.nasa.gov/news/&amp;ap=225&amp;be=437&amp;fe=2996&amp;dc=1342&amp;af=err,xhr,stn,ins&amp;perf=%7B%22timing%22:%7B%22of%22:1534891889844,%22n%22:0,%22f%22:6,%22dn%22:6,%22dne%22:6,%22c%22:6,%22ce%22:6,%22rq%22:243,%22rp%22:292,%22rpe%22:300,%22dl%22:368,%22di%22:1341,%22ds%22:1342,%22de%22:1599,%22dc%22:2995,%22l%22:2995,%22le%22:3042%7D,%22navigation%22:%7B%7D%7D&amp;jsonp=NREUM.setToken" type="text/javascript">
      </script>
      <script src="//m.addthis.com/live/red_lojson/300lo.json?si=5b7c9774008384fe&amp;bkl=0&amp;bl=1&amp;pdt=1504&amp;sid=5b7c9774008384fe&amp;pub=ra-5a690e4c1320e328&amp;rev=v8.3.27-wp&amp;ln=en&amp;pc=men&amp;cb=0&amp;ab=-&amp;dp=mars.nasa.gov&amp;fp=news%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;fr=&amp;of=1&amp;pd=0&amp;irt=0&amp;vcl=0&amp;md=0&amp;ct=1&amp;tct=0&amp;abt=0&amp;cdn=0&amp;pi=1&amp;rb=0&amp;gen=100&amp;chr=UTF-8&amp;mk=Mars%2Cmissions%2CNASA%2Crover%2CCuriosity%2COpportunity%2CInSight%2CMars%20Reconnaissance%20Orbiter%2Cfacts&amp;colc=1534891892875&amp;jsl=1&amp;skipb=1&amp;callback=addthis.cbs.oln9_68405802543282790" type="text/javascript">
      </script>
      <script src="//m.addthisedge.com/live/boost/ra-5a690e4c1320e328/_ate.track.config_resp" type="text/javascript">
      </script>
      <meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
      <!-- Always force latest IE rendering engine or request Chrome Frame -->
      <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible"/>
      <script src="https://js-agent.newrelic.com/nr-1071.min.js">
      </script>
      <script async="" src="https://www.google-analytics.com/analytics.js">
      </script>
      <script type="text/javascript">
       window.NREUM||(NREUM={});NREUM.info={"beacon":"bam.nr-data.net","errorBeacon":"bam.nr-data.net","licenseKey":"5e33925808","applicationID":"59562082","transactionName":"JVcPR0MLWApSRU1eAQVVEhxSC1oSUlkWbBMHXwRAHhdcCUA=","queueTime":0,"applicationTime":225,"agent":""}
      </script>
      <script type="text/javascript">
       (window.NREUM||(NREUM={})).loader_config={xpid:"VQcPUlZTDxAFXVRUBQEPVA=="};window.NREUM||(NREUM={}),__nr_require=function(t,n,e){function r(e){if(!n[e]){var o=n[e]={exports:{}};t[e][0].call(o.exports,function(n){var o=t[e][1][n];return r(o||n)},o,o.exports)}return n[e].exports}if("function"==typeof __nr_require)return __nr_require;for(var o=0;o&lt;e.length;o++)r(e[o]);return r}({1:[function(t,n,e){function r(t){try{s.console&amp;&amp;console.log(t)}catch(n){}}var o,i=t("ee"),a=t(15),s={};try{o=localStorage.getItem("__nr_flags").split(","),console&amp;&amp;"function"==typeof console.log&amp;&amp;(s.console=!0,o.indexOf("dev")!==-1&amp;&amp;(s.dev=!0),o.indexOf("nr_dev")!==-1&amp;&amp;(s.nrDev=!0))}catch(c){}s.nrDev&amp;&amp;i.on("internal-error",function(t){r(t.stack)}),s.dev&amp;&amp;i.on("fn-err",function(t,n,e){r(e.stack)}),s.dev&amp;&amp;(r("NR AGENT IN DEVELOPMENT MODE"),r("flags: "+a(s,function(t,n){return t}).join(", ")))},{}],2:[function(t,n,e){function r(t,n,e,r,s){try{p?p-=1:o(s||new UncaughtException(t,n,e),!0)}catch(f){try{i("ierr",[f,c.now(),!0])}catch(d){}}return"function"==typeof u&amp;&amp;u.apply(this,a(arguments))}function UncaughtException(t,n,e){this.message=t||"Uncaught error with no additional information",this.sourceURL=n,this.line=e}function o(t,n){var e=n?null:c.now();i("err",[t,e])}var i=t("handle"),a=t(16),s=t("ee"),c=t("loader"),f=t("gos"),u=window.onerror,d=!1,l="nr@seenError",p=0;c.features.err=!0,t(1),window.onerror=r;try{throw new Error}catch(h){"stack"in h&amp;&amp;(t(8),t(7),"addEventListener"in window&amp;&amp;t(5),c.xhrWrappable&amp;&amp;t(9),d=!0)}s.on("fn-start",function(t,n,e){d&amp;&amp;(p+=1)}),s.on("fn-err",function(t,n,e){d&amp;&amp;!e[l]&amp;&amp;(f(e,l,function(){return!0}),this.thrown=!0,o(e))}),s.on("fn-end",function(){d&amp;&amp;!this.thrown&amp;&amp;p&gt;0&amp;&amp;(p-=1)}),s.on("internal-error",function(t){i("ierr",[t,c.now(),!0])})},{}],3:[function(t,n,e){t("loader").features.ins=!0},{}],4:[function(t,n,e){function r(t){}if(window.performance&amp;&amp;window.performance.timing&amp;&amp;window.performance.getEntriesByType){var o=t("ee"),i=t("handle"),a=t(8),s=t(7),c="learResourceTimings",f="addEventListener",u="resourcetimingbufferfull",d="bstResource",l="resource",p="-start",h="-end",m="fn"+p,w="fn"+h,v="bstTimer",y="pushState",g=t("loader");g.features.stn=!0,t(6);var b=NREUM.o.EV;o.on(m,function(t,n){var e=t[0];e instanceof b&amp;&amp;(this.bstStart=g.now())}),o.on(w,function(t,n){var e=t[0];e instanceof b&amp;&amp;i("bst",[e,n,this.bstStart,g.now()])}),a.on(m,function(t,n,e){this.bstStart=g.now(),this.bstType=e}),a.on(w,function(t,n){i(v,[n,this.bstStart,g.now(),this.bstType])}),s.on(m,function(){this.bstStart=g.now()}),s.on(w,function(t,n){i(v,[n,this.bstStart,g.now(),"requestAnimationFrame"])}),o.on(y+p,function(t){this.time=g.now(),this.startPath=location.pathname+location.hash}),o.on(y+h,function(t){i("bstHist",[location.pathname+location.hash,this.startPath,this.time])}),f in window.performance&amp;&amp;(window.performance["c"+c]?window.performance[f](u,function(t){i(d,[window.performance.getEntriesByType(l)]),window.performance["c"+c]()},!1):window.performance[f]("webkit"+u,function(t){i(d,[window.performance.getEntriesByType(l)]),window.performance["webkitC"+c]()},!1)),document[f]("scroll",r,{passive:!0}),document[f]("keypress",r,!1),document[f]("click",r,!1)}},{}],5:[function(t,n,e){function r(t){for(var n=t;n&amp;&amp;!n.hasOwnProperty(u);)n=Object.getPrototypeOf(n);n&amp;&amp;o(n)}function o(t){s.inPlace(t,[u,d],"-",i)}function i(t,n){return t[1]}var a=t("ee").get("events"),s=t(18)(a,!0),c=t("gos"),f=XMLHttpRequest,u="addEventListener",d="removeEventListener";n.exports=a,"getPrototypeOf"in Object?(r(document),r(window),r(f.prototype)):f.prototype.hasOwnProperty(u)&amp;&amp;(o(window),o(f.prototype)),a.on(u+"-start",function(t,n){var e=t[1],r=c(e,"nr@wrapped",function(){function t(){if("function"==typeof e.handleEvent)return e.handleEvent.apply(e,arguments)}var n={object:t,"function":e}[typeof e];return n?s(n,"fn-",null,n.name||"anonymous"):e});this.wrapped=t[1]=r}),a.on(d+"-start",function(t){t[1]=this.wrapped||t[1]})},{}],6:[function(t,n,e){var r=t("ee").get("history"),o=t(18)(r);n.exports=r,o.inPlace(window.history,["pushState","replaceState"],"-")},{}],7:[function(t,n,e){var r=t("ee").get("raf"),o=t(18)(r),i="equestAnimationFrame";n.exports=r,o.inPlace(window,["r"+i,"mozR"+i,"webkitR"+i,"msR"+i],"raf-"),r.on("raf-start",function(t){t[0]=o(t[0],"fn-")})},{}],8:[function(t,n,e){function r(t,n,e){t[0]=a(t[0],"fn-",null,e)}function o(t,n,e){this.method=e,this.timerDuration=isNaN(t[1])?0:+t[1],t[0]=a(t[0],"fn-",this,e)}var i=t("ee").get("timer"),a=t(18)(i),s="setTimeout",c="setInterval",f="clearTimeout",u="-start",d="-";n.exports=i,a.inPlace(window,[s,"setImmediate"],s+d),a.inPlace(window,[c],c+d),a.inPlace(window,[f,"clearImmediate"],f+d),i.on(c+u,r),i.on(s+u,o)},{}],9:[function(t,n,e){function r(t,n){d.inPlace(n,["onreadystatechange"],"fn-",s)}function o(){var t=this,n=u.context(t);t.readyState&gt;3&amp;&amp;!n.resolved&amp;&amp;(n.resolved=!0,u.emit("xhr-resolved",[],t)),d.inPlace(t,y,"fn-",s)}function i(t){g.push(t),h&amp;&amp;(x?x.then(a):w?w(a):(E=-E,O.data=E))}function a(){for(var t=0;t&lt;g.length;t++)r([],g[t]);g.length&amp;&amp;(g=[])}function s(t,n){return n}function c(t,n){for(var e in t)n[e]=t[e];return n}t(5);var f=t("ee"),u=f.get("xhr"),d=t(18)(u),l=NREUM.o,p=l.XHR,h=l.MO,m=l.PR,w=l.SI,v="readystatechange",y=["onload","onerror","onabort","onloadstart","onloadend","onprogress","ontimeout"],g=[];n.exports=u;var b=window.XMLHttpRequest=function(t){var n=new p(t);try{u.emit("new-xhr",[n],n),n.addEventListener(v,o,!1)}catch(e){try{u.emit("internal-error",[e])}catch(r){}}return n};if(c(p,b),b.prototype=p.prototype,d.inPlace(b.prototype,["open","send"],"-xhr-",s),u.on("send-xhr-start",function(t,n){r(t,n),i(n)}),u.on("open-xhr-start",r),h){var x=m&amp;&amp;m.resolve();if(!w&amp;&amp;!m){var E=1,O=document.createTextNode(E);new h(a).observe(O,{characterData:!0})}}else f.on("fn-end",function(t){t[0]&amp;&amp;t[0].type===v||a()})},{}],10:[function(t,n,e){function r(t){var n=this.params,e=this.metrics;if(!this.ended){this.ended=!0;for(var r=0;r&lt;d;r++)t.removeEventListener(u[r],this.listener,!1);if(!n.aborted){if(e.duration=a.now()-this.startTime,4===t.readyState){n.status=t.status;var i=o(t,this.lastSize);if(i&amp;&amp;(e.rxSize=i),this.sameOrigin){var c=t.getResponseHeader("X-NewRelic-App-Data");c&amp;&amp;(n.cat=c.split(", ").pop())}}else n.status=0;e.cbTime=this.cbTime,f.emit("xhr-done",[t],t),s("xhr",[n,e,this.startTime])}}}function o(t,n){var e=t.responseType;if("json"===e&amp;&amp;null!==n)return n;var r="arraybuffer"===e||"blob"===e||"json"===e?t.response:t.responseText;return h(r)}function i(t,n){var e=c(n),r=t.params;r.host=e.hostname+":"+e.port,r.pathname=e.pathname,t.sameOrigin=e.sameOrigin}var a=t("loader");if(a.xhrWrappable){var s=t("handle"),c=t(11),f=t("ee"),u=["load","error","abort","timeout"],d=u.length,l=t("id"),p=t(14),h=t(13),m=window.XMLHttpRequest;a.features.xhr=!0,t(9),f.on("new-xhr",function(t){var n=this;n.totalCbs=0,n.called=0,n.cbTime=0,n.end=r,n.ended=!1,n.xhrGuids={},n.lastSize=null,p&amp;&amp;(p&gt;34||p&lt;10)||window.opera||t.addEventListener("progress",function(t){n.lastSize=t.loaded},!1)}),f.on("open-xhr-start",function(t){this.params={method:t[0]},i(this,t[1]),this.metrics={}}),f.on("open-xhr-end",function(t,n){"loader_config"in NREUM&amp;&amp;"xpid"in NREUM.loader_config&amp;&amp;this.sameOrigin&amp;&amp;n.setRequestHeader("X-NewRelic-ID",NREUM.loader_config.xpid)}),f.on("send-xhr-start",function(t,n){var e=this.metrics,r=t[0],o=this;if(e&amp;&amp;r){var i=h(r);i&amp;&amp;(e.txSize=i)}this.startTime=a.now(),this.listener=function(t){try{"abort"===t.type&amp;&amp;(o.params.aborted=!0),("load"!==t.type||o.called===o.totalCbs&amp;&amp;(o.onloadCalled||"function"!=typeof n.onload))&amp;&amp;o.end(n)}catch(e){try{f.emit("internal-error",[e])}catch(r){}}};for(var s=0;s&lt;d;s++)n.addEventListener(u[s],this.listener,!1)}),f.on("xhr-cb-time",function(t,n,e){this.cbTime+=t,n?this.onloadCalled=!0:this.called+=1,this.called!==this.totalCbs||!this.onloadCalled&amp;&amp;"function"==typeof e.onload||this.end(e)}),f.on("xhr-load-added",function(t,n){var e=""+l(t)+!!n;this.xhrGuids&amp;&amp;!this.xhrGuids[e]&amp;&amp;(this.xhrGuids[e]=!0,this.totalCbs+=1)}),f.on("xhr-load-removed",function(t,n){var e=""+l(t)+!!n;this.xhrGuids&amp;&amp;this.xhrGuids[e]&amp;&amp;(delete this.xhrGuids[e],this.totalCbs-=1)}),f.on("addEventListener-end",function(t,n){n instanceof m&amp;&amp;"load"===t[0]&amp;&amp;f.emit("xhr-load-added",[t[1],t[2]],n)}),f.on("removeEventListener-end",function(t,n){n instanceof m&amp;&amp;"load"===t[0]&amp;&amp;f.emit("xhr-load-removed",[t[1],t[2]],n)}),f.on("fn-start",function(t,n,e){n instanceof m&amp;&amp;("onload"===e&amp;&amp;(this.onload=!0),("load"===(t[0]&amp;&amp;t[0].type)||this.onload)&amp;&amp;(this.xhrCbStart=a.now()))}),f.on("fn-end",function(t,n){this.xhrCbStart&amp;&amp;f.emit("xhr-cb-time",[a.now()-this.xhrCbStart,this.onload,n],n)})}},{}],11:[function(t,n,e){n.exports=function(t){var n=document.createElement("a"),e=window.location,r={};n.href=t,r.port=n.port;var o=n.href.split("://");!r.port&amp;&amp;o[1]&amp;&amp;(r.port=o[1].split("/")[0].split("@").pop().split(":")[1]),r.port&amp;&amp;"0"!==r.port||(r.port="https"===o[0]?"443":"80"),r.hostname=n.hostname||e.hostname,r.pathname=n.pathname,r.protocol=o[0],"/"!==r.pathname.charAt(0)&amp;&amp;(r.pathname="/"+r.pathname);var i=!n.protocol||":"===n.protocol||n.protocol===e.protocol,a=n.hostname===document.domain&amp;&amp;n.port===e.port;return r.sameOrigin=i&amp;&amp;(!n.hostname||a),r}},{}],12:[function(t,n,e){function r(){}function o(t,n,e){return function(){return i(t,[f.now()].concat(s(arguments)),n?null:this,e),n?void 0:this}}var i=t("handle"),a=t(15),s=t(16),c=t("ee").get("tracer"),f=t("loader"),u=NREUM;"undefined"==typeof window.newrelic&amp;&amp;(newrelic=u);var d=["setPageViewName","setCustomAttribute","setErrorHandler","finished","addToTrace","inlineHit","addRelease"],l="api-",p=l+"ixn-";a(d,function(t,n){u[n]=o(l+n,!0,"api")}),u.addPageAction=o(l+"addPageAction",!0),u.setCurrentRouteName=o(l+"routeName",!0),n.exports=newrelic,u.interaction=function(){return(new r).get()};var h=r.prototype={createTracer:function(t,n){var e={},r=this,o="function"==typeof n;return i(p+"tracer",[f.now(),t,e],r),function(){if(c.emit((o?"":"no-")+"fn-start",[f.now(),r,o],e),o)try{return n.apply(this,arguments)}catch(t){throw c.emit("fn-err",[arguments,this,t],e),t}finally{c.emit("fn-end",[f.now()],e)}}}};a("setName,setAttribute,save,ignore,onEnd,getContext,end,get".split(","),function(t,n){h[n]=o(p+n)}),newrelic.noticeError=function(t){"string"==typeof t&amp;&amp;(t=new Error(t)),i("err",[t,f.now()])}},{}],13:[function(t,n,e){n.exports=function(t){if("string"==typeof t&amp;&amp;t.length)return t.length;if("object"==typeof t){if("undefined"!=typeof ArrayBuffer&amp;&amp;t instanceof ArrayBuffer&amp;&amp;t.byteLength)return t.byteLength;if("undefined"!=typeof Blob&amp;&amp;t instanceof Blob&amp;&amp;t.size)return t.size;if(!("undefined"!=typeof FormData&amp;&amp;t instanceof FormData))try{return JSON.stringify(t).length}catch(n){return}}}},{}],14:[function(t,n,e){var r=0,o=navigator.userAgent.match(/Firefox[\/\s](\d+\.\d+)/);o&amp;&amp;(r=+o[1]),n.exports=r},{}],15:[function(t,n,e){function r(t,n){var e=[],r="",i=0;for(r in t)o.call(t,r)&amp;&amp;(e[i]=n(r,t[r]),i+=1);return e}var o=Object.prototype.hasOwnProperty;n.exports=r},{}],16:[function(t,n,e){function r(t,n,e){n||(n=0),"undefined"==typeof e&amp;&amp;(e=t?t.length:0);for(var r=-1,o=e-n||0,i=Array(o&lt;0?0:o);++r&lt;o;)i[r]=t[n+r];return i}n.exports=r},{}],17:[function(t,n,e){n.exports={exists:"undefined"!=typeof window.performance&amp;&amp;window.performance.timing&amp;&amp;"undefined"!=typeof window.performance.timing.navigationStart}},{}],18:[function(t,n,e){function r(t){return!(t&amp;&amp;t instanceof Function&amp;&amp;t.apply&amp;&amp;!t[a])}var o=t("ee"),i=t(16),a="nr@original",s=Object.prototype.hasOwnProperty,c=!1;n.exports=function(t,n){function e(t,n,e,o){function nrWrapper(){var r,a,s,c;try{a=this,r=i(arguments),s="function"==typeof e?e(r,a):e||{}}catch(f){l([f,"",[r,a,o],s])}u(n+"start",[r,a,o],s);try{return c=t.apply(a,r)}catch(d){throw u(n+"err",[r,a,d],s),d}finally{u(n+"end",[r,a,c],s)}}return r(t)?t:(n||(n=""),nrWrapper[a]=t,d(t,nrWrapper),nrWrapper)}function f(t,n,o,i){o||(o="");var a,s,c,f="-"===o.charAt(0);for(c=0;c&lt;n.length;c++)s=n[c],a=t[s],r(a)||(t[s]=e(a,f?s+o:o,i,s))}function u(e,r,o){if(!c||n){var i=c;c=!0;try{t.emit(e,r,o,n)}catch(a){l([a,e,r,o])}c=i}}function d(t,n){if(Object.defineProperty&amp;&amp;Object.keys)try{var e=Object.keys(t);return e.forEach(function(e){Object.defineProperty(n,e,{get:function(){return t[e]},set:function(n){return t[e]=n,n}})}),n}catch(r){l([r])}for(var o in t)s.call(t,o)&amp;&amp;(n[o]=t[o]);return n}function l(n){try{t.emit("internal-error",n)}catch(e){}}return t||(t=o),e.inPlace=f,e.flag=a,e}},{}],ee:[function(t,n,e){function r(){}function o(t){function n(t){return t&amp;&amp;t instanceof r?t:t?c(t,s,i):i()}function e(e,r,o,i){if(!l.aborted||i){t&amp;&amp;t(e,r,o);for(var a=n(o),s=h(e),c=s.length,f=0;f&lt;c;f++)s[f].apply(a,r);var d=u[y[e]];return d&amp;&amp;d.push([g,e,r,a]),a}}function p(t,n){v[t]=h(t).concat(n)}function h(t){return v[t]||[]}function m(t){return d[t]=d[t]||o(e)}function w(t,n){f(t,function(t,e){n=n||"feature",y[e]=n,n in u||(u[n]=[])})}var v={},y={},g={on:p,emit:e,get:m,listeners:h,context:n,buffer:w,abort:a,aborted:!1};return g}function i(){return new r}function a(){(u.api||u.feature)&amp;&amp;(l.aborted=!0,u=l.backlog={})}var s="nr@context",c=t("gos"),f=t(15),u={},d={},l=n.exports=o();l.backlog=u},{}],gos:[function(t,n,e){function r(t,n,e){if(o.call(t,n))return t[n];var r=e();if(Object.defineProperty&amp;&amp;Object.keys)try{return Object.defineProperty(t,n,{value:r,writable:!0,enumerable:!1}),r}catch(i){}return t[n]=r,r}var o=Object.prototype.hasOwnProperty;n.exports=r},{}],handle:[function(t,n,e){function r(t,n,e,r){o.buffer([t],r),o.emit(t,n,e)}var o=t("ee").get("handle");n.exports=r,r.ee=o},{}],id:[function(t,n,e){function r(t){var n=typeof t;return!t||"object"!==n&amp;&amp;"function"!==n?-1:t===window?0:a(t,i,function(){return o++})}var o=1,i="nr@id",a=t("gos");n.exports=r},{}],loader:[function(t,n,e){function r(){if(!x++){var t=b.info=NREUM.info,n=l.getElementsByTagName("script")[0];if(setTimeout(u.abort,3e4),!(t&amp;&amp;t.licenseKey&amp;&amp;t.applicationID&amp;&amp;n))return u.abort();f(y,function(n,e){t[n]||(t[n]=e)}),c("mark",["onload",a()+b.offset],null,"api");var e=l.createElement("script");e.src="https://"+t.agent,n.parentNode.insertBefore(e,n)}}function o(){"complete"===l.readyState&amp;&amp;i()}function i(){c("mark",["domContent",a()+b.offset],null,"api")}function a(){return E.exists&amp;&amp;performance.now?Math.round(performance.now()):(s=Math.max((new Date).getTime(),s))-b.offset}var s=(new Date).getTime(),c=t("handle"),f=t(15),u=t("ee"),d=window,l=d.document,p="addEventListener",h="attachEvent",m=d.XMLHttpRequest,w=m&amp;&amp;m.prototype;NREUM.o={ST:setTimeout,SI:d.setImmediate,CT:clearTimeout,XHR:m,REQ:d.Request,EV:d.Event,PR:d.Promise,MO:d.MutationObserver};var v=""+location,y={beacon:"bam.nr-data.net",errorBeacon:"bam.nr-data.net",agent:"js-agent.newrelic.com/nr-1071.min.js"},g=m&amp;&amp;w&amp;&amp;w[p]&amp;&amp;!/CriOS/.test(navigator.userAgent),b=n.exports={offset:s,now:a,origin:v,features:{},xhrWrappable:g};t(12),l[p]?(l[p]("DOMContentLoaded",i,!1),d[p]("load",r,!1)):(l[h]("onreadystatechange",o),d[h]("onload",r)),c("mark",["firstbyte",s],null,"api");var x=0,E=t(17)},{}]},{},["loader",2,10,4,3]);
      </script>
      <!-- Responsiveness -->
      <meta content="width=device-width, initial-scale=1.0" name="viewport"/>
      <!-- Favicon -->
      <link href="/apple-touch-icon.png" rel="apple-touch-icon" sizes="180x180"/>
      <link href="/favicon-32x32.png" rel="icon" sizes="32x32" type="image/png"/>
      <link href="/favicon-16x16.png" rel="icon" sizes="16x16" type="image/png"/>
      <link href="/manifest.json" rel="manifest"/>
      <link color="#e48b55" href="/safari-pinned-tab.svg" rel="mask-icon"/>
      <meta content="#000000" name="theme-color"/>
      <meta content="authenticity_token" name="csrf-param"/>
      <meta content="DW98Qk+GFAVR/uJS5QRO0DDTnDDEQv55m+iwfPx1dmU=" name="csrf-token"/>
      <title>
       News  – NASA’s Mars Exploration Program
      </title>
      <meta content="NASA’s Mars Exploration Program " property="og:site_name"/>
      <meta content="mars.nasa.gov" name="author"/>
      <meta content="Mars, missions, NASA, rover, Curiosity, Opportunity, InSight, Mars Reconnaissance Orbiter, facts" name="keywords"/>
      <meta content="NASA’s real-time portal for Mars exploration, featuring the latest news, images, and discoveries from the Red Planet." name="description"/>
      <meta content="NASA’s real-time portal for Mars exploration, featuring the latest news, images, and discoveries from the Red Planet." property="og:description"/>
      <meta content="News  – NASA’s Mars Exploration Program " property="og:title"/>
      <meta content="https://mars.nasa.gov/news" property="og:url"/>
      <meta content="article" property="og:type"/>
      <meta content="2017-09-22 19:53:22 UTC" property="og:updated_time"/>
      <meta content="https://mars.nasa.gov/system/site_config_values/meta_share_images/1_142497main_PIA03154-200.jpg" property="og:image"/>
      <meta content="https://mars.nasa.gov/system/site_config_values/meta_share_images/1_142497main_PIA03154-200.jpg" name="twitter:image"/>
      <link href="https://mars.nasa.gov/system/site_config_values/meta_share_images/1_142497main_PIA03154-200.jpg" rel="image_src"/>
      <meta content="195570401081308" property="fb:app_id"/>
      <style data-href="https://fonts.googleapis.com/css?family=Montserrat:200,300,400,500,600,700|Raleway:300,400" media="">
       /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 200;
      src: local('Montserrat ExtraLight'), local('Montserrat-ExtraLight'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_aZA3gTD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 200;
      src: local('Montserrat ExtraLight'), local('Montserrat-ExtraLight'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_aZA3g3D_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 200;
      src: local('Montserrat ExtraLight'), local('Montserrat-ExtraLight'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_aZA3gbD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 200;
      src: local('Montserrat ExtraLight'), local('Montserrat-ExtraLight'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_aZA3gfD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 200;
      src: local('Montserrat ExtraLight'), local('Montserrat-ExtraLight'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_aZA3gnD_vx3rCs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 300;
      src: local('Montserrat Light'), local('Montserrat-Light'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_cJD3gTD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 300;
      src: local('Montserrat Light'), local('Montserrat-Light'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_cJD3g3D_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 300;
      src: local('Montserrat Light'), local('Montserrat-Light'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_cJD3gbD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 300;
      src: local('Montserrat Light'), local('Montserrat-Light'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_cJD3gfD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 300;
      src: local('Montserrat Light'), local('Montserrat-Light'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_cJD3gnD_vx3rCs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 400;
      src: local('Montserrat Regular'), local('Montserrat-Regular'), url(https://fonts.gstatic.com/s/montserrat/v12/JTUSjIg1_i6t8kCHKm459WRhyyTh89ZNpQ.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 400;
      src: local('Montserrat Regular'), local('Montserrat-Regular'), url(https://fonts.gstatic.com/s/montserrat/v12/JTUSjIg1_i6t8kCHKm459W1hyyTh89ZNpQ.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 400;
      src: local('Montserrat Regular'), local('Montserrat-Regular'), url(https://fonts.gstatic.com/s/montserrat/v12/JTUSjIg1_i6t8kCHKm459WZhyyTh89ZNpQ.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 400;
      src: local('Montserrat Regular'), local('Montserrat-Regular'), url(https://fonts.gstatic.com/s/montserrat/v12/JTUSjIg1_i6t8kCHKm459WdhyyTh89ZNpQ.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 400;
      src: local('Montserrat Regular'), local('Montserrat-Regular'), url(https://fonts.gstatic.com/s/montserrat/v12/JTUSjIg1_i6t8kCHKm459WlhyyTh89Y.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      src: local('Montserrat Medium'), local('Montserrat-Medium'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_ZpC3gTD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      src: local('Montserrat Medium'), local('Montserrat-Medium'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_ZpC3g3D_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      src: local('Montserrat Medium'), local('Montserrat-Medium'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_ZpC3gbD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      src: local('Montserrat Medium'), local('Montserrat-Medium'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_ZpC3gfD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      src: local('Montserrat Medium'), local('Montserrat-Medium'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_ZpC3gnD_vx3rCs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 600;
      src: local('Montserrat SemiBold'), local('Montserrat-SemiBold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_bZF3gTD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 600;
      src: local('Montserrat SemiBold'), local('Montserrat-SemiBold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_bZF3g3D_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 600;
      src: local('Montserrat SemiBold'), local('Montserrat-SemiBold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_bZF3gbD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 600;
      src: local('Montserrat SemiBold'), local('Montserrat-SemiBold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_bZF3gfD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 600;
      src: local('Montserrat SemiBold'), local('Montserrat-SemiBold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_bZF3gnD_vx3rCs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* cyrillic-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 700;
      src: local('Montserrat Bold'), local('Montserrat-Bold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_dJE3gTD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0460-052F, U+1C80-1C88, U+20B4, U+2DE0-2DFF, U+A640-A69F, U+FE2E-FE2F;
    }
    /* cyrillic */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 700;
      src: local('Montserrat Bold'), local('Montserrat-Bold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_dJE3g3D_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0400-045F, U+0490-0491, U+04B0-04B1, U+2116;
    }
    /* vietnamese */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 700;
      src: local('Montserrat Bold'), local('Montserrat-Bold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_dJE3gbD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0102-0103, U+0110-0111, U+1EA0-1EF9, U+20AB;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 700;
      src: local('Montserrat Bold'), local('Montserrat-Bold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_dJE3gfD_vx3rCubqg.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 700;
      src: local('Montserrat Bold'), local('Montserrat-Bold'), url(https://fonts.gstatic.com/s/montserrat/v12/JTURjIg1_i6t8kCHKm45_dJE3gnD_vx3rCs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Raleway';
      font-style: normal;
      font-weight: 300;
      src: local('Raleway Light'), local('Raleway-Light'), url(https://fonts.gstatic.com/s/raleway/v12/1Ptrg8zYS_SKggPNwIYqWqhPANqczVsq4A.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Raleway';
      font-style: normal;
      font-weight: 300;
      src: local('Raleway Light'), local('Raleway-Light'), url(https://fonts.gstatic.com/s/raleway/v12/1Ptrg8zYS_SKggPNwIYqWqZPANqczVs.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
    /* latin-ext */
    @font-face {
      font-family: 'Raleway';
      font-style: normal;
      font-weight: 400;
      src: local('Raleway'), local('Raleway-Regular'), url(https://fonts.gstatic.com/s/raleway/v12/1Ptug8zYS_SKggPNyCMIT4ttDfCmxA.woff2) format('woff2');
      unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB, U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
    }
    /* latin */
    @font-face {
      font-family: 'Raleway';
      font-style: normal;
      font-weight: 400;
      src: local('Raleway'), local('Raleway-Regular'), url(https://fonts.gstatic.com/s/raleway/v12/1Ptug8zYS_SKggPNyC0IT4ttDfA.woff2) format('woff2');
      unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
    }
      </style>
      <style data-href="/assets/public_manifest-4cbc4dac695ce3cc362575f04bfbe065ab2d7ae583807e7a2c5b4e0385ccd5c2.css" media="all">
       @charset "UTF-8";.ui-helper-hidden{display:none}.ui-helper-hidden-accessible{position:absolute !important;clip:rect(1px 1px 1px 1px);clip:rect(1px, 1px, 1px, 1px)}.ui-helper-reset{margin:0;padding:0;border:0;outline:0;line-height:1.3;text-decoration:none;font-size:100%;list-style:none}.ui-helper-clearfix:before,.ui-helper-clearfix:after{content:"";display:table}.ui-helper-clearfix:after{clear:both}.ui-helper-clearfix{zoom:1}.ui-helper-zfix{width:100%;height:100%;top:0;left:0;position:absolute;opacity:0;filter:Alpha(Opacity=0)}.ui-state-disabled{cursor:default !important}.ui-icon{display:block;text-indent:-99999px;overflow:hidden;background-repeat:no-repeat}.ui-widget-overlay{position:absolute;top:0;left:0;width:100%;height:100%}.ui-slider{position:relative;text-align:left}.ui-slider .ui-slider-handle{position:absolute;z-index:2;width:6.2em;height:.7em;cursor:default}.ui-slider .ui-slider-range{position:absolute;z-index:1;font-size:.7em;display:block;border:0;background-position:0 0}.ui-slider-horizontal{height:.8em}.ui-slider-horizontal .ui-slider-handle{top:0;margin-left:0}.ui-slider-horizontal .ui-slider-range{top:0;height:100%}.ui-slider-horizontal .ui-slider-range-min{left:0}.ui-slider-horizontal .ui-slider-range-max{right:0}.ui-slider-vertical{width:.8em;height:100px}.ui-slider-vertical .ui-slider-handle{left:-.3em;margin-left:0;margin-bottom:-.6em}.ui-slider-vertical .ui-slider-range{left:0;width:100%}.ui-slider-vertical .ui-slider-range-min{bottom:0}.ui-slider-vertical .ui-slider-range-max{top:0}.ui-widget{font-family:Segoe UI,Arial,sans-serif;font-size:1.1em}.ui-widget .ui-widget{font-size:1em}.ui-widget input,.ui-widget select,.ui-widget textarea,.ui-widget button{font-family:Segoe UI,Arial,sans-serif;font-size:1em}.ui-widget-content{border:1px solid #666666;background:#000000;color:#ffffff}.ui-widget-content a{color:#ffffff}.ui-widget-header{border:1px solid #333333;background:#333333;color:#ffffff;font-weight:700}.ui-widget-header a{color:#ffffff}.ui-state-default,.ui-widget-content .ui-state-default,.ui-widget-header .ui-state-default{border:1px solid #666666;background:#555555;font-weight:700;color:#eeeeee}.ui-state-default a,.ui-state-default a:link,.ui-state-default a:visited{color:#eeeeee;text-decoration:none}.ui-state-hover a,.ui-state-hover a:hover{color:#ffffff;text-decoration:none}.ui-state-active,.ui-widget-content .ui-state-active,.ui-widget-header .ui-state-active{border:1px solid #ffaf0f;background:#f58400;font-weight:700;color:#ffffff}.ui-state-active a,.ui-state-active a:link,.ui-state-active a:visited{color:#ffffff;text-decoration:none}.ui-state-highlight,.ui-widget-content .ui-state-highlight,.ui-widget-header .ui-state-highlight{border:1px solid #cccccc;background:#eeeeee;color:#2e7db2}.ui-state-highlight a,.ui-widget-content .ui-state-highlight a,.ui-widget-header .ui-state-highlight a{color:#2e7db2}.ui-state-error,.ui-widget-content .ui-state-error,.ui-widget-header .ui-state-error{border:1px solid #ffb73d;background:#ffc73d;color:#111111}.ui-state-error a,.ui-widget-content .ui-state-error a,.ui-widget-header .ui-state-error a{color:#111111}.ui-state-error-text,.ui-widget-content .ui-state-error-text,.ui-widget-header .ui-state-error-text{color:#111111}.ui-priority-primary,.ui-widget-content .ui-priority-primary,.ui-widget-header .ui-priority-primary{font-weight:700}.ui-priority-secondary,.ui-widget-content .ui-priority-secondary,.ui-widget-header .ui-priority-secondary{opacity:.7;filter:Alpha(Opacity=70);font-weight:400}.ui-state-disabled,.ui-widget-content .ui-state-disabled,.ui-widget-header .ui-state-disabled{opacity:.35;filter:Alpha(Opacity=35);background-image:none}.ui-icon{width:16px;height:16px}.ui-icon-carat-1-n{background-position:0 0}.ui-icon-carat-1-ne{background-position:-16px 0}.ui-icon-carat-1-e{background-position:-32px 0}.ui-icon-carat-1-se{background-position:-48px 0}.ui-icon-carat-1-s{background-position:-64px 0}.ui-icon-carat-1-sw{background-position:-80px 0}.ui-icon-carat-1-w{background-position:-96px 0}.ui-icon-carat-1-nw{background-position:-112px 0}.ui-icon-carat-2-n-s{background-position:-128px 0}.ui-icon-carat-2-e-w{background-position:-144px 0}.ui-icon-triangle-1-n{background-position:0 -16px}.ui-icon-triangle-1-ne{background-position:-16px -16px}.ui-icon-triangle-1-e{background-position:-32px -16px}.ui-icon-triangle-1-se{background-position:-48px -16px}.ui-icon-triangle-1-s{background-position:-64px -16px}.ui-icon-triangle-1-sw{background-position:-80px -16px}.ui-icon-triangle-1-w{background-position:-96px -16px}.ui-icon-triangle-1-nw{background-position:-112px -16px}.ui-icon-triangle-2-n-s{background-position:-128px -16px}.ui-icon-triangle-2-e-w{background-position:-144px -16px}.ui-icon-arrow-1-n{background-position:0 -32px}.ui-icon-arrow-1-ne{background-position:-16px -32px}.ui-icon-arrow-1-e{background-position:-32px -32px}.ui-icon-arrow-1-se{background-position:-48px -32px}.ui-icon-arrow-1-s{background-position:-64px -32px}.ui-icon-arrow-1-sw{background-position:-80px -32px}.ui-icon-arrow-1-w{background-position:-96px -32px}.ui-icon-arrow-1-nw{background-position:-112px -32px}.ui-icon-arrow-2-n-s{background-position:-128px -32px}.ui-icon-arrow-2-ne-sw{background-position:-144px -32px}.ui-icon-arrow-2-e-w{background-position:-160px -32px}.ui-icon-arrow-2-se-nw{background-position:-176px -32px}.ui-icon-arrowstop-1-n{background-position:-192px -32px}.ui-icon-arrowstop-1-e{background-position:-208px -32px}.ui-icon-arrowstop-1-s{background-position:-224px -32px}.ui-icon-arrowstop-1-w{background-position:-240px -32px}.ui-icon-arrowthick-1-n{background-position:0 -48px}.ui-icon-arrowthick-1-ne{background-position:-16px -48px}.ui-icon-arrowthick-1-e{background-position:-32px -48px}.ui-icon-arrowthick-1-se{background-position:-48px -48px}.ui-icon-arrowthick-1-s{background-position:-64px -48px}.ui-icon-arrowthick-1-sw{background-position:-80px -48px}.ui-icon-arrowthick-1-w{background-position:-96px -48px}.ui-icon-arrowthick-1-nw{background-position:-112px -48px}.ui-icon-arrowthick-2-n-s{background-position:-128px -48px}.ui-icon-arrowthick-2-ne-sw{background-position:-144px -48px}.ui-icon-arrowthick-2-e-w{background-position:-160px -48px}.ui-icon-arrowthick-2-se-nw{background-position:-176px -48px}.ui-icon-arrowthickstop-1-n{background-position:-192px -48px}.ui-icon-arrowthickstop-1-e{background-position:-208px -48px}.ui-icon-arrowthickstop-1-s{background-position:-224px -48px}.ui-icon-arrowthickstop-1-w{background-position:-240px -48px}.ui-icon-arrowreturnthick-1-w{background-position:0 -64px}.ui-icon-arrowreturnthick-1-n{background-position:-16px -64px}.ui-icon-arrowreturnthick-1-e{background-position:-32px -64px}.ui-icon-arrowreturnthick-1-s{background-position:-48px -64px}.ui-icon-arrowreturn-1-w{background-position:-64px -64px}.ui-icon-arrowreturn-1-n{background-position:-80px -64px}.ui-icon-arrowreturn-1-e{background-position:-96px -64px}.ui-icon-arrowreturn-1-s{background-position:-112px -64px}.ui-icon-arrowrefresh-1-w{background-position:-128px -64px}.ui-icon-arrowrefresh-1-n{background-position:-144px -64px}.ui-icon-arrowrefresh-1-e{background-position:-160px -64px}.ui-icon-arrowrefresh-1-s{background-position:-176px -64px}.ui-icon-arrow-4{background-position:0 -80px}.ui-icon-arrow-4-diag{background-position:-16px -80px}.ui-icon-extlink{background-position:-32px -80px}.ui-icon-newwin{background-position:-48px -80px}.ui-icon-refresh{background-position:-64px -80px}.ui-icon-shuffle{background-position:-80px -80px}.ui-icon-transfer-e-w{background-position:-96px -80px}.ui-icon-transferthick-e-w{background-position:-112px -80px}.ui-icon-folder-collapsed{background-position:0 -96px}.ui-icon-folder-open{background-position:-16px -96px}.ui-icon-document{background-position:-32px -96px}.ui-icon-document-b{background-position:-48px -96px}.ui-icon-note{background-position:-64px -96px}.ui-icon-mail-closed{background-position:-80px -96px}.ui-icon-mail-open{background-position:-96px -96px}.ui-icon-suitcase{background-position:-112px -96px}.ui-icon-comment{background-position:-128px -96px}.ui-icon-person{background-position:-144px -96px}.ui-icon-print{background-position:-160px -96px}.ui-icon-trash{background-position:-176px -96px}.ui-icon-locked{background-position:-192px -96px}.ui-icon-unlocked{background-position:-208px -96px}.ui-icon-bookmark{background-position:-224px -96px}.ui-icon-tag{background-position:-240px -96px}.ui-icon-home{background-position:0 -112px}.ui-icon-flag{background-position:-16px -112px}.ui-icon-calendar{background-position:-32px -112px}.ui-icon-cart{background-position:-48px -112px}.ui-icon-pencil{background-position:-64px -112px}.ui-icon-clock{background-position:-80px -112px}.ui-icon-disk{background-position:-96px -112px}.ui-icon-calculator{background-position:-112px -112px}.ui-icon-zoomin{background-position:-128px -112px}.ui-icon-zoomout{background-position:-144px -112px}.ui-icon-search{background-position:-160px -112px}.ui-icon-wrench{background-position:-176px -112px}.ui-icon-gear{background-position:-192px -112px}.ui-icon-heart{background-position:-208px -112px}.ui-icon-star{background-position:-224px -112px}.ui-icon-link{background-position:-240px -112px}.ui-icon-cancel{background-position:0 -128px}.ui-icon-plus{background-position:-16px -128px}.ui-icon-plusthick{background-position:-32px -128px}.ui-icon-minus{background-position:-48px -128px}.ui-icon-minusthick{background-position:-64px -128px}.ui-icon-close{background-position:-80px -128px}.ui-icon-closethick{background-position:-96px -128px}.ui-icon-key{background-position:-112px -128px}.ui-icon-lightbulb{background-position:-128px -128px}.ui-icon-scissors{background-position:-144px -128px}.ui-icon-clipboard{background-position:-160px -128px}.ui-icon-copy{background-position:-176px -128px}.ui-icon-contact{background-position:-192px -128px}.ui-icon-image{background-position:-208px -128px}.ui-icon-video{background-position:-224px -128px}.ui-icon-script{background-position:-240px -128px}.ui-icon-alert{background-position:0 -144px}.ui-icon-info{background-position:-16px -144px}.ui-icon-notice{background-position:-32px -144px}.ui-icon-help{background-position:-48px -144px}.ui-icon-check{background-position:-64px -144px}.ui-icon-bullet{background-position:-80px -144px}.ui-icon-radio-on{background-position:-96px -144px}.ui-icon-radio-off{background-position:-112px -144px}.ui-icon-pin-w{background-position:-128px -144px}.ui-icon-pin-s{background-position:-144px -144px}.ui-icon-play{background-position:0 -160px}.ui-icon-pause{background-position:-16px -160px}.ui-icon-seek-next{background-position:-32px -160px}.ui-icon-seek-prev{background-position:-48px -160px}.ui-icon-seek-end{background-position:-64px -160px}.ui-icon-seek-start{background-position:-80px -160px}.ui-icon-seek-first{background-position:-80px -160px}.ui-icon-stop{background-position:-96px -160px}.ui-icon-eject{background-position:-112px -160px}.ui-icon-volume-off{background-position:-128px -160px}.ui-icon-volume-on{background-position:-144px -160px}.ui-icon-power{background-position:0 -176px}.ui-icon-signal-diag{background-position:-16px -176px}.ui-icon-signal{background-position:-32px -176px}.ui-icon-battery-0{background-position:-48px -176px}.ui-icon-battery-1{background-position:-64px -176px}.ui-icon-battery-2{background-position:-80px -176px}.ui-icon-battery-3{background-position:-96px -176px}.ui-icon-circle-plus{background-position:0 -192px}.ui-icon-circle-minus{background-position:-16px -192px}.ui-icon-circle-close{background-position:-32px -192px}.ui-icon-circle-triangle-e{background-position:-48px -192px}.ui-icon-circle-triangle-s{background-position:-64px -192px}.ui-icon-circle-triangle-w{background-position:-80px -192px}.ui-icon-circle-triangle-n{background-position:-96px -192px}.ui-icon-circle-arrow-e{background-position:-112px -192px}.ui-icon-circle-arrow-s{background-position:-128px -192px}.ui-icon-circle-arrow-w{background-position:-144px -192px}.ui-icon-circle-arrow-n{background-position:-160px -192px}.ui-icon-circle-zoomin{background-position:-176px -192px}.ui-icon-circle-zoomout{background-position:-192px -192px}.ui-icon-circle-check{background-position:-208px -192px}.ui-icon-circlesmall-plus{background-position:0 -208px}.ui-icon-circlesmall-minus{background-position:-16px -208px}.ui-icon-circlesmall-close{background-position:-32px -208px}.ui-icon-squaresmall-plus{background-position:-48px -208px}.ui-icon-squaresmall-minus{background-position:-64px -208px}.ui-icon-squaresmall-close{background-position:-80px -208px}.ui-icon-grip-dotted-vertical{background-position:0 -224px}.ui-icon-grip-dotted-horizontal{background-position:-16px -224px}.ui-icon-grip-solid-vertical{background-position:-32px -224px}.ui-icon-grip-solid-horizontal{background-position:-48px -224px}.ui-icon-gripsmall-diagonal-se{background-position:-64px -224px}.ui-icon-grip-diagonal-se{background-position:-80px -224px}.ui-corner-all,.ui-corner-top,.ui-corner-left,.ui-corner-tl{-moz-border-radius-topleft:6px;-webkit-border-top-left-radius:6px;-khtml-border-top-left-radius:6px;border-top-left-radius:6px}.ui-corner-all,.ui-corner-top,.ui-corner-right,.ui-corner-tr{-moz-border-radius-topright:6px;-webkit-border-top-right-radius:6px;-khtml-border-top-right-radius:6px;border-top-right-radius:6px}.ui-corner-all,.ui-corner-bottom,.ui-corner-left,.ui-corner-bl{-moz-border-radius-bottomleft:6px;-webkit-border-bottom-left-radius:6px;-khtml-border-bottom-left-radius:6px;border-bottom-left-radius:6px}.ui-corner-all,.ui-corner-bottom,.ui-corner-right,.ui-corner-br{-moz-border-radius-bottomright:6px;-webkit-border-bottom-right-radius:6px;-khtml-border-bottom-right-radius:6px;border-bottom-right-radius:6px}#simplemodal-overlay{background-color:#000}#simplemodal-container{height:360px;width:600px;color:#fff;background-color:#000;border:0;padding:0}#simplemodal-container .simplemodal-data{padding:20px 50px}.slick-slider{position:relative;display:block;box-sizing:border-box;-moz-box-sizing:border-box;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;-ms-touch-action:pan-y;touch-action:pan-y;-webkit-tap-highlight-color:transparent}.slick-list{position:relative;overflow:hidden;display:block;margin:0;padding:0}.slick-list:focus{outline:none}.slick-loading .slick-list{background:#fff url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/ajax-loader.gif") center center no-repeat}.slick-list.dragging{cursor:pointer;cursor:hand}.slick-slider .slick-list,.slick-track,.slick-slide,.slick-slide img{-webkit-transform:translate3d(0, 0, 0);-moz-transform:translate3d(0, 0, 0);-ms-transform:translate3d(0, 0, 0);-o-transform:translate3d(0, 0, 0);transform:translate3d(0, 0, 0)}.slick-track{position:relative;left:0;top:0;display:block;zoom:1}.slick-track:before,.slick-track:after{content:"";display:table}.slick-track:after{clear:both}.slick-loading .slick-track{visibility:hidden}.slick-slide{float:left;height:100%;min-height:1px;display:none}.slick-slide img{display:block}.slick-slide.slick-loading img{display:none}.slick-slide.dragging img{pointer-events:none}.slick-initialized .slick-slide{display:block}.slick-loading .slick-slide{visibility:hidden}.slick-vertical .slick-slide{display:block;height:auto;border:1px solid transparent}@font-face{font-family:"slick";src:url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/fonts/slick.eot");src:url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/fonts/slick.eot?#iefix") format("embedded-opentype"),url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/fonts/slick.woff") format("woff"),url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/fonts/slick.ttf") format("truetype"),url("https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/fonts/slick.svg#slick") format("svg");font-weight:normal;font-style:normal}.slick-prev,.slick-next{position:absolute;display:block;height:20px;width:20px;line-height:0;font-size:0;cursor:pointer;background:transparent;color:transparent;top:50%;margin-top:-10px;padding:0;border:none;outline:none}.slick-prev:hover,.slick-prev:focus,.slick-next:hover,.slick-next:focus{outline:none;background:transparent;color:transparent}.slick-prev:hover:before,.slick-prev:focus:before,.slick-next:hover:before,.slick-next:focus:before{opacity:1}.slick-prev.slick-disabled:before,.slick-next.slick-disabled:before{opacity:0.25}.slick-prev:before,.slick-next:before{font-family:"slick";font-size:20px;line-height:1;color:white;opacity:0.75;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}.slick-prev{left:-25px}.slick-prev:before{content:"\2190"}.slick-next{right:-25px}.slick-next:before{content:"\2192"}.slick-slider{margin-bottom:30px}.slick-dots{position:absolute;bottom:-45px;list-style:none;display:block;text-align:center;padding:0;width:100%}.slick-dots li{position:relative;display:inline-block;height:20px;width:20px;margin:0 5px;padding:0;cursor:pointer}.slick-dots li button{border:0;background:transparent;display:block;height:20px;width:20px;outline:none;line-height:0;font-size:0;color:transparent;padding:5px;cursor:pointer}.slick-dots li button:hover,.slick-dots li button:focus{outline:none}.slick-dots li button:hover:before,.slick-dots li button:focus:before{opacity:1}.slick-dots li button:before{position:absolute;top:0;left:0;content:"\2022";width:20px;height:20px;font-family:"slick";font-size:6px;line-height:20px;text-align:center;color:black;opacity:0.25;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}.slick-dots li.slick-active button:before{color:black;opacity:0.75}[dir="rtl"] .slick-next{right:auto;left:-25px}[dir="rtl"] .slick-next:before{content:"\2190"}[dir="rtl"] .slick-prev{right:-25px;left:auto}[dir="rtl"] .slick-prev:before{content:"\2192"}[dir="rtl"] .slick-slide{float:right}@font-face{font-family:"foundation-icons";src:url("https://mars.nasa.gov/assets/gulp/vendor/fonts/foundation-icons.eot");src:url("https://mars.nasa.gov/assets/gulp/vendor/fonts/foundation-icons.eot?#iefix") format("embedded-opentype"),url("https://mars.nasa.gov/assets/gulp/vendor/fonts/foundation-icons.woff") format("woff"),url("https://mars.nasa.gov/assets/gulp/vendor/fonts/foundation-icons.ttf") format("truetype"),url("https://mars.nasa.gov/assets/gulp/vendor/fonts/foundation-icons.svg#fontcustom") format("svg");font-weight:normal;font-style:normal}.fi-address-book:before,.fi-alert:before,.fi-align-center:before,.fi-align-justify:before,.fi-align-left:before,.fi-align-right:before,.fi-anchor:before,.fi-annotate:before,.fi-archive:before,.fi-arrow-down:before,.fi-arrow-left:before,.fi-arrow-right:before,.fi-arrow-up:before,.fi-arrows-compress:before,.fi-arrows-expand:before,.fi-arrows-in:before,.fi-arrows-out:before,.fi-asl:before,.fi-asterisk:before,.fi-at-sign:before,.fi-background-color:before,.fi-battery-empty:before,.fi-battery-full:before,.fi-battery-half:before,.fi-bitcoin-circle:before,.fi-bitcoin:before,.fi-blind:before,.fi-bluetooth:before,.fi-bold:before,.fi-book-bookmark:before,.fi-book:before,.fi-bookmark:before,.fi-braille:before,.fi-burst-new:before,.fi-burst-sale:before,.fi-burst:before,.fi-calendar:before,.fi-camera:before,.fi-check:before,.fi-checkbox:before,.fi-clipboard-notes:before,.fi-clipboard-pencil:before,.fi-clipboard:before,.fi-clock:before,.fi-closed-caption:before,.fi-cloud:before,.fi-comment-minus:before,.fi-comment-quotes:before,.fi-comment-video:before,.fi-comment:before,.fi-comments:before,.fi-compass:before,.fi-contrast:before,.fi-credit-card:before,.fi-crop:before,.fi-crown:before,.fi-css3:before,.fi-database:before,.fi-die-five:before,.fi-die-four:before,.fi-die-one:before,.fi-die-six:before,.fi-die-three:before,.fi-die-two:before,.fi-dislike:before,.fi-dollar-bill:before,.fi-dollar:before,.fi-download:before,.fi-eject:before,.fi-elevator:before,.fi-euro:before,.fi-eye:before,.fi-fast-forward:before,.fi-female-symbol:before,.fi-female:before,.fi-filter:before,.fi-first-aid:before,.fi-flag:before,.fi-folder-add:before,.fi-folder-lock:before,.fi-folder:before,.fi-foot:before,.fi-foundation:before,.fi-graph-bar:before,.fi-graph-horizontal:before,.fi-graph-pie:before,.fi-graph-trend:before,.fi-guide-dog:before,.fi-hearing-aid:before,.fi-heart:before,.fi-home:before,.fi-html5:before,.fi-indent-less:before,.fi-indent-more:before,.fi-info:before,.fi-italic:before,.fi-key:before,.fi-laptop:before,.fi-layout:before,.fi-lightbulb:before,.fi-like:before,.fi-link:before,.fi-list-bullet:before,.fi-list-number:before,.fi-list-thumbnails:before,.fi-list:before,.fi-lock:before,.fi-loop:before,.fi-magnifying-glass:before,.fi-mail:before,.fi-male-female:before,.fi-male-symbol:before,.fi-male:before,.fi-map:before,.fi-marker:before,.fi-megaphone:before,.fi-microphone:before,.fi-minus-circle:before,.fi-minus:before,.fi-mobile-signal:before,.fi-mobile:before,.fi-monitor:before,.fi-mountains:before,.fi-music:before,.fi-next:before,.fi-no-dogs:before,.fi-no-smoking:before,.fi-page-add:before,.fi-page-copy:before,.fi-page-csv:before,.fi-page-delete:before,.fi-page-doc:before,.fi-page-edit:before,.fi-page-export-csv:before,.fi-page-export-doc:before,.fi-page-export-pdf:before,.fi-page-export:before,.fi-page-filled:before,.fi-page-multiple:before,.fi-page-pdf:before,.fi-page-remove:before,.fi-page-search:before,.fi-page:before,.fi-paint-bucket:before,.fi-paperclip:before,.fi-pause:before,.fi-paw:before,.fi-paypal:before,.fi-pencil:before,.fi-photo:before,.fi-play-circle:before,.fi-play-video:before,.fi-play:before,.fi-plus:before,.fi-pound:before,.fi-power:before,.fi-previous:before,.fi-price-tag:before,.fi-pricetag-multiple:before,.fi-print:before,.fi-prohibited:before,.fi-projection-screen:before,.fi-puzzle:before,.fi-quote:before,.fi-record:before,.fi-refresh:before,.fi-results-demographics:before,.fi-results:before,.fi-rewind-ten:before,.fi-rewind:before,.fi-rss:before,.fi-safety-cone:before,.fi-save:before,.fi-share:before,.fi-sheriff-badge:before,.fi-shield:before,.fi-shopping-bag:before,.fi-shopping-cart:before,.fi-shuffle:before,.fi-skull:before,.fi-social-500px:before,.fi-social-adobe:before,.fi-social-amazon:before,.fi-social-android:before,.fi-social-apple:before,.fi-social-behance:before,.fi-social-bing:before,.fi-social-blogger:before,.fi-social-delicious:before,.fi-social-designer-news:before,.fi-social-deviant-art:before,.fi-social-digg:before,.fi-social-dribbble:before,.fi-social-drive:before,.fi-social-dropbox:before,.fi-social-evernote:before,.fi-social-facebook:before,.fi-social-flickr:before,.fi-social-forrst:before,.fi-social-foursquare:before,.fi-social-game-center:before,.fi-social-github:before,.fi-social-google-plus:before,.fi-social-hacker-news:before,.fi-social-hi5:before,.fi-social-instagram:before,.fi-social-joomla:before,.fi-social-lastfm:before,.fi-social-linkedin:before,.fi-social-medium:before,.fi-social-myspace:before,.fi-social-orkut:before,.fi-social-path:before,.fi-social-picasa:before,.fi-social-pinterest:before,.fi-social-rdio:before,.fi-social-reddit:before,.fi-social-skillshare:before,.fi-social-skype:before,.fi-social-smashing-mag:before,.fi-social-snapchat:before,.fi-social-spotify:before,.fi-social-squidoo:before,.fi-social-stack-overflow:before,.fi-social-steam:before,.fi-social-stumbleupon:before,.fi-social-treehouse:before,.fi-social-tumblr:before,.fi-social-twitter:before,.fi-social-vimeo:before,.fi-social-windows:before,.fi-social-xbox:before,.fi-social-yahoo:before,.fi-social-yelp:before,.fi-social-youtube:before,.fi-social-zerply:before,.fi-social-zurb:before,.fi-sound:before,.fi-star:before,.fi-stop:before,.fi-strikethrough:before,.fi-subscript:before,.fi-superscript:before,.fi-tablet-landscape:before,.fi-tablet-portrait:before,.fi-target-two:before,.fi-target:before,.fi-telephone-accessible:before,.fi-telephone:before,.fi-text-color:before,.fi-thumbnails:before,.fi-ticket:before,.fi-torso-business:before,.fi-torso-female:before,.fi-torso:before,.fi-torsos-all-female:before,.fi-torsos-all:before,.fi-torsos-female-male:before,.fi-torsos-male-female:before,.fi-torsos:before,.fi-trash:before,.fi-trees:before,.fi-trophy:before,.fi-underline:before,.fi-universal-access:before,.fi-unlink:before,.fi-unlock:before,.fi-upload-cloud:before,.fi-upload:before,.fi-usb:before,.fi-video:before,.fi-volume-none:before,.fi-volume-strike:before,.fi-volume:before,.fi-web:before,.fi-wheelchair:before,.fi-widget:before,.fi-wrench:before,.fi-x-circle:before,.fi-x:before,.fi-yen:before,.fi-zoom-in:before,.fi-zoom-out:before{font-family:"foundation-icons";font-style:normal;font-weight:normal;font-variant:normal;text-transform:none;line-height:1;-webkit-font-smoothing:antialiased;display:inline-block;text-decoration:inherit}.fi-address-book:before{content:"\f100"}.fi-alert:before{content:"\f101"}.fi-align-center:before{content:"\f102"}.fi-align-justify:before{content:"\f103"}.fi-align-left:before{content:"\f104"}.fi-align-right:before{content:"\f105"}.fi-anchor:before{content:"\f106"}.fi-annotate:before{content:"\f107"}.fi-archive:before{content:"\f108"}.fi-arrow-down:before{content:"\f109"}.fi-arrow-left:before{content:"\f10a"}.fi-arrow-right:before{content:"\f10b"}.fi-arrow-up:before{content:"\f10c"}.fi-arrows-compress:before{content:"\f10d"}.fi-arrows-expand:before{content:"\f10e"}.fi-arrows-in:before{content:"\f10f"}.fi-arrows-out:before{content:"\f110"}.fi-asl:before{content:"\f111"}.fi-asterisk:before{content:"\f112"}.fi-at-sign:before{content:"\f113"}.fi-background-color:before{content:"\f114"}.fi-battery-empty:before{content:"\f115"}.fi-battery-full:before{content:"\f116"}.fi-battery-half:before{content:"\f117"}.fi-bitcoin-circle:before{content:"\f118"}.fi-bitcoin:before{content:"\f119"}.fi-blind:before{content:"\f11a"}.fi-bluetooth:before{content:"\f11b"}.fi-bold:before{content:"\f11c"}.fi-book-bookmark:before{content:"\f11d"}.fi-book:before{content:"\f11e"}.fi-bookmark:before{content:"\f11f"}.fi-braille:before{content:"\f120"}.fi-burst-new:before{content:"\f121"}.fi-burst-sale:before{content:"\f122"}.fi-burst:before{content:"\f123"}.fi-calendar:before{content:"\f124"}.fi-camera:before{content:"\f125"}.fi-check:before{content:"\f126"}.fi-checkbox:before{content:"\f127"}.fi-clipboard-notes:before{content:"\f128"}.fi-clipboard-pencil:before{content:"\f129"}.fi-clipboard:before{content:"\f12a"}.fi-clock:before{content:"\f12b"}.fi-closed-caption:before{content:"\f12c"}.fi-cloud:before{content:"\f12d"}.fi-comment-minus:before{content:"\f12e"}.fi-comment-quotes:before{content:"\f12f"}.fi-comment-video:before{content:"\f130"}.fi-comment:before{content:"\f131"}.fi-comments:before{content:"\f132"}.fi-compass:before{content:"\f133"}.fi-contrast:before{content:"\f134"}.fi-credit-card:before{content:"\f135"}.fi-crop:before{content:"\f136"}.fi-crown:before{content:"\f137"}.fi-css3:before{content:"\f138"}.fi-database:before{content:"\f139"}.fi-die-five:before{content:"\f13a"}.fi-die-four:before{content:"\f13b"}.fi-die-one:before{content:"\f13c"}.fi-die-six:before{content:"\f13d"}.fi-die-three:before{content:"\f13e"}.fi-die-two:before{content:"\f13f"}.fi-dislike:before{content:"\f140"}.fi-dollar-bill:before{content:"\f141"}.fi-dollar:before{content:"\f142"}.fi-download:before{content:"\f143"}.fi-eject:before{content:"\f144"}.fi-elevator:before{content:"\f145"}.fi-euro:before{content:"\f146"}.fi-eye:before{content:"\f147"}.fi-fast-forward:before{content:"\f148"}.fi-female-symbol:before{content:"\f149"}.fi-female:before{content:"\f14a"}.fi-filter:before{content:"\f14b"}.fi-first-aid:before{content:"\f14c"}.fi-flag:before{content:"\f14d"}.fi-folder-add:before{content:"\f14e"}.fi-folder-lock:before{content:"\f14f"}.fi-folder:before{content:"\f150"}.fi-foot:before{content:"\f151"}.fi-foundation:before{content:"\f152"}.fi-graph-bar:before{content:"\f153"}.fi-graph-horizontal:before{content:"\f154"}.fi-graph-pie:before{content:"\f155"}.fi-graph-trend:before{content:"\f156"}.fi-guide-dog:before{content:"\f157"}.fi-hearing-aid:before{content:"\f158"}.fi-heart:before{content:"\f159"}.fi-home:before{content:"\f15a"}.fi-html5:before{content:"\f15b"}.fi-indent-less:before{content:"\f15c"}.fi-indent-more:before{content:"\f15d"}.fi-info:before{content:"\f15e"}.fi-italic:before{content:"\f15f"}.fi-key:before{content:"\f160"}.fi-laptop:before{content:"\f161"}.fi-layout:before{content:"\f162"}.fi-lightbulb:before{content:"\f163"}.fi-like:before{content:"\f164"}.fi-link:before{content:"\f165"}.fi-list-bullet:before{content:"\f166"}.fi-list-number:before{content:"\f167"}.fi-list-thumbnails:before{content:"\f168"}.fi-list:before{content:"\f169"}.fi-lock:before{content:"\f16a"}.fi-loop:before{content:"\f16b"}.fi-magnifying-glass:before{content:"\f16c"}.fi-mail:before{content:"\f16d"}.fi-male-female:before{content:"\f16e"}.fi-male-symbol:before{content:"\f16f"}.fi-male:before{content:"\f170"}.fi-map:before{content:"\f171"}.fi-marker:before{content:"\f172"}.fi-megaphone:before{content:"\f173"}.fi-microphone:before{content:"\f174"}.fi-minus-circle:before{content:"\f175"}.fi-minus:before{content:"\f176"}.fi-mobile-signal:before{content:"\f177"}.fi-mobile:before{content:"\f178"}.fi-monitor:before{content:"\f179"}.fi-mountains:before{content:"\f17a"}.fi-music:before{content:"\f17b"}.fi-next:before{content:"\f17c"}.fi-no-dogs:before{content:"\f17d"}.fi-no-smoking:before{content:"\f17e"}.fi-page-add:before{content:"\f17f"}.fi-page-copy:before{content:"\f180"}.fi-page-csv:before{content:"\f181"}.fi-page-delete:before{content:"\f182"}.fi-page-doc:before{content:"\f183"}.fi-page-edit:before{content:"\f184"}.fi-page-export-csv:before{content:"\f185"}.fi-page-export-doc:before{content:"\f186"}.fi-page-export-pdf:before{content:"\f187"}.fi-page-export:before{content:"\f188"}.fi-page-filled:before{content:"\f189"}.fi-page-multiple:before{content:"\f18a"}.fi-page-pdf:before{content:"\f18b"}.fi-page-remove:before{content:"\f18c"}.fi-page-search:before{content:"\f18d"}.fi-page:before{content:"\f18e"}.fi-paint-bucket:before{content:"\f18f"}.fi-paperclip:before{content:"\f190"}.fi-pause:before{content:"\f191"}.fi-paw:before{content:"\f192"}.fi-paypal:before{content:"\f193"}.fi-pencil:before{content:"\f194"}.fi-photo:before{content:"\f195"}.fi-play-circle:before{content:"\f196"}.fi-play-video:before{content:"\f197"}.fi-play:before{content:"\f198"}.fi-plus:before{content:"\f199"}.fi-pound:before{content:"\f19a"}.fi-power:before{content:"\f19b"}.fi-previous:before{content:"\f19c"}.fi-price-tag:before{content:"\f19d"}.fi-pricetag-multiple:before{content:"\f19e"}.fi-print:before{content:"\f19f"}.fi-prohibited:before{content:"\f1a0"}.fi-projection-screen:before{content:"\f1a1"}.fi-puzzle:before{content:"\f1a2"}.fi-quote:before{content:"\f1a3"}.fi-record:before{content:"\f1a4"}.fi-refresh:before{content:"\f1a5"}.fi-results-demographics:before{content:"\f1a6"}.fi-results:before{content:"\f1a7"}.fi-rewind-ten:before{content:"\f1a8"}.fi-rewind:before{content:"\f1a9"}.fi-rss:before{content:"\f1aa"}.fi-safety-cone:before{content:"\f1ab"}.fi-save:before{content:"\f1ac"}.fi-share:before{content:"\f1ad"}.fi-sheriff-badge:before{content:"\f1ae"}.fi-shield:before{content:"\f1af"}.fi-shopping-bag:before{content:"\f1b0"}.fi-shopping-cart:before{content:"\f1b1"}.fi-shuffle:before{content:"\f1b2"}.fi-skull:before{content:"\f1b3"}.fi-social-500px:before{content:"\f1b4"}.fi-social-adobe:before{content:"\f1b5"}.fi-social-amazon:before{content:"\f1b6"}.fi-social-android:before{content:"\f1b7"}.fi-social-apple:before{content:"\f1b8"}.fi-social-behance:before{content:"\f1b9"}.fi-social-bing:before{content:"\f1ba"}.fi-social-blogger:before{content:"\f1bb"}.fi-social-delicious:before{content:"\f1bc"}.fi-social-designer-news:before{content:"\f1bd"}.fi-social-deviant-art:before{content:"\f1be"}.fi-social-digg:before{content:"\f1bf"}.fi-social-dribbble:before{content:"\f1c0"}.fi-social-drive:before{content:"\f1c1"}.fi-social-dropbox:before{content:"\f1c2"}.fi-social-evernote:before{content:"\f1c3"}.fi-social-facebook:before{content:"\f1c4"}.fi-social-flickr:before{content:"\f1c5"}.fi-social-forrst:before{content:"\f1c6"}.fi-social-foursquare:before{content:"\f1c7"}.fi-social-game-center:before{content:"\f1c8"}.fi-social-github:before{content:"\f1c9"}.fi-social-google-plus:before{content:"\f1ca"}.fi-social-hacker-news:before{content:"\f1cb"}.fi-social-hi5:before{content:"\f1cc"}.fi-social-instagram:before{content:"\f1cd"}.fi-social-joomla:before{content:"\f1ce"}.fi-social-lastfm:before{content:"\f1cf"}.fi-social-linkedin:before{content:"\f1d0"}.fi-social-medium:before{content:"\f1d1"}.fi-social-myspace:before{content:"\f1d2"}.fi-social-orkut:before{content:"\f1d3"}.fi-social-path:before{content:"\f1d4"}.fi-social-picasa:before{content:"\f1d5"}.fi-social-pinterest:before{content:"\f1d6"}.fi-social-rdio:before{content:"\f1d7"}.fi-social-reddit:before{content:"\f1d8"}.fi-social-skillshare:before{content:"\f1d9"}.fi-social-skype:before{content:"\f1da"}.fi-social-smashing-mag:before{content:"\f1db"}.fi-social-snapchat:before{content:"\f1dc"}.fi-social-spotify:before{content:"\f1dd"}.fi-social-squidoo:before{content:"\f1de"}.fi-social-stack-overflow:before{content:"\f1df"}.fi-social-steam:before{content:"\f1e0"}.fi-social-stumbleupon:before{content:"\f1e1"}.fi-social-treehouse:before{content:"\f1e2"}.fi-social-tumblr:before{content:"\f1e3"}.fi-social-twitter:before{content:"\f1e4"}.fi-social-vimeo:before{content:"\f1e5"}.fi-social-windows:before{content:"\f1e6"}.fi-social-xbox:before{content:"\f1e7"}.fi-social-yahoo:before{content:"\f1e8"}.fi-social-yelp:before{content:"\f1e9"}.fi-social-youtube:before{content:"\f1ea"}.fi-social-zerply:before{content:"\f1eb"}.fi-social-zurb:before{content:"\f1ec"}.fi-sound:before{content:"\f1ed"}.fi-star:before{content:"\f1ee"}.fi-stop:before{content:"\f1ef"}.fi-strikethrough:before{content:"\f1f0"}.fi-subscript:before{content:"\f1f1"}.fi-superscript:before{content:"\f1f2"}.fi-tablet-landscape:before{content:"\f1f3"}.fi-tablet-portrait:before{content:"\f1f4"}.fi-target-two:before{content:"\f1f5"}.fi-target:before{content:"\f1f6"}.fi-telephone-accessible:before{content:"\f1f7"}.fi-telephone:before{content:"\f1f8"}.fi-text-color:before{content:"\f1f9"}.fi-thumbnails:before{content:"\f1fa"}.fi-ticket:before{content:"\f1fb"}.fi-torso-business:before{content:"\f1fc"}.fi-torso-female:before{content:"\f1fd"}.fi-torso:before{content:"\f1fe"}.fi-torsos-all-female:before{content:"\f1ff"}.fi-torsos-all:before{content:"\f200"}.fi-torsos-female-male:before{content:"\f201"}.fi-torsos-male-female:before{content:"\f202"}.fi-torsos:before{content:"\f203"}.fi-trash:before{content:"\f204"}.fi-trees:before{content:"\f205"}.fi-trophy:before{content:"\f206"}.fi-underline:before{content:"\f207"}.fi-universal-access:before{content:"\f208"}.fi-unlink:before{content:"\f209"}.fi-unlock:before{content:"\f20a"}.fi-upload-cloud:before{content:"\f20b"}.fi-upload:before{content:"\f20c"}.fi-usb:before{content:"\f20d"}.fi-video:before{content:"\f20e"}.fi-volume-none:before{content:"\f20f"}.fi-volume-strike:before{content:"\f210"}.fi-volume:before{content:"\f211"}.fi-web:before{content:"\f212"}.fi-wheelchair:before{content:"\f213"}.fi-widget:before{content:"\f214"}.fi-wrench:before{content:"\f215"}.fi-x-circle:before{content:"\f216"}.fi-x:before{content:"\f217"}.fi-yen:before{content:"\f218"}.fi-zoom-in:before{content:"\f219"}.fi-zoom-out:before{content:"\f21a"}/*! normalize.css v1.1.2 | MIT License | git.io/normalize */article,aside,details,figcaption,figure,footer,header,hgroup,main,nav,section,summary{display:block}audio,canvas,video{display:inline-block;*display:inline;*zoom:1}audio:not([controls]){display:none;height:0}[hidden]{display:none}html{font-size:100%;-ms-text-size-adjust:100%;-webkit-text-size-adjust:100%}html,button,input,select,textarea{font-family:sans-serif}body{margin:0}a:focus{outline:thin dotted}a:active,a:hover{outline:0}h1{font-size:2em;margin:0.67em 0}h2{font-size:1.5em;margin:0.83em 0}h3{font-size:1.17em;margin:1em 0}h4{font-size:1em;margin:1.33em 0}h5{font-size:0.83em;margin:1.67em 0}h6{font-size:0.67em;margin:2.33em 0}abbr[title]{border-bottom:1px dotted}b,strong{font-weight:bold}blockquote{margin:1em 40px}dfn{font-style:italic}hr{-moz-box-sizing:content-box;box-sizing:content-box;height:0}mark{background:#ff0;color:#000}p,pre{margin:1em 0}code,kbd,pre,samp{font-family:monospace, serif;_font-family:'courier new', monospace;font-size:1em}pre{white-space:pre;white-space:pre-wrap;word-wrap:break-word}q{quotes:none}q:before,q:after{content:'';content:none}small{font-size:80%}sub,sup{font-size:75%;line-height:0;position:relative;vertical-align:baseline}sup{top:-0.5em}sub{bottom:-0.25em}dl,menu,ol,ul{margin:1em 0}dd{margin:0 0 0 40px}menu,ol,ul{padding:0 0 0 40px}nav ul,nav ol{list-style:none;list-style-image:none}img{border:0;-ms-interpolation-mode:bicubic}svg:not(:root){overflow:hidden}figure{margin:0}form{margin:0}fieldset{border:1px solid #c0c0c0;margin:0 2px;padding:0.35em 0.625em 0.75em}legend{border:0;padding:0;white-space:normal;*margin-left:-7px}button,input,select,textarea{font-size:100%;margin:0;vertical-align:baseline;*vertical-align:middle}button,input{line-height:normal}button,select{text-transform:none}button,html input[type="button"],input[type="reset"],input[type="submit"]{-webkit-appearance:button;cursor:pointer;*overflow:visible}button[disabled],html input[disabled]{cursor:default}input[type="checkbox"],input[type="radio"]{box-sizing:border-box;padding:0;*height:13px;*width:13px}input[type="search"]{-webkit-appearance:textfield;-moz-box-sizing:content-box;-webkit-box-sizing:content-box;box-sizing:content-box}input[type="search"]::-webkit-search-cancel-button,input[type="search"]::-webkit-search-decoration{-webkit-appearance:none}button::-moz-focus-inner,input::-moz-focus-inner{border:0;padding:0}textarea{overflow:auto;vertical-align:top}table{border-collapse:collapse;border-spacing:0}html,button,input,select,textarea{color:#222}body{font-size:1em;line-height:1.4}::-moz-selection{background:#b3d4fc;text-shadow:none}::selection{background:#b3d4fc;text-shadow:none}hr{display:block;height:1px;border:0;border-top:1px solid #ccc;margin:1em 0;padding:0}audio,canvas,img,video{vertical-align:middle}fieldset{border:0;margin:0;padding:0}textarea{resize:vertical}.browsehappy{margin:0.2em 0;background:#ccc;color:#000;padding:0.2em 0}.ir{background-color:transparent;border:0;overflow:hidden;*text-indent:-9999px}.ir:before{content:"";display:block;width:0;height:150%}.hidden{display:none !important;visibility:hidden}.visuallyhidden{border:0;clip:rect(0 0 0 0);height:1px;margin:-1px;overflow:hidden;padding:0;position:absolute;width:1px}.visuallyhidden.focusable:active,.visuallyhidden.focusable:focus{clip:auto;height:auto;margin:0;overflow:visible;position:static;width:auto}.invisible{visibility:hidden}.clearfix:before,.body_form fieldset.radio_buttons .option:before,#site_footer .sitemap_directory:before,.main_area_sitemap .sitemap_directory:before,article:before,.grid_gallery.list_view li.slide:before,.main_carousel .slick-nav:before,.main_carousel.module .slick-slider .content_body:before,.advanced_search .filter_bar .search_row:before,.content_page #primary_column:before,#secondary_column aside.list_view_module li:before,.wysiwyg_content .related_content_module ul:before,#secondary_column .related_content_module ul:before,.wysiwyg_content .related_content_module li:before,#secondary_column .related_content_module li:before,blockquote:before,.faq_section ul.q_and_a .text.answer:before,ul.item_list:before,ul.item_list&gt;li:before,ul.item_list .list_content:before,.clearfix:after,.body_form fieldset.radio_buttons .option:after,#site_footer .sitemap_directory:after,.main_area_sitemap .sitemap_directory:after,article:after,.grid_gallery.list_view li.slide:after,.main_carousel .slick-nav:after,.main_carousel.module .slick-slider .content_body:after,.advanced_search .filter_bar .search_row:after,.content_page #primary_column:after,#secondary_column aside.list_view_module li:after,.wysiwyg_content .related_content_module ul:after,#secondary_column .related_content_module ul:after,.wysiwyg_content .related_content_module li:after,#secondary_column .related_content_module li:after,blockquote:after,.faq_section ul.q_and_a .text.answer:after,ul.item_list:after,ul.item_list&gt;li:after,ul.item_list .list_content:after{content:" ";display:table}.clearfix:after,.body_form fieldset.radio_buttons .option:after,#site_footer .sitemap_directory:after,.main_area_sitemap .sitemap_directory:after,article:after,.grid_gallery.list_view li.slide:after,.main_carousel .slick-nav:after,.main_carousel.module .slick-slider .content_body:after,.advanced_search .filter_bar .search_row:after,.content_page #primary_column:after,#secondary_column aside.list_view_module li:after,.wysiwyg_content .related_content_module ul:after,#secondary_column .related_content_module ul:after,.wysiwyg_content .related_content_module li:after,#secondary_column .related_content_module li:after,blockquote:after,.faq_section ul.q_and_a .text.answer:after,ul.item_list:after,ul.item_list&gt;li:after,ul.item_list .list_content:after{clear:both}.clearfix,.body_form fieldset.radio_buttons .option,#site_footer .sitemap_directory,.main_area_sitemap .sitemap_directory,article,.grid_gallery.list_view li.slide,.main_carousel .slick-nav,.main_carousel.module .slick-slider .content_body,.advanced_search .filter_bar .search_row,.content_page #primary_column,#secondary_column aside.list_view_module li,.wysiwyg_content .related_content_module ul,#secondary_column .related_content_module ul,.wysiwyg_content .related_content_module li,#secondary_column .related_content_module li,blockquote,.faq_section ul.q_and_a .text.answer,ul.item_list,ul.item_list&gt;li,ul.item_list .list_content{*zoom:1}@media print{*{background:transparent !important;color:#000 !important;box-shadow:none !important;text-shadow:none !important}a,a:visited{text-decoration:underline}a[href]:after{content:" (" attr(href) ")"}abbr[title]:after{content:" (" attr(title) ")"}.ir a:after,a[href^="javascript:"]:after,a[href^="#"]:after{content:""}pre,blockquote{border:1px solid #999;page-break-inside:avoid}thead{display:table-header-group}tr,img{page-break-inside:avoid}img{max-width:100% !important}@page{margin:0.5cm}p,h2,h3{orphans:3;widows:3}h2,h3{page-break-after:avoid}}html,button,input,select,textarea{color:#3c3c3c}.browsehappy{background:white;color:#333;padding:1em;position:absolute;top:0;left:0;z-index:9999;width:100%;height:100%}html.touch.-webkit-{-webkit-tap-highlight-color:transparent}.visuallyhidden.focusable:active,.visuallyhidden.focusable:focus{position:absolute}.site_header_area .brand_area{background:url("https://mars.nasa.gov/assets/logo_nasa_trio@2x.png") no-repeat;background-size:100%;display:inline-block;width:54px;height:54px}.site_header_area .brand_area .brand1{height:100%;float:left;text-indent:-9999px}.site_header_area .brand_area .brand2{display:block;float:left;height:100%;text-indent:-9999px}.site_header_area .brand_area a.top_logo,.site_header_area .brand_area a.sub_logo{width:100%;float:left}.site_header_area .brand_area a.top_logo{height:39%;width:30%}.site_header_area .brand_area a.sub_logo{height:45%}.site_header_area .brand_area a.single_logo{width:100%;float:left;height:82%}.site_header_area .brand_area .nasa_logo{width:100%;height:100%;display:block}*,*:before,*:after{-moz-box-sizing:border-box;-webkit-box-sizing:border-box;box-sizing:border-box}body{margin-left:auto;margin-right:auto;margin-top:0;background-color:white}@media (max-width: 1023px){body.nav_overlay_true{overflow:hidden}}img{width:100%}p{line-height:1.4em;margin-bottom:17px;margin-top:0;font-size:16px;color:#222}@media (min-width: 600px), print{p{font-size:18px}}@media (min-width: 769px), print{p{margin-bottom:20px;font-size:16px}}@media (min-width: 1024px), print{p{font-size:17px}}@media (min-width: 1200px){p{font-size:18px}}a{text-decoration:none;color:#257cdf}a:hover{text-decoration:underline}a[name]{position:relative;display:block;visibility:hidden;margin:0;padding:0}@media (max-width: 1023px){a[name]{top:-58px}}@media (max-width: 1023px) and (min-width: 480px){a[name]{top:-58px}}@media (max-width: 1023px) and (min-width: 600px), print and (max-width: 1023px){a[name]{top:-58px}}@media (max-width: 1023px) and (min-width: 769px), print and (max-width: 1023px){a[name]{top:-70px}}@media (min-width: 1024px){a[name]{top:-47px}}dl,menu,ol,ul{margin:0;padding:0}ul{list-style-type:none}ol{list-style-position:inside}hr,.gradient_line,.related.module .gradient_line_module_top{clear:both;margin:1em 0}.print_only{display:none}@font-face{font-family:'Whitney';src:url("https://mars.nasa.gov/assets/fonts/Whitney-Book.otf")}@font-face{font-family:'Whitney-Bold';src:url("https://mars.nasa.gov/assets/fonts/Whitney-Bold.otf")}@font-face{font-family:'WhitneyCondensed-Bold';src:url("https://mars.nasa.gov/assets/fonts/WhitneyCondensed-Bold.otf")}.button,.outline_button,.primary_media_feature .floating_text_area .button,.banner_header_overlay .button{font-weight:700;display:inline-block;margin-bottom:.5em;margin-left:auto;margin-right:auto;background-color:#3b788b;color:white;line-height:1em;border:0;text-decoration:none;border-radius:4px;cursor:pointer;text-shadow:none;font-size:13px;padding:12px 24px;text-transform:uppercase;white-space:nowrap}@media (min-width: 769px), print{.button,.outline_button,.primary_media_feature .floating_text_area .button,.banner_header_overlay .button{font-size:14px}}.button:hover,.outline_button:hover,.primary_media_feature .floating_text_area .button:hover{background-color:#5097ad;text-decoration:none}.outline_button,.primary_media_feature .floating_text_area .button,.primary_media_feature .floating_text_area .outline_button,.banner_header_overlay .button,.banner_header_overlay .outline_button{border-radius:10px;border:2px solid white;background:none;color:#FFF}.outline_button:hover,.primary_media_feature .floating_text_area .button:hover,.primary_media_feature .floating_text_area .outline_button:hover,.banner_header_overlay .button:hover{background-color:#5097ad;border-color:#5097ad}.section_search,.overlay_search{color:white;display:inline-block;position:relative}.section_search .search_field,.overlay_search .search_field{color:white;background-color:#282828;background-color:rgba(255,255,255,0.1);font-weight:500;font-size:16px;border:none;border-radius:4px;height:40px;padding-left:1.1em;padding-right:40px;width:155px}.section_search .search_field.placeholder,.overlay_search .search_field.placeholder{color:rgba(255,255,255,0.8);-webkit-font-smoothing:antialiased;opacity:1 !important;font-family:"Montserrat",Helvetica,Arial,sans-serif}.section_search .search_field:-moz-placeholder,.overlay_search .search_field:-moz-placeholder{color:rgba(255,255,255,0.8);-webkit-font-smoothing:antialiased;opacity:1 !important;font-family:"Montserrat",Helvetica,Arial,sans-serif}.section_search .search_field::-moz-placeholder,.overlay_search .search_field::-moz-placeholder{color:rgba(255,255,255,0.8);-webkit-font-smoothing:antialiased;opacity:1 !important;font-family:"Montserrat",Helvetica,Arial,sans-serif}.section_search .search_field::-webkit-input-placeholder,.overlay_search .search_field::-webkit-input-placeholder{color:rgba(255,255,255,0.8);-webkit-font-smoothing:antialiased;opacity:1 !important;font-family:"Montserrat",Helvetica,Arial,sans-serif}.section_search .search_field:-ms-input-placeholder,.overlay_search .search_field:-ms-input-placeholder{color:rgba(255,255,255,0.8);-webkit-font-smoothing:antialiased;opacity:1 !important;font-family:"Montserrat",Helvetica,Arial,sans-serif}.section_search .search_submit,.overlay_search .search_submit{padding:0;cursor:pointer;width:42px;height:42px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -127px -5px;background-size:300px;position:absolute;right:-5px;top:-3px;border:none;margin-left:-44px;opacity:.8}.section_search .search_submit:hover,.overlay_search .search_submit:hover,.section_search .search_submit.active,.overlay_search .search_submit.active,.section_search .search_submit.current,.overlay_search .search_submit.current{background-position:-127px -5px}.section_search .search_field{background-color:#F3F4F8;color:#222}.section_search .search_field.placeholder{color:rgba(255,255,255,0.8);opacity:1 !important}.section_search .search_field:-moz-placeholder{color:rgba(255,255,255,0.8);opacity:1 !important}.section_search .search_field::-moz-placeholder{color:rgba(255,255,255,0.8);opacity:1 !important}.section_search .search_field::-webkit-input-placeholder{color:rgba(255,255,255,0.8);opacity:1 !important}.section_search .search_field:-ms-input-placeholder{color:rgba(255,255,255,0.8);opacity:1 !important}.section_search .search_submit{padding:0;cursor:pointer;width:42px;height:42px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -127px -54px;background-size:300px;opacity:.6}.section_search .search_submit:hover,.section_search .search_submit.active,.section_search .search_submit.current{background-position:-127px -54px}form.nav_search .search_field{padding-right:20px;height:34px}form.nav_search input:-webkit-autofill,form.overlay_search input:-webkit-autofill{-webkit-box-shadow:0 0 0px 1000px #989898 inset;-webkit-text-fill-color:white !important}.overlay_search .search_field{color:white;background-color:rgba(255,255,255,0.3)}.overlay_search .search_field.placeholder{color:white}.overlay_search .search_field:-moz-placeholder{color:white}.overlay_search .search_field::-moz-placeholder{color:white}.overlay_search .search_field::-webkit-input-placeholder{color:white}.overlay_search .search_field:-ms-input-placeholder{color:white}.overlay_search label.search_label{display:none}.overlay_search .search_submit{padding:0;cursor:pointer;width:42px;height:42px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -131px -5px;background-size:300px}.overlay_search .search_submit:hover,.overlay_search .search_submit.active,.overlay_search .search_submit.current{background-position:-131px -5px}.body_form label{display:block;margin-bottom:.3em}.body_form input:not([type="submit"]):not([type="reset"]),.body_form textarea{font-size:16px}.body_form input[type="text"]:not(#recaptcha_response_field),.body_form input[type="tel"],.body_form input[type="email"]{height:40px}.body_form input:not(#recaptcha_response_field):not(.inline_button):not([type="submit"]):not([type="radio"]):not([type="checkbox"]),.body_form textarea{width:100%;border:1px solid #a7a8a8;background-color:white;border-radius:4px;padding:10px 12px}.body_form input,.body_form textarea{margin-bottom:1em}.body_form .button,.body_form .outline_button,.body_form .primary_media_feature .floating_text_area .button,.primary_media_feature .floating_text_area .body_form .button{margin-top:1em}.body_form select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin-bottom:1em}.body_form select::-ms-expand{display:none}.body_form select option{padding:0.5em 1em}.body_form label{font-weight:700}.body_form .radio_title{margin-bottom:.5em;font-weight:700}.body_form fieldset.radio_buttons .option{white-space:nowrap;margin-bottom:1em}.body_form fieldset.radio_buttons label{white-space:normal;vertical-align:middle;display:inline}.body_form fieldset.radio_buttons input[type="radio"],.body_form fieldset.radio_buttons input[type="checkbox"]{display:inline-block;margin:0 .5em 0 0;vertical-align:middle}.body_form fieldset.radio_buttons input[type="radio"]+label,.body_form fieldset.radio_buttons input[type="checkbox"]+label{font-weight:400}.body_form .centered{text-align:center}@media (max-width: 480px){#recaptcha_widget_div{overflow:hidden}#recaptcha_widget_div #recaptcha_area{margin:0 auto}}.event_location,.event_date{margin-bottom:1em}.site_header_area{height:58px}@media (min-width: 600px), print{.site_header_area{height:58px}}@media (min-width: 600px), print{.site_header_area{height:58px}}@media (min-width: 769px), print{.site_header_area{height:70px}}@media (min-width: 1024px), print{.site_header_area{height:74px}}@media (min-width: 1200px){.site_header_area{height:82px}}@media (min-width: 1700px){.site_header_area{height:88px}}.site_header_area .brand_area{top:8px;margin-left:8px;height:49px;width:260px;transition:width .3s, height .3s}.site_header_area .site_logo_container{top:16px;margin-left:0;width:189px}.site_header_area .menu_button,.site_header_area #modal_close{top:8px;right:8px}@media (min-width: 769px), print{.site_header_area .brand_area{top:10px;margin-left:12px;height:60px;width:313px}.site_header_area .site_logo_container{top:23px;margin-left:13px;width:189px}.site_header_area .menu_button,.site_header_area #modal_close{top:12px;right:12px}}@media (min-width: 1024px), print{.site_header_area .brand_area{top:12px;margin-left:10px;height:54px;width:288px}.site_header_area .site_logo_container{width:120px}}@media (min-width: 1200px){.site_header_area .brand_area{top:14px;margin-left:17px;height:60px;width:338px}.site_header_area .site_logo_container{top:30px;width:210px}}@media (min-width: 1700px){.site_header_area .brand_area{top:14px;margin-left:30px}.site_header_area .site_logo_container{top:30px;width:246px}}@media (min-width: 769px), print{#home:not(.nav_is_fixed) .brand_area{height:69px;width:375px}}@media (min-width: 1024px), print{#home:not(.nav_is_fixed) .brand_area{width:300px;height:55px}}@media (min-width: 1200px){#home:not(.nav_is_fixed) .brand_area{width:368px;height:68px}}@media (min-width: 1700px){#home:not(.nav_is_fixed) .brand_area{width:420px;height:78px}}#home .site_header_area{background-color:transparent}#home.nav_overlay_true .site_header_area,#home.nav_is_fixed .site_header_area{background-color:#5a2017}.site_header_area{background-color:#5a2017;width:100%;position:absolute;z-index:21}.site_header_area.opaque{transition:background-color .5s ease-in-out}.main_feature_present .site_header_area{background-color:transparent}.main_feature_present .site_header_area.opaque{background-color:transparent}#home.nav_is_fixed .site_header_area{z-index:42}.nav_is_fixed .site_header_area{box-shadow:0 4px 4px -2px rgba(0,0,0,0.15);transition:background-color .5s ease-in-out;background-color:#5a2017}.site_header_area .site_header{width:100%;height:100%}.site_header_area .brand_area{position:relative;display:inline-block;z-index:100;background-image:url("https://mars.nasa.gov/assets/logo_nasa_trio@2x.png")}.site_header_area .brand_area .brand1{width:22%}.site_header_area .brand_area .brand2{width:78%}@media (min-width: 769px){.site_header_area .brand_area .brand2{display:block}}.site_header_area .site_logo_container{position:relative;display:inline-block;vertical-align:top;z-index:100}@media (min-width: 769px), print{.site_header_area .site_logo_container:before{content:"";height:110%;background-color:rgba(255,255,255,0.4);width:1px;position:absolute;left:-9px;top:0px}}@media (min-width: 1024px), print{.site_header_area .site_logo_container:before{height:150%;top:1px}}@media (min-width: 1200px){.site_header_area .site_logo_container:before{height:110%;top:-2px}}@media (min-width: 1700px){.site_header_area .site_logo_container:before{top:0px}}.site_header_area .site_logo_container a{display:block}.site_header_area .site_logo_container a:hover{text-decoration:none}.site_header_area .site_logo_container img.site_logo{display:block;position:relative;width:130px;top:3px}@media (min-width: 769px), print{.site_header_area .site_logo_container img.site_logo{width:170px;top:0px}}@media (min-width: 1024px), print{.site_header_area .site_logo_container img.site_logo{width:120px;top:6px}}@media (min-width: 1200px){.site_header_area .site_logo_container img.site_logo{width:188px;top:0}}@media (min-width: 1700px){.site_header_area .site_logo_container img.site_logo{width:215px}}.site_header_area .site_logo_container img.site_logo_truncated{display:none}@media (min-width: 1024px), print{.site_header_area .site_logo_container img.site_logo_truncated{display:block}}@media (min-width: 1200px){.site_header_area .site_logo_container img.site_logo_truncated{display:none}}.site_header_area img.site_logo_black{display:none}.site_header_area form.nav_search{display:inline-block;vertical-align:middle;margin-right:1em}.header_mask{display:none}@media (min-width: 1024px){.header_mask{height:58px;display:block}}@media (min-width: 1024px) and (min-width: 600px), print and (min-width: 1024px){.header_mask{height:58px}}@media (min-width: 1024px) and (min-width: 600px), print and (min-width: 1024px){.header_mask{height:58px}}@media (min-width: 1024px) and (min-width: 769px), print and (min-width: 1024px){.header_mask{height:70px}}@media (min-width: 1024px) and (min-width: 1024px), print and (min-width: 1024px){.header_mask{height:74px}}@media (min-width: 1024px) and (min-width: 1200px){.header_mask{height:82px}}@media (min-width: 1024px) and (min-width: 1700px){.header_mask{height:88px}}@media (max-width: 1023px){#sticky_nav_spacer{height:58px}}@media (max-width: 1023px) and (min-width: 480px){#sticky_nav_spacer{height:58px}}@media (max-width: 1023px) and (min-width: 600px), print and (max-width: 1023px){#sticky_nav_spacer{height:58px}}@media (max-width: 1023px) and (min-width: 769px), print and (max-width: 1023px){#sticky_nav_spacer{height:70px}}@media (max-width: 1023px){.main_feature_present #sticky_nav_spacer{display:none}}.site_header_area .menu_icon{color:transparent;font-size:0}@media (max-width: 1023px){.site_header_area{position:fixed}.fixfixed .site_header_area{position:absolute;box-shadow:none}.nav_is_fixed .site_header_area{box-shadow:0 4px 4px -2px rgba(0,0,0,0.15)}.nav_overlay_true .site_header_area{background-color:#5a2017;transition:none}.site_header_area img.grace_logo_black{display:none}.site_header_area .right_header_container{width:300px}.site_header_area .right_header_container .menu_button{position:absolute;vertical-align:middle;padding:10px;text-decoration:none;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none}.site_header_area .right_header_container .menu_button .menu_icon{display:block;padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") 0 0;background-size:300px}.site_header_area .right_header_container .menu_button .menu_icon:hover,.site_header_area .right_header_container .menu_button .menu_icon.active,.site_header_area .right_header_container .menu_button .menu_icon.current{background-position:0 0}.site_header_area .right_header_container #modal_close{display:none;position:absolute;padding:10px;text-decoration:none;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none}.site_header_area .right_header_container #modal_close .modal_close_icon{display:block;padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -25px 0;background-size:300px}.site_header_area .right_header_container #modal_close .modal_close_icon:hover,.site_header_area .right_header_container #modal_close .modal_close_icon.active,.site_header_area .right_header_container #modal_close .modal_close_icon.current{background-position:-25px 0}.site_header_area .right_header_container form.nav_search{display:none}.site_header_area.menu_open #modal_close{display:inline-block}}@media (min-width: 1024px){.site_header_area{display:block !important}.site_header_area form.nav_search{display:inline-block;max-width:216px}.site_header_area form.nav_search .search_field{width:37px;padding-right:0;padding-left:0;height:34px}.site_header_area form.nav_search .search_open{padding-left:.8em;padding-right:38px}.no-touchevents .nav_is_fixed .site_header_area{bottom:auto;top:0;position:fixed;width:100%;box-shadow:0 4px 4px -2px rgba(0,0,0,0.15);margin-top:0px}}#site_footer{padding:0;background:black;background-size:100%;position:relative;line-height:1.4}@media (min-width: 600px), print{#site_footer{background:#000 url("https://mars.nasa.gov/assets/footer_bg.png") center no-repeat;background-size:cover}}@media (min-width: 1200px){#site_footer{background-position:center 70%}}#site_footer .gradient_line,#site_footer .related.module .gradient_line_module_top,.related.module #site_footer .gradient_line_module_top{margin-left:auto;margin-right:auto;content:" ";width:100%;height:1px;clear:both;background:#a7abd2;background:-moz-linear-gradient(left, rgba(167,171,210,0), #a7abd2, rgba(167,171,210,0));background:-webkit-linear-gradient(left, rgba(167,171,210,0), #a7abd2, rgba(167,171,210,0));background:linear-gradient(left, rgba(167,171,210,0), #a7abd2, rgba(167,171,210,0));width:90%}@media (min-width: 769px), print{#site_footer .gradient_line,#site_footer .related.module .gradient_line_module_top,.related.module #site_footer .gradient_line_module_top{width:50%}}#site_footer .footer_line{display:none;margin-left:auto;margin-right:auto;content:" ";width:85%;height:1px;clear:both;background-color:rgba(255,255,255,0.25)}@media (min-width: 600px), print{#site_footer .footer_line{display:block;width:65%}}.upper_footer{padding:2em 0 0em;width:100%;margin:0 auto}@media (min-width: 600px), print{.upper_footer{padding:4em 0 4em}}@media (min-width: 769px), print{.upper_footer{width:85%}}@media (min-width: 1024px), print{.upper_footer{width:65%}}.upper_footer .share,.upper_footer .footer_newsletter{text-align:center;margin-bottom:2.7em}@media (min-width: 600px), print{.upper_footer .share,.upper_footer .footer_newsletter{margin-bottom:4em;width:100%;float:left}}.upper_footer .share h2,.upper_footer .footer_newsletter h2{font-size:1.8em;font-weight:300;margin-bottom:0.6em;color:#ccdeef;letter-spacing:-.035em}.lower_footer{padding-bottom:4em}@media (min-width: 769px), print{.lower_footer{padding-bottom:9em}}.lower_footer .nav_container{margin:0 auto 1em;position:relative;left:0;width:100%}@media (min-width: 769px), print{.lower_footer .nav_container{padding-top:0.5em}}.lower_footer nav{font-size:1em;text-transform:uppercase;text-align:center;margin-left:auto;margin-right:auto;color:#98c7fc}.lower_footer nav a{padding:0 .4em;font-weight:600;color:#98c7fc;font-size:.85em;text-decoration:none;line-height:2em}@media (min-width: 769px), print{.lower_footer nav a{padding:0 .6em}}.no-touchevents .lower_footer nav a:hover{color:white}.lower_footer nav li{display:inline}.lower_footer nav li:not(:last-child):after{content:"|"}.lower_footer .credits{position:relative;float:none;width:auto;text-align:center}.lower_footer .credits .footer_brands_top,.lower_footer .credits .staff,.lower_footer .credits p{color:#ccdeef;font-weight:700;font-size:1em;text-align:center;line-height:1.3em}@media (min-width: 769px), print{.lower_footer .credits .footer_brands_top,.lower_footer .credits .staff,.lower_footer .credits p{font-size:1em}}.lower_footer .credits .footer_brands_top p{font-weight:400}.lower_footer .credits .footer_brands_top p:last-child{margin-bottom:.4em}.lower_footer .credits .footer_brands{color:#ccdeef;margin-bottom:1em}.lower_footer .credits .footer_brands .caltech{font-weight:300}.lower_footer .credits .staff,.lower_footer .credits .staff p{line-height:1.6em;margin:.3em 0;font-weight:400}.lower_footer .credits a{color:#ccdeef;font-weight:700}.no-touchevents .lower_footer .credits a:hover{color:white}@media (max-width: 1023px){.nav_area{display:none;position:fixed;left:0;width:104%;overflow:hidden;height:100%;min-height:100%;background-color:#5a2017;z-index:10000;top:58px}}@media (max-width: 1023px) and (min-width: 480px){.nav_area{top:58px}}@media (max-width: 1023px) and (min-width: 600px), print and (max-width: 1023px){.nav_area{top:58px}}@media (max-width: 1023px) and (min-width: 769px), print and (max-width: 1023px){.nav_area{top:70px}}#site_nav_container .global_subnav_container{display:none}@media (min-width: 1024px){#site_nav_container .global_subnav_container{display:block !important}}@media (max-width: 1023px){#site_nav_container{width:100%;text-align:center;overflow-y:scroll;padding:0 8.8% 150px 4.8%;height:100%;min-height:100%;-webkit-overflow-scrolling:touch}#site_nav_container .site_nav{display:block}#site_nav_container ul.nav{margin-bottom:2em}#site_nav_container ul.nav&gt;li{display:block;padding:1em 0 0}#site_nav_container ul.nav&gt;li .gradient_line,#site_nav_container ul.nav&gt;li .related.module .gradient_line_module_top,.related.module #site_nav_container ul.nav&gt;li .gradient_line_module_top{margin:1em 0 0 0}#site_nav_container ul.nav&gt;li .arrow_box{padding:20px 20px;width:52px;float:right;cursor:pointer;margin:-0.4em -.8em 0 0;display:block;text-align:center}#site_nav_container ul.nav&gt;li .arrow_box.reverse{transform:rotate(180deg);-ms-filter:"progid:DXImageTransform.Microsoft.Matrix(M11=-1, M12=1.2246063538223773e-16, M21=-1.2246063538223773e-16, M22=-1, SizingMethod='auto expand')"}#site_nav_container ul.nav&gt;li .arrow_box .arrow_down{width:0;height:0;border-left:6px solid rgba(255,255,255,0);border-right:6px solid rgba(255,255,255,0);border-top:8px solid #fff;float:right}#site_nav_container .nav_title{margin-bottom:.3em;display:block;line-height:1.4em;font-weight:700;text-align:left;width:80%}#site_nav_container .nav_title a{font-size:1.2em;color:#FFF;display:block;width:100%;height:100%;padding:.4em .4em .4em 0}#site_nav_container .nav_title a:hover{text-decoration:none}#site_nav_container ul.subnav li{text-align:left}#site_nav_container ul.subnav a{color:#84B0DD;font-size:1em;line-height:1.4em;text-decoration:none;display:block;padding:.4em 0;font-weight:600}#site_nav_container ul.nav&gt;li.admin_site_nav_item .arrow_box .arrow_up,#site_nav_container ul.nav&gt;li.admin_site_nav_item .arrow_box .arrow_down{border-top-color:#F45F5F}.no-touchevents #site_nav_container ul.nav&gt;li.admin_site_nav_item .arrow_box:hover .arrow_up,.no-touchevents #site_nav_container ul.nav&gt;li.admin_site_nav_item .arrow_box:hover .arrow_down{border-top-color:white}#site_nav_container ul.nav&gt;li.admin_site_nav_item .nav_title a,#site_nav_container ul.nav&gt;li.admin_site_nav_item ul.subnav a{color:#F45F5F}.no-touchevents #site_nav_container ul.nav&gt;li.admin_site_nav_item .nav_title a:hover,.no-touchevents #site_nav_container ul.nav&gt;li.admin_site_nav_item ul.subnav a:hover{color:white}#site_nav_container .overlay_search{margin-bottom:2em;width:100%;max-width:320px}#site_nav_container .overlay_search .search_field{width:100%}#site_nav_container .social_nav{color:white;display:block;background-color:#394862;font-size:1.3em;width:100%;border-radius:4px;padding:1.3em 0 1.7em;max-width:320px;margin:0 auto}#site_nav_container .social_nav .nav_title{margin-bottom:1em;text-align:center;width:auto}}@media (min-width: 1024px){.nav_area{width:100%;height:100%;float:right;bottom:0;right:0;position:absolute;text-align:right;z-index:50;display:block !important}}.no-touchevents .nav_is_fixed .fancybox-wrap #sticky_nav_spacer{display:none}.no-touchevents .nav_is_fixed .fancybox-wrap .nav_area{position:relative}@media (min-width: 1024px){#site_nav_container{padding:0;position:relative;display:inline-block !important;width:100%;height:100%;position:relative;bottom:0}}@media (min-width: 1024px) and (min-width: 1024px), print and (min-width: 1024px){#site_nav_container{bottom:0}}@media (min-width: 1024px) and (min-width: 1200px){#site_nav_container{bottom:0}}@media (min-width: 1024px){#site_nav_container .site_nav{width:100%;padding:0;position:relative;overflow-y:visible;min-height:0;height:100%;top:auto;left:auto;padding-top:22px}}@media (min-width: 1024px) and (min-width: 1200px){#site_nav_container .site_nav{padding-top:27px}}@media (min-width: 1024px) and (min-width: 1700px){#site_nav_container .site_nav{padding-right:0.8em}}@media (min-width: 1024px){#site_nav_container ul.nav{margin-bottom:0;display:inline-block;margin-right:.6em}#site_nav_container ul.nav&gt;li{display:block}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.nav&gt;li{display:inline-block;cursor:pointer;border-radius:2px;position:relative}#site_nav_container ul.nav&gt;li:hover{background-color:#9a4739;border-bottom-left-radius:0;border-bottom-right-radius:0}.main_feature_present #site_nav_container ul.nav&gt;li:hover{background-color:rgba(0,0,0,0.5)}.main_feature_present.nav_is_fixed #site_nav_container ul.nav&gt;li:hover{background-color:#9a4739}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.nav&gt;li .global_subnav_container{z-index:20;position:relative}}@media (min-width: 1024px){#site_nav_container ul.nav&gt;li:hover .subnav{display:block}#site_nav_container ul.nav&gt;li .gradient_line,#site_nav_container ul.nav&gt;li .related.module .gradient_line_module_top,.related.module #site_nav_container ul.nav&gt;li .gradient_line_module_top{width:60%;margin-top:1.5em;margin-bottom:1.5em}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.nav&gt;li .gradient_line,#site_nav_container ul.nav&gt;li .related.module .gradient_line_module_top,.related.module #site_nav_container ul.nav&gt;li .gradient_line_module_top{display:none}}@media (min-width: 1024px){#site_nav_container ul.nav&gt;li:last-child{margin-left:14px}#site_nav_container ul.nav&gt;li:last-child:before{content:"";border:1px solid rgba(124,113,110,0.6);position:absolute;height:20px;left:-8px;top:9px}#site_nav_container ul.nav&gt;li:last-child .global_subnav_container&gt;ul.subnav{border-top-left-radius:2px;right:-52px}#site_nav_container .nav_title{margin-bottom:0;display:block;line-height:1.4em;color:white}#site_nav_container .nav_title a,#site_nav_container .nav_title .main_nav_item{display:block;font-size:.88rem;font-weight:600;padding:.5em 6px;color:white}}@media (min-width: 1024px) and (min-width: 1200px){#site_nav_container .nav_title a,#site_nav_container .nav_title .main_nav_item{padding:.5em 0.8em;font-size:.9rem}}@media (min-width: 1024px) and (min-width: 1700px){#site_nav_container .nav_title a,#site_nav_container .nav_title .main_nav_item{padding:.5em 1em}}@media (min-width: 1024px){#site_nav_container .nav_title a:hover,#site_nav_container .nav_title .main_nav_item:hover{text-decoration:none}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.subnav{padding:0.4em 0;margin-bottom:0;min-width:190px;display:none;position:absolute;margin:0;border-bottom-left-radius:2px;border-bottom-right-radius:2px;border-top-right-radius:2px;background-color:#9a4739}.main_feature_present #site_nav_container ul.subnav{background-color:rgba(0,0,0,0.5)}.main_feature_present.nav_is_fixed #site_nav_container ul.subnav{background-color:#9a4739}}@media (min-width: 1024px){#site_nav_container ul.subnav li{text-align:center}}@media (min-width: 1024px) and (min-width: 600px), print and (min-width: 1024px){#site_nav_container ul.subnav li{display:inline-block}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.subnav li{text-align:left;clear:both;display:block}#site_nav_container ul.subnav li:hover{background-color:rgba(0,0,0,0.3)}}@media (min-width: 1024px){#site_nav_container ul.subnav a{color:#84B0DD;font-size:1em;line-height:1.4em;text-decoration:none;display:block;padding:.4em 0;font-weight:600;white-space:nowrap}}@media (min-width: 1024px) and (min-width: 600px), print and (min-width: 1024px){#site_nav_container ul.subnav a{padding:.4em 1em}}@media (min-width: 1024px) and (min-width: 1024px){#site_nav_container ul.subnav a{white-space:normal;font-size:.85em;color:white;padding:.4em 1.1em}}@media (min-width: 1024px){.no-touchevents #site_nav_container ul.subnav a:hover{color:white}#site_nav_container .social_nav{display:none}#site_nav_container li.admin_site_nav_item{background-color:#D94F34}#site_nav_container li.admin_site_nav_item .nav_title a{color:white}#site_nav_container li.admin_site_nav_item:hover .nav_title,#site_nav_container li.admin_site_nav_item.current .nav_title{background-color:#FF7054 !important}#site_nav_container li.admin_site_nav_item:hover .subnav{display:block !important}#site_nav_container li.admin_site_nav_item ul.subnav{border:none;background-color:#D94F34}#site_nav_container li.admin_site_nav_item ul.subnav a{color:white}#site_nav_container li.admin_site_nav_item ul.subnav li{background-color:#D94F34;border:none}#site_nav_container li.admin_site_nav_item ul.subnav li:hover{background-color:#FF7054}}#site_nav_container .nav_title,#site_nav_container ul.subnav a{font-weight:600}#site_footer .sitemap{font-weight:400;z-index:10;position:relative;margin-bottom:2em}@media (min-width: large){#site_footer .sitemap .grid_layout{width:97%}}#site_footer .sitemap_directory{margin-bottom:2em}#site_footer .sitemap_directory .footer_sitemap_item{margin-bottom:1.8em}@media (min-width: 600px), print{#site_footer .sitemap_directory .footer_sitemap_item{margin-bottom:2em}}@media (min-width: 1024px), print{#site_footer .sitemap_directory .footer_sitemap_item{margin-left:10%}}#site_footer .sitemap_title{font-weight:400;text-transform:capitalize;font-size:1em;margin-bottom:.4em}#site_footer .sitemap_title a,#site_footer .sitemap_title .no_link_nav_item{color:white;text-decoration:none}@media (min-width: 600px), print{#site_footer .sitemap_title{font-size:1.1em;margin-bottom:.4em}}@media (min-width: 1024px), print{#site_footer .sitemap_title{font-size:1.1em}}#site_footer .sitemap_block{text-align:center;width:100%}@media (min-width: 600px), print{#site_footer .sitemap_block{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;width:25%;float:left;padding-left:1.66667%;padding-right:1.66667%;text-align:left}}@media (min-width: 1024px), print{#site_footer .sitemap_block{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;width:16.66667%;float:left;padding-left:1.66667%;padding-right:1.66667%}}#site_footer ul.subnav{margin-bottom:1em}#site_footer ul.subnav li{padding-left:1em;text-indent:-1em;margin:0 0 .25em 0}#site_footer ul.subnav a{color:#98c7fc;text-decoration:none;font-size:1em}@media (min-width: 600px), print{#site_footer ul.subnav a{font-size:.85em}}@media (min-width: 1024px), print{#site_footer ul.subnav a{font-size:.95em}}.no-touchevents #site_footer ul.subnav a:hover{color:white}@media (min-width: 600px), print{.main_area_sitemap .grid_layout{width:100%}}.main_area_sitemap .sitemap_directory{padding:2em 0 0}.main_area_sitemap .sitemap_directory .footer_sitemap_item{margin-bottom:1.8em}@media (min-width: 600px), print{.main_area_sitemap .sitemap_directory .footer_sitemap_item{margin-bottom:2em}}.main_area_sitemap .sitemap_block{text-align:center;width:100%}@media (min-width: 600px), print{.main_area_sitemap .sitemap_block{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;width:25%;float:left;padding-left:1.66667%;padding-right:1.66667%;text-align:left}}@media (min-width: 1024px), print{.main_area_sitemap .sitemap_block{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;width:16.66667%;float:left;padding-left:1.66667%;padding-right:1.66667%}}.main_area_sitemap .sitemap_block a{word-wrap:normal}.main_area_sitemap .sitemap_title{margin-top:0}.main_area_sitemap .sitemap_title a,.main_area_sitemap .sitemap_title .no_link_nav_item{color:#222}.main_area_sitemap .subnav a{display:block}@media (min-width: 600px), print{.main_area_sitemap .subnav a{padding-left:1em;text-indent:-1em;margin:.1em 0}}.social_icons{display:block}.social_icons .icon{width:44px !important;height:44px !important;display:inline-block;overflow:hidden}.social_icons .icon+.icon{margin-left:.7em}@media (min-width: 769px), print{.social_icons .icon+.icon{margin-left:.9em}}.social_icons .icon img{opacity:1 !important;height:100%;max-width:none}.triple_teaser .social_icons{max-width:188px;white-space:nowrap}@media (min-width: 769px), print{.triple_teaser .social_icons{max-width:none}}.triple_teaser .social_icons .icon{width:44px;height:44px}.triple_teaser .social_icons .icon+.icon{margin-left:.7em}@media (min-width: 600px), print{.triple_teaser .social_icons .icon{width:38px;height:38px}.triple_teaser .social_icons .icon+.icon{margin-left:.4em;margin-left:calc((100% - 152px)/3)}}@media (min-width: 769px), print{.triple_teaser .social_icons .icon{width:44px;height:44px}.triple_teaser .social_icons .icon+.icon{margin-left:.8em}}.addthis_default_style .at300b,.addthis_default_style .at300bo,.addthis_default_style .at300m{padding:0 !important;float:none !important}#_atssh{display:none}#at4-share,#at4-soc{top:60%;bottom:auto}html,html a,select,input,button{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}html.no-touchevents{text-rendering:optimizeLegibility}html.no-touchevents html a,html.no-touchevents select,html.no-touchevents input,html.no-touchevents button{text-rendering:optimizeLegibility}input.placeholder,textarea.placeholder{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;text-rendering:optimizeLegibility}input:-moz-placeholder,textarea:-moz-placeholder{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;text-rendering:optimizeLegibility}input::-moz-placeholder,textarea::-moz-placeholder{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;text-rendering:optimizeLegibility}input::-webkit-input-placeholder,textarea::-webkit-input-placeholder{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;text-rendering:optimizeLegibility}input:-ms-input-placeholder,textarea:-ms-input-placeholder{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;text-rendering:optimizeLegibility}html,button,input,select,textarea{font-family:"Montserrat",Helvetica,Arial,sans-serif;color:#222}html{min-height:100%}body{font-family:"Montserrat",Helvetica,Arial,sans-serif;font-weight:300;font-size:96%;line-height:1.4;min-height:100%;position:relative;background-color:transparent}body.noscroll{overflow-y:hidden}@media (min-width: 600px), print{body{font-size:98%}}@media (min-width: 769px), print{body{font-size:100%}}@media (min-width: 1024px), print{body{font-size:102%}}@media (min-width: 1200px){body{font-size:104%}}h1,h2,h3,h4,h5{line-height:1.2em}h1{letter-spacing:-.03em}h2{letter-spacing:-.03em}h3{letter-spacing:-.02em}h4{letter-spacing:-.02em}h1,h2,h3,h4,h5{margin:0}img{width:100%}img,embed,object,video{max-width:100%}.jwplayer video{max-width:none}.jw-rightclick{display:none !important}i{font-style:italic}strong{font-weight:700}p{margin:1em 0;font-size:100%}.gradient_line,.related.module .gradient_line_module_top{margin-left:auto;margin-right:auto;content:" ";width:100%;height:1px;clear:both;background:#b76b5f;background:-moz-linear-gradient(left, rgba(183,107,95,0), #b76b5f, rgba(183,107,95,0));background:-webkit-linear-gradient(left, rgba(183,107,95,0), #b76b5f, rgba(183,107,95,0));background:linear-gradient(left, rgba(183,107,95,0), #b76b5f, rgba(183,107,95,0))}.gradient_line_extra_margin{margin-left:auto;margin-right:auto;content:" ";width:100%;height:1px;clear:both;background:#BEBEBE;background:-moz-linear-gradient(left, rgba(190,190,190,0), #bebebe, rgba(190,190,190,0));background:-webkit-linear-gradient(left, rgba(190,190,190,0), #bebebe, rgba(190,190,190,0));background:linear-gradient(left, rgba(190,190,190,0), #bebebe, rgba(190,190,190,0));margin:2em 0}@media (min-width: 769px), print{.gradient_line_extra_margin{margin:3em 0}}.module_title,.main_carousel.module .carousel_header .carousel_title,.media_feature_title,.sitemap_title,.nav_title,.article_title,.sidebar_title,#secondary_column .related_content_module .module_title,#secondary_column .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #secondary_column .related_content_module .carousel_title,.right_col .related_content_module .module_title,.right_col .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .right_col .related_content_module .carousel_title,.rollover_title{letter-spacing:-.02em}.module_title,.main_carousel.module .carousel_header .carousel_title{letter-spacing:-.02em}.rollover_title{font-size:2.34em;margin-bottom:0em}@media (min-width: 600px), print{.rollover_title{font-size:2.7em;margin-bottom:0em}}@media (min-width: 769px), print{.rollover_title{font-size:3.06em;margin-bottom:0em}}@media (min-width: 1024px), print{.rollover_title{font-size:3.24em;margin-bottom:0em}}@media (min-width: 1200px){.rollover_title{font-size:3.42em;margin-bottom:0em}}.content_title{letter-spacing:0;font-weight:600}.module_title,.main_carousel.module .carousel_header .carousel_title{font-size:1.69em;margin-bottom:.35em;text-align:center;font-weight:600}@media (min-width: 600px), print{.module_title,.main_carousel.module .carousel_header .carousel_title{font-size:1.95em;margin-bottom:.63em}}@media (min-width: 769px), print{.module_title,.main_carousel.module .carousel_header .carousel_title{font-size:2.21em;margin-bottom:.91em}}@media (min-width: 1024px), print{.module_title,.main_carousel.module .carousel_header .carousel_title{font-size:2.34em;margin-bottom:1.015em}}@media (min-width: 1200px){.module_title,.main_carousel.module .carousel_header .carousel_title{font-size:2.47em;margin-bottom:1.12em}}@media (min-width: 600px), print{.grid_gallery .module_title,.grid_gallery .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .grid_gallery .carousel_title{text-align:left;width:80%}}.module_title_small,.double_teaser .module_title,.double_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .double_teaser .carousel_title{font-size:1.4em}@media (min-width: 600px), print{.module_title_small,.double_teaser .module_title,.double_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .double_teaser .carousel_title{font-size:1.8em;margin-bottom:.85em}}.filter_bar .module_title_small,.filter_bar .double_teaser .module_title,.filter_bar .double_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .filter_bar .double_teaser .carousel_title{text-align:left;width:90%}@media (min-width: 600px), print{.filter_bar .module_title_small,.filter_bar .double_teaser .module_title,.filter_bar .double_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .filter_bar .double_teaser .carousel_title{text-align:center}}.category_title{font-size:.9em;font-weight:500;color:#f08d77;text-transform:uppercase;margin-bottom:6px}.multimedia_teaser .category_title{font-size:.8em}.primary_media_feature .media_feature_title{font-size:1.43em;margin-bottom:0em;font-weight:400;color:white}@media (min-width: 600px), print{.primary_media_feature .media_feature_title{font-size:1.65em;margin-bottom:0em}}@media (min-width: 769px), print{.primary_media_feature .media_feature_title{font-size:1.87em;margin-bottom:0em}}@media (min-width: 1024px), print{.primary_media_feature .media_feature_title{font-size:1.98em;margin-bottom:0em}}@media (min-width: 1200px){.primary_media_feature .media_feature_title{font-size:2.09em;margin-bottom:0em}}.image_of_the_day .media_feature_title{font-size:1.43em;margin-bottom:0em;font-weight:600;color:white}@media (min-width: 600px), print{.image_of_the_day .media_feature_title{font-size:1.65em;margin-bottom:0em}}@media (min-width: 769px), print{.image_of_the_day .media_feature_title{font-size:1.87em;margin-bottom:0em}}@media (min-width: 1024px), print{.image_of_the_day .media_feature_title{font-size:1.98em;margin-bottom:0em}}@media (min-width: 1200px){.image_of_the_day .media_feature_title{font-size:2.09em;margin-bottom:0em}}.multimedia_module_gallery .media_feature_title{font-size:1.43em;margin-bottom:0em;color:white;font-weight:600}@media (min-width: 600px), print{.multimedia_module_gallery .media_feature_title{font-size:1.65em;margin-bottom:0em}}@media (min-width: 769px), print{.multimedia_module_gallery .media_feature_title{font-size:1.87em;margin-bottom:0em}}@media (min-width: 1024px), print{.multimedia_module_gallery .media_feature_title{font-size:1.98em;margin-bottom:0em}}@media (min-width: 1200px){.multimedia_module_gallery .media_feature_title{font-size:2.09em;margin-bottom:0em}}.article_title{font-size:1.82em;margin-bottom:0em;font-weight:600}@media (min-width: 600px), print{.article_title{font-size:2.1em;margin-bottom:0em}}@media (min-width: 769px), print{.article_title{font-size:2.38em;margin-bottom:0em}}@media (min-width: 1024px), print{.article_title{font-size:2.52em;margin-bottom:0em}}@media (min-width: 1200px){.article_title{font-size:2.66em;margin-bottom:0em}}.magic_shell_title,#iframe_overlay .magic_shell_title{font-family:WhitneyCondensed-Bold,Helvetica,Arial,sans-serif;background-color:#000;font-size:2.6em;font-weight:normal;padding:.9em .5em;text-align:center;line-height:.8}@media (min-width: 600px), print{.magic_shell_title,#iframe_overlay .magic_shell_title{padding:.9em .5em;text-align:left;font-size:2.6em}}.magic_shell_title .parent_title,#iframe_overlay .magic_shell_title .parent_title{display:block}.magic_shell_title .parent_title a,#iframe_overlay .magic_shell_title .parent_title a{color:#f08d77;transition:color 400ms}.magic_shell_title .parent_title a:hover,#iframe_overlay .magic_shell_title .parent_title a:hover{text-decoration:none;color:white}@media (min-width: 600px), print{.magic_shell_title .parent_title,#iframe_overlay .magic_shell_title .parent_title{display:inline;margin-right:0.1em}}.magic_shell_title .article_title,#iframe_overlay .magic_shell_title .article_title{color:#FFF;font-weight:normal}.magic_shell_title .article_title,#iframe_overlay .magic_shell_title .article_title,.magic_shell_title .parent_title,#iframe_overlay .magic_shell_title .parent_title{font-size:.7em;text-transform:uppercase;letter-spacing:normal}.sidebar_title,#secondary_column .related_content_module .module_title,#secondary_column .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #secondary_column .related_content_module .carousel_title,.right_col .related_content_module .module_title,.right_col .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .right_col .related_content_module .carousel_title{font-size:1.55em;margin-bottom:0.6em;font-weight:700;margin-left:-1px}.links_module a{font-size:1em;cursor:pointer}.module{padding:2.5em 0 2.2em;position:relative}@media (min-width: 769px), print{.module{padding:4.8em 0 5em}}.grid_layout{max-width:100%;margin-left:auto;margin-right:auto;width:95%}.grid_layout:after{content:" ";display:block;clear:both}@media (min-width: 600px), print{.grid_layout{max-width:100%;margin-left:auto;margin-right:auto;width:95%}.grid_layout:after{content:" ";display:block;clear:both}}@media (min-width: 769px), print{.grid_layout{max-width:100%;margin-left:auto;margin-right:auto;width:90%}.grid_layout:after{content:" ";display:block;clear:both}}@media (min-width: 1024px), print{.grid_layout{max-width:1200px;width:97%}.content_page .grid_layout{width:90%}}@media (max-width: 480px){.suggested_features .grid_layout,.news_teaser .grid_layout,.carousel_teaser .grid_layout{width:100%}.suggested_features .grid_layout header,.news_teaser .grid_layout header,.carousel_teaser .grid_layout header{margin-left:auto;margin-right:auto;width:95%}.suggested_features .grid_layout footer,.news_teaser .grid_layout footer,.carousel_teaser .grid_layout footer{margin-left:auto;margin-right:auto;width:95%}}.gradient_container_top,.gradient_container_bottom,.white_gradient_container_bottom{height:200px;width:100%;position:absolute;z-index:1}.homepage_carousel .gradient_container_top,.homepage_carousel .gradient_container_bottom,.homepage_carousel .white_gradient_container_bottom{z-index:7}.gradient_container_left{height:100%;width:70%;position:absolute;z-index:1}.gradient_container_top{background:-owg-linear-gradient(rgba(0,0,0,0.6), transparent);background:-webkit-linear-gradient(rgba(0,0,0,0.6), transparent);background:-moz-linear-gradient(rgba(0,0,0,0.6), transparent);background:-o-linear-gradient(rgba(0,0,0,0.6), transparent);background:linear-gradient(rgba(0,0,0,0.6), transparent);pointer-events:none;top:0}.gradient_container_left{background:-owg-linear-gradient(to right, rgba(0,0,0,0.6), transparent);background:-webkit-linear-gradient(to right, rgba(0,0,0,0.6), transparent);background:-moz-linear-gradient(to right, rgba(0,0,0,0.6), transparent);background:-o-linear-gradient(to right, rgba(0,0,0,0.6), transparent);background:linear-gradient(to right, rgba(0,0,0,0.6), transparent);left:0;top:0}.gradient_container_bottom{background:-owg-linear-gradient(transparent, rgba(0,0,0,0.7));background:-webkit-linear-gradient(transparent, rgba(0,0,0,0.7));background:-moz-linear-gradient(transparent, rgba(0,0,0,0.7));background:-o-linear-gradient(transparent, rgba(0,0,0,0.7));background:linear-gradient(transparent, rgba(0,0,0,0.7));pointer-events:none;bottom:0}.white_gradient_container_bottom{background:url("https://mars.nasa.gov/assets/white_gradient.png") repeat-x bottom left;bottom:0;height:100px;pointer-events:none}.gradient_bottom_grid{background-image:linear-gradient(to bottom, transparent 0%, #000 30%, #000 100%)}.grid_gallery .gallery_header{margin-bottom:2em}@media (min-width: 769px), print{.grid_gallery .gallery_header{margin-bottom:3em}}.grid_gallery .gallery_header .module_title,.grid_gallery .gallery_header .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .grid_gallery .gallery_header .carousel_title{margin-bottom:0.5em;text-align:left}.grid_gallery .list_date{font-size:.9em;margin-bottom:.4em;color:#5A5A5A}.grid_gallery.grid_view{background:white}.grid_gallery.grid_view .content_title{letter-spacing:-.03em;display:none}.grid_gallery.grid_view .image_and_description_container{min-height:0}.grid_gallery.grid_view .article_teaser_body{display:none}.grid_gallery.grid_view .list_date{display:none}.grid_gallery.grid_view .list_image{width:100%;float:none;margin:0}.grid_gallery.grid_view .bottom_gradient{color:#222;display:block;position:relative;margin-top:0.3rem;padding-bottom:0.4rem;text-align:left;min-height:52px}@media (min-width: 769px), print{.grid_gallery.grid_view .bottom_gradient{margin-top:.5rem;min-height:85px}}.grid_gallery.grid_view .bottom_gradient div{text-align:left}.grid_gallery.grid_view .bottom_gradient h3{font-weight:500;font-size:1em}.grid_gallery.grid_view li.slide{margin-bottom:.84034%;width:49.57983%;float:left}.grid_gallery.grid_view li.slide:nth-child(2n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery.grid_view li.slide:nth-child(2n+2){margin-left:50.42017%;margin-right:-100%;clear:none}@media (min-width: 600px), print{.grid_gallery.grid_view li.slide{margin-bottom:.84034%;width:32.77311%;float:left}.grid_gallery.grid_view li.slide:nth-child(3n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery.grid_view li.slide:nth-child(3n+2){margin-left:33.61345%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(3n+3){margin-left:67.22689%;margin-right:-100%;clear:none}}@media (min-width: 769px), print{.grid_gallery.grid_view li.slide{width:24.36975%;float:left}.grid_gallery.grid_view li.slide:nth-child(4n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery.grid_view li.slide:nth-child(4n+2){margin-left:25.21008%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(4n+3){margin-left:50.42017%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(4n+4){margin-left:75.63025%;margin-right:-100%;clear:none}}@media (min-width: 1200px){.grid_gallery.grid_view li.slide{width:19.32773%;float:left}.grid_gallery.grid_view li.slide:nth-child(5n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery.grid_view li.slide:nth-child(5n+2){margin-left:20.16807%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(5n+3){margin-left:40.33613%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(5n+4){margin-left:60.5042%;margin-right:-100%;clear:none}.grid_gallery.grid_view li.slide:nth-child(5n+5){margin-left:80.67227%;margin-right:-100%;clear:none}}.grid_gallery.grid_view li.slide a{text-decoration:none}.grid_gallery.list_view .list_image{float:right;margin-left:4%;margin-bottom:.5em;width:32%}@media (min-width: 600px), print{.grid_gallery.list_view .list_image{margin-left:0;margin-bottom:0;width:23.07692%;float:left;margin-right:2.5641%}}@media (min-width: 769px), print{.grid_gallery.list_view .list_image{width:23.72881%;float:left;margin-right:1.69492%}}@media (min-width: 1024px), print{.grid_gallery.list_view .list_image{width:23.72881%;float:left;margin-right:1.69492%}}.grid_gallery.list_view .list_text{width:auto}@media (min-width: 600px), print{.grid_gallery.list_view .list_text{width:74.35897%;float:right;margin-right:0}}@media (min-width: 769px), print{.grid_gallery.list_view .list_text{width:74.57627%;float:right;margin-right:0}}@media (min-width: 1024px), print{.grid_gallery.list_view .list_text{width:66.10169%;float:left;margin-right:1.69492%}}.grid_gallery.list_view .content_title a{text-decoration:none;cursor:pointer;color:#222}.grid_gallery.list_view .content_title a:hover{text-decoration:underline}.grid_gallery.list_view .content_title{display:block;font-size:1.17em;margin-bottom:.1em;margin-bottom:.2em;font-weight:700;color:#222;letter-spacing:-.035em}@media (min-width: 600px), print{.grid_gallery.list_view .content_title{font-size:1.35em;margin-bottom:.18em}}@media (min-width: 769px), print{.grid_gallery.list_view .content_title{font-size:1.53em;margin-bottom:.26em}}@media (min-width: 1024px), print{.grid_gallery.list_view .content_title{font-size:1.62em;margin-bottom:.29em}}@media (min-width: 1200px){.grid_gallery.list_view .content_title{font-size:1.71em;margin-bottom:.32em}}.grid_gallery.list_view .bottom_gradient{display:none}@media (min-width: 1024px), print{.grid_gallery.list_view .article_teaser_body{font-size:1.1em}}.grid_gallery.list_view li.slide:first-child{border-top:1px solid #CCC}.grid_gallery.list_view li.slide{border-bottom:1px solid #CCC;padding:1.2em 0}.grid_gallery.list_view li.slide a{text-decoration:none;cursor:pointer}.view_selectors{position:relative;margin:0 auto;text-align:center;width:106px;text-align:right}@media (min-width: 769px), print{.view_selectors{position:absolute;right:0;top:0;height:100%}}.view_selectors .nav_item{display:inline-block;position:relative;background-repeat:no-repeat;width:50px;height:50px;cursor:pointer;background-image:url("https://mars.nasa.gov/assets/grid_list_icon@2x.png");background-size:125px;background-color:#eef2f6;border-radius:50%;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none}.view_selectors .nav_item.list_icon{background-position:-12px -62px}.no-touchevents .view_selectors .nav_item.list_icon:hover{background-position:-12px -12px}.list_view .view_selectors .nav_item.list_icon{background-position:-12px -12px}.view_selectors .nav_item.grid_icon{background-position:-62px -62px}.no-touchevents .view_selectors .nav_item.grid_icon:hover{background-position:-62px -12px}.grid_view .view_selectors .nav_item.grid_icon{background-position:-62px -12px}.grid_gallery#more_section .module_title,.grid_gallery#more_section .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .grid_gallery#more_section .carousel_title{text-align:center;width:100%}.grid_gallery#more_section li.slide{margin-bottom:1.69492%;width:49.15254%;float:left}.grid_gallery#more_section li.slide:nth-child(2n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery#more_section li.slide:nth-child(2n+2){margin-left:50.84746%;margin-right:-100%;clear:none}@media (min-width: 1200px){.grid_gallery#more_section li.slide{width:32.20339%;float:left}.grid_gallery#more_section li.slide:nth-child(3n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.grid_gallery#more_section li.slide:nth-child(3n+2){margin-left:33.89831%;margin-right:-100%;clear:none}.grid_gallery#more_section li.slide:nth-child(3n+3){margin-left:67.79661%;margin-right:-100%;clear:none}}.grid_gallery#more_section li.slide .image_and_description_container{position:relative}.grid_gallery#more_section li.slide a.slide_title{padding-top:.6em;display:block;color:#222;font-weight:400}.grid_gallery#more_section li.slide:hover a.slide_title{color:#366599}.wysiwyg_content ul,ol{margin-left:1.6em}.feature_pages .wysiwyg_content ol,.feature_pages .wysiwyg_content ul{list-style-position:inside}#secondary_column ul,ol{margin-left:1.2em}.wysiwyg_content ul,#secondary_column ul{margin-left:1.6em;list-style-type:disc;list-style-position:outside}.wysiwyg_content ul ul,#secondary_column ul ul{list-style-type:circle;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ul ul ul,#secondary_column ul ul ul{list-style-type:square;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ul ul ul ul,#secondary_column ul ul ul ul{list-style-type:disc;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ol,#secondary_column ol{margin-left:1.6em;list-style-type:decimal;list-style-position:outside}.wysiwyg_content ol ol,#secondary_column ol ol{list-style-type:decimal;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ol ol ol,#secondary_column ol ol ol{list-style-type:decimal;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ol ol ol ol,#secondary_column ol ol ol ol{list-style-type:decimal;margin-left:1.6em;margin-top:.5em}.wysiwyg_content ol,.wysiwyg_content ul,#secondary_column ol,#secondary_column ul{margin-bottom:2em}.wysiwyg_content ol:last-child,.wysiwyg_content ul:last-child,#secondary_column ol:last-child,#secondary_column ul:last-child{margin-bottom:0}.wysiwyg_content ol li,.wysiwyg_content ul li,#secondary_column ol li,#secondary_column ul li{margin-bottom:.5em}.wysiwyg_content .item_list_module,.wysiwyg_content .item_list,.wysiwyg_content .list_sublist,.wysiwyg_content .footnotes ul,.wysiwyg_content .sidebar_gallery,.wysiwyg_content .related_items,.wysiwyg_content .sitemap_directory ul,.wysiwyg_content .list_view_module ul,.wysiwyg_content .faq_topics ul,.wysiwyg_content .item_grid,.wysiwyg_content .related_content_module ul,.wysiwyg_content .sig_events_module ul,.wysiwyg_content ul.detailed_def_nav,#secondary_column .item_list_module,#secondary_column .item_list,#secondary_column .list_sublist,#secondary_column .footnotes ul,#secondary_column .sidebar_gallery,#secondary_column .related_items,#secondary_column .sitemap_directory ul,#secondary_column .list_view_module ul,#secondary_column .faq_topics ul,#secondary_column .item_grid,#secondary_column .related_content_module ul,#secondary_column .sig_events_module ul,#secondary_column ul.detailed_def_nav{margin-left:0;list-style-type:none;list-style-position:inside}.wysiwyg_content .item_list_module li,.wysiwyg_content .item_list li,.wysiwyg_content .list_sublist li,.wysiwyg_content .footnotes ul li,.wysiwyg_content .sidebar_gallery li,.wysiwyg_content .related_items li,.wysiwyg_content .sitemap_directory ul li,.wysiwyg_content .list_view_module ul li,.wysiwyg_content .faq_topics ul li,.wysiwyg_content .item_grid li,.wysiwyg_content .related_content_module ul li,.wysiwyg_content .sig_events_module ul li,.wysiwyg_content ul.detailed_def_nav li,#secondary_column .item_list_module li,#secondary_column .item_list li,#secondary_column .list_sublist li,#secondary_column .footnotes ul li,#secondary_column .sidebar_gallery li,#secondary_column .related_items li,#secondary_column .sitemap_directory ul li,#secondary_column .list_view_module ul li,#secondary_column .faq_topics ul li,#secondary_column .item_grid li,#secondary_column .related_content_module ul li,#secondary_column .sig_events_module ul li,#secondary_column ul.detailed_def_nav li{margin-bottom:0}.module header{margin-bottom:1em;position:relative}.module footer{text-align:center;position:relative}.module footer a.detail_link{text-transform:uppercase;font-size:.9em;font-weight:400}.module .module_title,.main_carousel.module .carousel_header .carousel_title{font-weight:300;color:#6d3007}.multimedia_teaser{-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none;overflow:hidden}#secondary_column aside .multimedia_teaser{position:relative}#secondary_column aside .multimedia_teaser .text{position:absolute;width:100%;text-align:center;padding:0 1.4em 2em;bottom:0}#secondary_column aside .multimedia_teaser .text .category_title,#secondary_column aside .multimedia_teaser .text .media_feature_title{color:white}.multimedia_teaser{-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none;overflow:hidden}.multimedia_teaser .util-carousel{margin-bottom:2em;width:190%}@media (min-width: 480px){.multimedia_teaser .util-carousel{width:90%}}@media (min-width: 769px), print{.multimedia_teaser .util-carousel{margin-bottom:3em}}.suggested_features.module{-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none;background-color:#eef2f6}.related.module{-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none;padding-top:1em}.related.module .module_title,.related.module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .related.module .carousel_title{text-align:left;font-size:2em}.related.module .gradient_line_module_top{margin:0 0 2em}.carousel_teaser.related .module_title,.carousel_teaser.related .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .carousel_teaser.related .carousel_title{text-align:center}@media (min-width: 600px), print{.carousel_teaser.related .module_title,.carousel_teaser.related .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .carousel_teaser.related .carousel_title{text-align:left;width:88%;margin-left:auto;margin-right:auto}}@media (min-width: 769px), print{.carousel_teaser.related .module_title,.carousel_teaser.related .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .carousel_teaser.related .carousel_title{width:88.5%}}@media (min-width: 1024px), print{.carousel_teaser.related .module_title,.carousel_teaser.related .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .carousel_teaser.related .carousel_title{width:89%}}section.site_teaser .img_col{width:100%;margin-bottom:1.5em}@media (min-width: 600px), print{section.site_teaser .img_col{width:40.78947%;float:left;margin-right:5.26316%;margin-bottom:0}}section.site_teaser .text_col{width:100%}@media (min-width: 600px), print{section.site_teaser .text_col{width:53.94737%;float:left;margin-right:5.26316%;float:right;margin-right:0}}section.site_teaser .text_col .category_title{font-size:0.9em}section.site_teaser .text_col p{margin:1em 0 1.7em}section.site_teaser .site_teaser_caption{margin:.5em 1em 0 0;text-align:right;font-size:.8em}section.site_teaser footer{text-align:center}@media (min-width: 600px), print{section.site_teaser footer{text-align:left}}section.site_teaser .button,section.site_teaser .outline_button,section.site_teaser .primary_media_feature .floating_text_area .button,.primary_media_feature .floating_text_area section.site_teaser .button{padding:0.8em 1.2em}section.more_bar{text-align:center;background-color:#4d91a6;color:black;height:36px;cursor:pointer;position:relative}section.more_bar .title,section.more_bar .arrow_down{display:inline-block;vertical-align:middle;margin-top:6px}section.more_bar .arrow_down{padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -50px -125px;background-size:300px}section.more_bar .arrow_down:hover,section.more_bar .arrow_down.active,section.more_bar .arrow_down.current{background-position:-50px -125px}.inline_dashboard_item{display:inline}.inline_dashboard_item div{display:inline-block}.homepage_carousel .floating_text_area{width:100%;padding:1.4em;bottom:120px;text-align:center;margin-left:auto;margin-right:auto;color:white}@media (min-width: 769px), print{.homepage_carousel .floating_text_area{bottom:calc(125px + 3em)}}.homepage_carousel .floating_text_area .description{display:block;max-height:130px;overflow-y:auto;padding:0 1.4em;color:#ffffff;font-weight:300}.homepage_carousel .floating_text_area .description a{color:#69B9FF}@media (min-width: 769px), print{.homepage_carousel .floating_text_area .description{display:block;line-height:1.4em;padding:0;max-height:none;overflow:hidden}}.touchevents .homepage_carousel .floating_text_area .description{display:none}.no-touchevents .homepage_carousel .floating_text_area .description{display:none}@media (min-width: 769px), print{.no-touchevents .homepage_carousel .floating_text_area .description{display:block !important}}.homepage_carousel .floating_text_area .description .detail_link{display:inline-block;color:#69B9FF;text-transform:none}.homepage_carousel .floating_text_area .description .detail_link:hover{text-decoration:none;color:#ffffff}@media (min-width: 769px), print{.homepage_carousel .floating_text_area .description .detail_link{display:none}}@media (orientation: landscape){.homepage_carousel .floating_text_area .description{display:none !important}}.homepage_carousel .floating_text_area footer{margin:0}@media (min-width: 769px), print{.homepage_carousel .floating_text_area footer{margin:1.6em 0 0}}.homepage_carousel .floating_text_area .media_feature_title{color:white;margin-bottom:.4em;font-size:1.6em;width:70%;margin-left:auto;margin-right:auto;position:relative;font-weight:400}.homepage_carousel .floating_text_area .media_feature_title a{color:white;text-decoration:none}@media (min-width: 600px), print{.homepage_carousel .floating_text_area .media_feature_title{font-size:2em;width:80%}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area .media_feature_title{font-size:1.5em;margin-bottom:.4em;width:100%}}@media (min-width: 1024px), print{.homepage_carousel .floating_text_area .media_feature_title{font-size:1.6em}}@media (min-width: 1200px){.homepage_carousel .floating_text_area .media_feature_title{font-size:1.8em}}@media (min-width: 1700px){.homepage_carousel .floating_text_area .media_feature_title{font-size:1.9em}}.homepage_carousel .floating_text_area .media_feature_title span.arrow{background:url("https://mars.nasa.gov/assets/arrow_up_white_@2x.png") center no-repeat;position:absolute;right:-25%;margin-top:-0.2em;background-size:12px;height:44px;width:44px;bottom:-9px}@media (min-width: 600px), print{.homepage_carousel .floating_text_area .media_feature_title span.arrow{margin-top:0;right:-10%}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area .media_feature_title span.arrow{display:none}}@media (orientation: landscape){.homepage_carousel .floating_text_area .media_feature_title span.arrow{display:none}}.homepage_carousel .floating_text_area .button,.homepage_carousel .floating_text_area .outline_button,.homepage_carousel .floating_text_area .button:hover,.homepage_carousel .floating_text_area .outline_button:hover{display:none;background-color:#3b788b;text-transform:none;font-size:16px;font-weight:400;border-radius:3px;padding:9px 16px}@media (min-width: 769px), print{.homepage_carousel .floating_text_area .button,.homepage_carousel .floating_text_area .outline_button,.homepage_carousel .floating_text_area .button:hover,.homepage_carousel .floating_text_area .outline_button:hover{display:inline-block !important;color:white !important}}.no-touchevents .homepage_carousel .floating_text_area .button,.no-touchevents .homepage_carousel .floating_text_area .outline_button{transition:background 300ms}.no-touchevents .homepage_carousel .floating_text_area .button:hover,.no-touchevents .homepage_carousel .floating_text_area .outline_button:hover{background-color:#569bb1}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.expandable .media_feature_title{font-size:1.5em}}@media (min-width: 1024px), print{.homepage_carousel .floating_text_area.expandable .media_feature_title{font-size:1.7em}}@media (min-width: 1200px){.homepage_carousel .floating_text_area.expandable .media_feature_title{font-size:2.0em}}@media (min-width: 1700px){.homepage_carousel .floating_text_area.expandable .media_feature_title{font-size:2.2em}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area{text-align:left;padding:1.4em;margin:0}.homepage_carousel .floating_text_area.no-box{position:relative;top:40%;transform:translateY(-50%);width:45%;max-width:500px}.homepage_carousel .floating_text_area.no-box.left{left:5%}.homepage_carousel .floating_text_area.no-box.right{left:51%}.homepage_carousel .floating_text_area.box{position:absolute;background-color:rgba(0,0,0,0.6);width:400px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 1024px), print and (min-width: 769px), print{.homepage_carousel .floating_text_area.box{width:480px}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.homepage_carousel .floating_text_area.box{width:530px}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.box.left{left:8%}.homepage_carousel .floating_text_area.box.right{right:8%}.homepage_carousel .floating_text_area.expandable,.homepage_carousel .floating_text_area.expandable_light{transition:background-color .5s ease-out;width:40%;top:auto}.homepage_carousel .floating_text_area.expandable footer,.homepage_carousel .floating_text_area.expandable_light footer{margin-bottom:1em}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.homepage_carousel .floating_text_area.expandable,.homepage_carousel .floating_text_area.expandable_light{width:415px}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.homepage_carousel .floating_text_area.expandable,.homepage_carousel .floating_text_area.expandable_light{width:480px}}@media (min-width: 769px) and (min-width: 1700px), print and (min-width: 1700px){.homepage_carousel .floating_text_area.expandable,.homepage_carousel .floating_text_area.expandable_light{width:600px}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.expandable.left,.homepage_carousel .floating_text_area.expandable_light.left{left:8%}}@media (min-width: 769px) and (min-width: 1700px), print and (min-width: 1700px){.homepage_carousel .floating_text_area.expandable.left,.homepage_carousel .floating_text_area.expandable_light.left{left:4%}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.expandable.right,.homepage_carousel .floating_text_area.expandable_light.right{right:8%}}@media (min-width: 769px) and (min-width: 1700px), print and (min-width: 1700px){.homepage_carousel .floating_text_area.expandable.right,.homepage_carousel .floating_text_area.expandable_light.right{right:4%}}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.expandable .description,.homepage_carousel .floating_text_area.expandable_light .description{max-height:0;overflow:hidden;transition:all .7s}.homepage_carousel .floating_text_area.expandable .media_feature_title:after,.homepage_carousel .floating_text_area.expandable_light .media_feature_title:after{content:url("https://mars.nasa.gov/assets/arrow_down_prompt.png");transition:opacity .25s;position:relative;top:-4px;left:10px;opacity:1}.homepage_carousel .floating_text_area.expandable .media_feature_title span.arrow,.homepage_carousel .floating_text_area.expandable_light .media_feature_title span.arrow{display:none}.homepage_carousel .floating_text_area.expandable:hover:before,.homepage_carousel .floating_text_area.expandable_light:hover:before{opacity:0}.homepage_carousel .floating_text_area.expandable:hover .description,.homepage_carousel .floating_text_area.expandable_light:hover .description{max-height:400px}.homepage_carousel .floating_text_area.expandable:hover .media_feature_title:after,.homepage_carousel .floating_text_area.expandable_light:hover .media_feature_title:after{opacity:0}.homepage_carousel .floating_text_area.expandable{background-color:rgba(0,0,0,0.6)}.homepage_carousel .floating_text_area.expandable_light{background-color:rgba(255,255,255,0.9)}.homepage_carousel .floating_text_area.expandable_light .media_feature_title,.homepage_carousel .floating_text_area.expandable_light .description{color:#452520}.homepage_carousel .floating_text_area.expandable_light .category_title{color:#d63e1c}.homepage_carousel .floating_text_area.expandable_light .media_feature_title:after{content:url("https://mars.nasa.gov/assets/arrow_down_light.png")}}.homepage_carousel .floating_text_area.open span.arrow{transform:rotate(180deg)}@media (min-width: 769px), print{.homepage_carousel .floating_text_area.open .description{display:block}}.primary_media_feature .floating_text_area{bottom:2em}.banner_header_overlay{bottom:1em}.primary_media_feature .floating_text_area,.banner_header_overlay{position:absolute;z-index:12;color:white;width:100%;text-align:center;padding:0 1%}.primary_media_feature .floating_text_area .category_title,.banner_header_overlay .category_title{color:white;margin-bottom:0.7em}.primary_media_feature .floating_text_area .description,.banner_header_overlay .description{margin:-.5em auto 1em}@media (min-width: 769px), print{.primary_media_feature .floating_text_area .description,.banner_header_overlay .description{width:500px;margin-bottom:1.5em}}@media (min-width: 1024px), print{.primary_media_feature .floating_text_area .description,.banner_header_overlay .description{width:550px}}.primary_media_feature .floating_text_area .media_feature_title,.banner_header_overlay .media_feature_title{color:white;margin-bottom:.4em;font-size:1.93em}@media (min-width: 600px), print{.primary_media_feature .floating_text_area .media_feature_title,.banner_header_overlay .media_feature_title{font-size:2.8em}}.primary_media_feature .floating_text_area .media_feature_title a,.banner_header_overlay .media_feature_title a{color:white;text-decoration:none}.custom_banner_container{height:190px;width:100%;background-size:cover;background-position:center}@media only screen and (orientation: landscape){.custom_banner_container{height:260px}}@media (min-width: 600px), print{.custom_banner_container{height:420px}}@media only screen and (min-width: 600px) and (orientation: landscape){.custom_banner_container{height:350px}}@media (min-width: 769px), print{.custom_banner_container{height:400px}}@media only screen and (min-width: 769px) and (orientation: landscape){.custom_banner_container{height:400px}}@media (min-width: 1024px), print{.custom_banner_container{height:440px}}@media (min-width: 1200px){.custom_banner_container{height:550px}}@media (min-width: 1700px){.custom_banner_container{height:660px}}.custom_banner_container .banner_header_overlay{position:absolute;width:100%;bottom:0;z-index:2}.custom_banner_container .article_title{margin-bottom:.5em;text-align:center;color:#FFF}.custom_banner_container .secondary_nav_mobile{display:block}.custom_banner_container .secondary_nav_mobile select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin:.3em 0 2em}.custom_banner_container .secondary_nav_mobile select::-ms-expand{display:none}.custom_banner_container .secondary_nav_mobile select option{padding:0.5em 1em}@media (min-width: 769px), print{.custom_banner_container .secondary_nav_mobile{display:none}}.custom_banner_container .secondary_nav_desktop{display:none;margin:0 0 .8em 0;text-align:center}@media (min-width: 769px), print{.custom_banner_container .secondary_nav_desktop{display:block}}.custom_banner_container .secondary_nav_desktop li{display:inline-block;position:relative}.custom_banner_container .secondary_nav_desktop a{color:#5AA1F5;font-size:1.2em;font-weight:700;display:block;padding:.3em .9em .3em 0}@media (min-width: 1700px){.custom_banner_container .secondary_nav_desktop a{font-size:1.3em}}.custom_banner_container .secondary_nav_desktop li.current a,.custom_banner_container .secondary_nav_desktop li:hover a{text-decoration:none;color:white}.homepage_carousel .master-slider .ms-slide-bgvideocont{transform:none !important}#masterslider{height:480px;width:100%}@media only screen and (orientation: landscape){#masterslider{height:260px}}@media (min-width: 600px), print{#masterslider{height:500px}}@media only screen and (min-width: 600px) and (orientation: landscape){#masterslider{height:350px}}@media (min-width: 769px), print{#masterslider{height:500px}}@media only screen and (min-width: 769px) and (orientation: landscape){#masterslider{height:500px}}@media (min-width: 1024px), print{#masterslider{height:640px}}@media (min-width: 1200px){#masterslider{height:780px}}@media (min-width: 1700px){#masterslider{height:800px}}#masterslider .ms-slide-bgvideocont{background-color:#000}#masterslider .ms-slide-bgvideocont video{max-width:none}#masterslider .ms-nav-next,#masterslider .ms-nav-prev{display:none}@media (min-width: 769px), print{#masterslider .ms-nav-next,#masterslider .ms-nav-prev{display:block}}#masterslider .ms-nav-prev,#masterslider .ms-nav-next{width:40px;height:80px;margin-top:-60px}@media (min-width: 769px), print{#masterslider .ms-nav-prev,#masterslider .ms-nav-next{margin-top:-80px}}#masterslider .ms-nav-prev{background:url("https://mars.nasa.gov/assets/arrow_left_slim.png");background-size:40px 103px;background-position:0;left:15px;border-top-right-radius:6px;border-bottom-right-radius:6px}#masterslider .ms-nav-next{background:url("https://mars.nasa.gov/assets/arrow_right_slim.png");background-size:40px 103px;background-position:0;right:15px;border-top-left-radius:6px;border-bottom-left-radius:6px}#masterslider .ms-bullets{left:0;right:0;margin:0 auto;bottom:120px;z-index:10}@media (min-width: 769px), print{#masterslider .ms-bullets{bottom:125px}}#masterslider .ms-bullet{background-color:white;background-image:none;border-radius:50% 50% 50% 50%;height:8px;width:8px;opacity:0.5;margin:0 10px}#masterslider .ms-bullet:hover{opacity:1.0}#masterslider .ms-bullet-selected{opacity:1.0}.wysiwyg_content section.jpl_carousel{margin-bottom:3rem}.wysiwyg_content section.jpl_carousel .image_carousel_caption{position:relative}.wysiwyg_content section.jpl_carousel .master-slider{width:100%;height:300px}@media (min-width: 600px), print{.wysiwyg_content section.jpl_carousel .master-slider{height:394px}}@media (min-width: 769px), print{.wysiwyg_content section.jpl_carousel .master-slider{height:534px}}.feature_pages .wysiwyg_content section.jpl_carousel{max-width:600px;width:94%}@media (min-width: 769px), print{.feature_pages .wysiwyg_content section.jpl_carousel{max-width:calc(600px + 15%)}}.wysiwyg_content section.jpl_carousel .slider_title{margin-top:0.4rem;font-weight:700}.wysiwyg_content section.jpl_carousel .slider_caption{color:#5a6470;font-size:0.8em;height:auto;line-height:1.4em}.wysiwyg_content section.jpl_carousel .slider_link{margin-left:0.4rem;font-size:.85em}.wysiwyg_content section.jpl_carousel .ms-nav-next,.wysiwyg_content section.jpl_carousel .ms-nav-prev{display:none}.wysiwyg_content section.jpl_carousel.medium_mid .ms-nav-next,.wysiwyg_content section.jpl_carousel.medium_mid .ms-nav-prev,.wysiwyg_content section.jpl_carousel.medium_large .ms-nav-next,.wysiwyg_content section.jpl_carousel.medium_large .ms-nav-prev,.wysiwyg_content section.jpl_carousel.large .ms-nav-next,.wysiwyg_content section.jpl_carousel.large .ms-nav-prev,.wysiwyg_content section.jpl_carousel.xlarge .ms-nav-next,.wysiwyg_content section.jpl_carousel.xlarge .ms-nav-prev,.wysiwyg_content section.jpl_carousel.xxlarge .ms-nav-next,.wysiwyg_content section.jpl_carousel.xxlarge .ms-nav-prev{display:block}.no-touchevents .wysiwyg_content section.jpl_carousel.medium .ms-nav-next,.no-touchevents .wysiwyg_content section.jpl_carousel.medium .ms-nav-prev,.no-touchevents .wysiwyg_content section.jpl_carousel.small .ms-nav-next,.no-touchevents .wysiwyg_content section.jpl_carousel.small .ms-nav-prev{display:block}.wysiwyg_content section.jpl_carousel .ms-nav-prev,.wysiwyg_content section.jpl_carousel .ms-nav-next{width:40px;height:80px;margin-top:-60px}.wysiwyg_content section.jpl_carousel.medium_large .ms-nav-next,.wysiwyg_content section.jpl_carousel.medium_large .ms-nav-prev,.wysiwyg_content section.jpl_carousel.large .ms-nav-next,.wysiwyg_content section.jpl_carousel.large .ms-nav-prev,.wysiwyg_content section.jpl_carousel.xlarge .ms-nav-next,.wysiwyg_content section.jpl_carousel.xlarge .ms-nav-prev,.wysiwyg_content section.jpl_carousel.xxlarge .ms-nav-next,.wysiwyg_content section.jpl_carousel.xxlarge .ms-nav-prev{margin-top:-80px}.wysiwyg_content section.jpl_carousel .ms-nav-prev{background:url("https://mars.nasa.gov/assets/arrow_left_slim.png");background-size:40px 95px;background-color:transparent;background-position:0;left:0;border-top-right-radius:6px;border-bottom-right-radius:6px}.wysiwyg_content section.jpl_carousel .ms-nav-next{background:url("https://mars.nasa.gov/assets/arrow_right_slim.png");background-size:40px 95px;background-color:transparent;background-position:0;right:0;border-top-left-radius:6px;border-bottom-left-radius:6px}.wysiwyg_content section.jpl_carousel .x_of_y{position:absolute;top:-48px;left:0;right:0;margin:auto;padding:2px;background-color:#e7dfdd;background-color:rgba(231,223,221,0.8);width:60px;border-radius:20px;text-align:center}.slick-slider .slick-prev,.slick-slider .slick-next{width:30px;height:48px}.slick-slider .slick-prev:before,.slick-slider .slick-next:before{content:'';display:inline-block;padding:0;cursor:pointer;width:17px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -30px -100px;background-size:300px;opacity:1}.slick-slider .slick-prev:before:hover,.slick-slider .slick-prev:before.active,.slick-slider .slick-prev:before.current,.slick-slider .slick-next:before:hover,.slick-slider .slick-next:before.active,.slick-slider .slick-next:before.current{background-position:-30px -100px}.slick-slider .slick-prev:not(.slick-disabled):hover:before,.slick-slider .slick-next:not(.slick-disabled):hover:before{opacity:1}.slick-slider .slick-next:before{content:'';display:inline-block;padding:0;cursor:pointer;width:17px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -30px -150px;background-size:300px}.slick-slider .slick-next:before:hover,.slick-slider .slick-next:before.active,.slick-slider .slick-next:before.current{background-position:-30px -150px}.slick-slider .slick-prev{left:-33px}.slick-slider .slick-next{right:-33px}.slick-slider .slick-disabled{cursor:default;opacity:.4}.slick-slider .slick-disabled:before{cursor:default}.main_carousel .slick-nav_container{width:100%;text-align:center;padding-top:1em;border-top:1px solid #BEBEBE}@media (min-width: 600px), print{.main_carousel .slick-nav_container{border:none;margin-top:1em}}.main_carousel .slick-dots{position:relative;top:0}.main_carousel .slick-dots li{vertical-align:top}.main_carousel .slick-dots li button:before{content:"";border-radius:50%;background-color:black;height:8px;width:8px;top:6px;left:5px;text-align:center}.main_carousel .slick-nav{position:relative;display:inline-block}.main_carousel .slick-prev,.main_carousel .slick-next{top:-4px}.suggested_features .slick-prev,.suggested_features .slick-next{top:38%}.suggested_features .slick-prev{left:-9%}@media (min-width: 600px), print{.suggested_features .slick-prev{left:-7%}}@media (min-width: 769px), print{.suggested_features .slick-prev{left:-5%}}.suggested_features .slick-next{right:-9%}@media (min-width: 600px), print{.suggested_features .slick-next{right:-7%}}@media (min-width: 769px), print{.suggested_features .slick-next{right:-5%}}.related .slick-prev,.related .slick-next{top:31%}@media (min-width: 600px), print{.related .slick-prev,.related .slick-next{top:38%}}@media (min-width: 1024px), print{.related .slick-prev,.related .slick-next{top:31%}}.related .slick-prev:before,.related .slick-next:before{background-image:none}.main_carousel.module{padding-top:1.7em;padding-bottom:2.1em}@media (min-width: 600px), print{.main_carousel.module{padding-top:2em;padding-bottom:2.2em}}@media (min-width: 769px), print{.main_carousel.module{padding-top:3em;padding-bottom:2.9em}}@media (min-width: 769px), print{.main_carousel.module{padding-top:3.5em;padding-bottom:3.2em}}.main_carousel.module .carousel_header .carousel_title{margin-bottom:.8em}@media (min-width: 600px), print{.main_carousel.module .carousel_header .carousel_title{margin-bottom:1.1em}}.main_carousel.module .slick-slider{margin-left:auto;margin-right:auto;width:100%;margin-bottom:0}.main_carousel.module .slick-slider .slick-slide a{text-decoration:none}.main_carousel.module .slick-slider .content_title{margin:.6em 0 0;color:#222}@media (min-width: 600px), print{.main_carousel.module .slick-slider .content_title{margin:0 0 .8em}}.main_carousel.module .slick-slider .content_title h1{font-size:1.209em;margin-bottom:0em}@media (min-width: 600px), print{.main_carousel.module .slick-slider .content_title h1{font-size:1.395em;margin-bottom:0em}}@media (min-width: 769px), print{.main_carousel.module .slick-slider .content_title h1{font-size:1.581em;margin-bottom:0em}}@media (min-width: 1024px), print{.main_carousel.module .slick-slider .content_title h1{font-size:1.674em;margin-bottom:0em}}@media (min-width: 1200px){.main_carousel.module .slick-slider .content_title h1{font-size:1.767em;margin-bottom:0em}}.main_carousel.module .slick-slider .left-col,.main_carousel.module .slick-slider .right-col{display:inline-block;vertical-align:top}@media (min-width: 600px), print{.main_carousel.module .slick-slider .left-col{display:inline-block;width:45.83333%;float:left;margin-right:4.16667%}}.main_carousel.module .slick-slider .right-col{width:100%}@media (min-width: 600px), print{.main_carousel.module .slick-slider .right-col{width:50%;float:right;margin-right:0}}.main_carousel.module .slick-slider .carousel_item_description{display:none}@media (min-width: 600px), print{.main_carousel.module .slick-slider .carousel_item_description{display:block;margin-bottom:1.5em}}.main_carousel.module .slick-slider .content_body{margin-bottom:.9em}@media (min-width: 600px), print{.main_carousel.module .slick-slider .content_body{margin-bottom:0}}.main_carousel.module .slick-slider .content_footer{padding:.6em 1em 1em 0}@media (min-width: 769px), print{.main_carousel.module .slick-slider .content_footer{padding:0}.main_carousel.module .slick-slider .content_footer a+a{margin-top:.6em}}.main_carousel.module .slick-slider .content_footer .full_artical_link,.main_carousel.module .slick-slider .content_footer .full_category_link{padding-right:.8em;display:inline-block;font-size:.9em;white-space:nowrap}@media (min-width: 600px), print{.main_carousel.module .slick-slider .content_footer .full_artical_link,.main_carousel.module .slick-slider .content_footer .full_category_link{font-size:1em}}@media (min-width: 769px), print{.main_carousel.module .slick-slider .content_footer .full_artical_link,.main_carousel.module .slick-slider .content_footer .full_category_link{float:left;clear:both}}.main_carousel.module .slick-slider .content_footer .full_artical_link:before,.main_carousel.module .slick-slider .content_footer .full_category_link:before{content:"› "}.suggested_features.module .slick-slider,.related.module .slick-slider{margin-left:auto;margin-right:auto;width:100%;margin-bottom:1em}@media (min-width: 480px){.suggested_features.module .slick-slider,.related.module .slick-slider{width:84%}}@media (min-width: 600px), print{.suggested_features.module .slick-slider,.related.module .slick-slider{margin-bottom:1.5em;width:90%}}.suggested_features.module .slick-slider .category_title,.related.module .slick-slider .category_title{color:white;line-height:1.4em}.suggested_features.module .slick-slider .slick-slide a,.related.module .slick-slider .slick-slide a{text-decoration:none;color:#222}.suggested_features.module .slick-slider .slide,.related.module .slick-slider .slide{margin:0 6px}@media (min-width: 769px), print{.suggested_features.module .slick-slider .slide,.related.module .slick-slider .slide{margin:0 9px}}.no-touchevents .suggested_features.module .slide:hover .content_title a,.no-touchevents .related.module .slide:hover .content_title a{color:#366599}.no-touchevents .suggested_features.module .slide:hover .rollover_description,.no-touchevents .related.module .slide:hover .rollover_description{background-color:rgba(0,0,0,0.7)}.suggested_features.module .content_title,.related.module .content_title{padding:.6em 0 0;color:#222}.suggested_features.module .content_title a,.related.module .content_title a{font-size:.9em}@media (min-width: 769px), print{.suggested_features.module .content_title a,.related.module .content_title a{font-size:1.14em}}.suggested_features.module .image_and_description_container,.related.module .image_and_description_container{position:relative;overflow:hidden;min-height:129px}.carousel_teaser .slick-slider,.news_teaser .slick-slider,.multimedia_teaser .slick-slider{margin-left:auto;margin-right:auto;width:100%;margin-bottom:2.5em}@media (min-width: 480px){.carousel_teaser .slick-slider,.news_teaser .slick-slider,.multimedia_teaser .slick-slider{width:84%}}@media (min-width: 600px), print{.carousel_teaser .slick-slider,.news_teaser .slick-slider,.multimedia_teaser .slick-slider{width:88%;margin-bottom:3em}}@media (min-width: 769px), print{.carousel_teaser .slick-slider,.news_teaser .slick-slider,.multimedia_teaser .slick-slider{margin-bottom:3.5em}}@media (min-width: 1300px){.carousel_teaser .slick-slider,.news_teaser .slick-slider,.multimedia_teaser .slick-slider{width:92%}}.carousel_teaser .slick-slider .category_title,.news_teaser .slick-slider .category_title,.multimedia_teaser .slick-slider .category_title{color:white;line-height:1.4em}.carousel_teaser .slick-slider .slick-slide a,.news_teaser .slick-slider .slick-slide a,.multimedia_teaser .slick-slider .slick-slide a{text-decoration:none;color:#222}.carousel_teaser .slick-slider .slide,.news_teaser .slick-slider .slide,.multimedia_teaser .slick-slider .slide{margin:0 6px}@media (min-width: 769px), print{.carousel_teaser .slick-slider .slide,.news_teaser .slick-slider .slide,.multimedia_teaser .slick-slider .slide{margin:0 6px}}.no-touchevents .carousel_teaser .slick-slider .slide:hover .content_title a,.no-touchevents .news_teaser .slick-slider .slide:hover .content_title a,.no-touchevents .multimedia_teaser .slick-slider .slide:hover .content_title a{color:#366599;cursor:pointer}.carousel_teaser .slick-slider .image_and_description_container,.news_teaser .slick-slider .image_and_description_container,.multimedia_teaser .slick-slider .image_and_description_container{position:relative;overflow:hidden;min-height:129px}.carousel_teaser .slick-slider .content_title,.news_teaser .slick-slider .content_title,.multimedia_teaser .slick-slider .content_title{padding:.6em 0 0;color:#222;font-weight:400}.carousel_teaser .slick-slider .slick-prev,.carousel_teaser .slick-slider .slick-next,.news_teaser .slick-slider .slick-prev,.news_teaser .slick-slider .slick-next,.multimedia_teaser .slick-slider .slick-prev,.multimedia_teaser .slick-slider .slick-next{top:35%;content:'';display:inline-block;padding:0;cursor:pointer;width:35px;height:35px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -32px -208px;background-size:300px}.carousel_teaser .slick-slider .slick-prev:hover,.carousel_teaser .slick-slider .slick-prev.active,.carousel_teaser .slick-slider .slick-prev.current,.carousel_teaser .slick-slider .slick-next:hover,.carousel_teaser .slick-slider .slick-next.active,.carousel_teaser .slick-slider .slick-next.current,.news_teaser .slick-slider .slick-prev:hover,.news_teaser .slick-slider .slick-prev.active,.news_teaser .slick-slider .slick-prev.current,.news_teaser .slick-slider .slick-next:hover,.news_teaser .slick-slider .slick-next.active,.news_teaser .slick-slider .slick-next.current,.multimedia_teaser .slick-slider .slick-prev:hover,.multimedia_teaser .slick-slider .slick-prev.active,.multimedia_teaser .slick-slider .slick-prev.current,.multimedia_teaser .slick-slider .slick-next:hover,.multimedia_teaser .slick-slider .slick-next.active,.multimedia_teaser .slick-slider .slick-next.current{background-position:-32px -258px}.multimedia_module_gallery .carousel_teaser .slick-slider .slick-prev,.multimedia_module_gallery .carousel_teaser .slick-slider .slick-next,.multimedia_module_gallery .news_teaser .slick-slider .slick-prev,.multimedia_module_gallery .news_teaser .slick-slider .slick-next,.multimedia_module_gallery .multimedia_teaser .slick-slider .slick-prev,.multimedia_module_gallery .multimedia_teaser .slick-slider .slick-next{top:45%}.related .carousel_teaser .slick-slider .slick-prev,.related .carousel_teaser .slick-slider .slick-next,.related .news_teaser .slick-slider .slick-prev,.related .news_teaser .slick-slider .slick-next,.related .multimedia_teaser .slick-slider .slick-prev,.related .multimedia_teaser .slick-slider .slick-next{top:25%}@media (min-width: 769px), print{.related .carousel_teaser .slick-slider .slick-prev,.related .carousel_teaser .slick-slider .slick-next,.related .news_teaser .slick-slider .slick-prev,.related .news_teaser .slick-slider .slick-next,.related .multimedia_teaser .slick-slider .slick-prev,.related .multimedia_teaser .slick-slider .slick-next{top:25%}}@media (min-width: 1024px), print{.related .carousel_teaser .slick-slider .slick-prev,.related .carousel_teaser .slick-slider .slick-next,.related .news_teaser .slick-slider .slick-prev,.related .news_teaser .slick-slider .slick-next,.related .multimedia_teaser .slick-slider .slick-prev,.related .multimedia_teaser .slick-slider .slick-next{top:31%}}.news_teaser .carousel_teaser .slick-slider .slick-prev,.suggested_features .carousel_teaser .slick-slider .slick-prev,.news_teaser .carousel_teaser .slick-slider .slick-next,.suggested_features .carousel_teaser .slick-slider .slick-next,.news_teaser .news_teaser .slick-slider .slick-prev,.suggested_features .news_teaser .slick-slider .slick-prev,.news_teaser .news_teaser .slick-slider .slick-next,.suggested_features .news_teaser .slick-slider .slick-next,.news_teaser .multimedia_teaser .slick-slider .slick-prev,.suggested_features .multimedia_teaser .slick-slider .slick-prev,.news_teaser .multimedia_teaser .slick-slider .slick-next,.suggested_features .multimedia_teaser .slick-slider .slick-next{top:38%}.carousel_teaser .slick-slider .slick-prev,.news_teaser .slick-slider .slick-prev,.multimedia_teaser .slick-slider .slick-prev{transform:scaleX(-1)}.carousel_teaser .slick-slider .slick-prev,.news_teaser .slick-slider .slick-prev,.multimedia_teaser .slick-slider .slick-prev{left:-9%}@media (min-width: 600px), print{.carousel_teaser .slick-slider .slick-prev,.news_teaser .slick-slider .slick-prev,.multimedia_teaser .slick-slider .slick-prev{left:-7%}}@media (min-width: 769px), print{.carousel_teaser .slick-slider .slick-prev,.news_teaser .slick-slider .slick-prev,.multimedia_teaser .slick-slider .slick-prev{left:-6.5%}}.carousel_teaser .slick-slider .slick-next,.news_teaser .slick-slider .slick-next,.multimedia_teaser .slick-slider .slick-next{right:-9%}@media (min-width: 600px), print{.carousel_teaser .slick-slider .slick-next,.news_teaser .slick-slider .slick-next,.multimedia_teaser .slick-slider .slick-next{right:-7%}}@media (min-width: 769px), print{.carousel_teaser .slick-slider .slick-next,.news_teaser .slick-slider .slick-next,.multimedia_teaser .slick-slider .slick-next{right:-6.5%}}.carousel_teaser .slick-slider .slick-disabled,.news_teaser .slick-slider .slick-disabled,.multimedia_teaser .slick-slider .slick-disabled{cursor:default;opacity:.4}.carousel_teaser .slick-slider .slick-disabled:before,.news_teaser .slick-slider .slick-disabled:before,.multimedia_teaser .slick-slider .slick-disabled:before{cursor:default}section.vital_signs_menu .readout{color:#222;display:inline-block;text-align:right;position:absolute;right:0;top:0;height:100%}section.vital_signs_menu .readout .title{font-size:1em;font-weight:300;color:#e4e4e4;white-space:nowrap;text-transform:uppercase}@media (min-width: 600px), print{section.vital_signs_menu .readout .title{font-size:1.4em}}.mini_module header h3,.mini_module header h4{color:#B27628;font-weight:400;font-size:0.9em;margin:0 0 0.1em;text-transform:uppercase;letter-spacing:0}.mini_module .description{font-size:1.85rem}.mini_module .transmissions{font-weight:400;color:#B27628}.mini_module.countdown{overflow:hidden}.mini_module.countdown .unit&gt;span{color:#B27628;background-color:#FFF1DE}#iframe_overlay .mini_module header h3,#iframe_overlay .mini_module header h4{color:#f7c585}#iframe_overlay .mini_module.countdown .unit&gt;span{color:#B27628;background-color:#382B19}#iframe_overlay .mini_module .transmissions{color:#f7c585}#secondary_column .boxed hr+.mini_module{border:none;margin:0;padding:0}#secondary_column .mini_module header{margin-bottom:0.1rem}#secondary_column .mini_module .description{font-size:1.4em}@media (min-width: 1024px), print{#secondary_column .mini_module .description{font-size:1.5em}}#secondary_column .countdown .unit span{padding:0 0.8em}#primary_column .mini_module{margin:3rem 0}#primary_column .mini_module header{margin-bottom:.5rem}#primary_column .mini_module header h3{font-size:1.7rem}.homepage_dashboard_modal.vital_signs_container{position:absolute;top:0;left:0;width:100%;height:calc(100vh - 36px);z-index:41;color:#222;overflow-x:hidden;overflow-y:hidden;-webkit-overflow-scrolling:touch;visibility:hidden;opacity:0;transition:opacity .5s;background:rgba(0,0,0,0.9);word-wrap:break-word;transition:opacity 400ms}.homepage_dashboard_modal.vital_signs_container.visible{visibility:visible;opacity:1}.homepage_dashboard_modal.vital_signs_container.hide_background{background:transparent}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container{z-index:30}}.homepage_dashboard_modal.vital_signs_container .loading{position:absolute;left:50%;top:44vh;width:150px;margin-left:-75px;text-align:center}.homepage_dashboard_modal.vital_signs_container .loading p{color:#fcb963;padding-top:67px;font-size:15px;font-weight:300;margin:0;display:none}.homepage_dashboard_modal.vital_signs_container .vital_signs.module{padding:0}.homepage_dashboard_modal.vital_signs_container .countdown_time .unit{color:#efe9e6}.homepage_dashboard_modal.vital_signs_container .countdown_time .unit span{color:#966b35;background-color:#000000}.homepage_dashboard_modal.vital_signs_container .content{transition:opacity .5s;opacity:0}.homepage_dashboard_modal.vital_signs_container .content.visible{opacity:1}.homepage_dashboard_modal.vital_signs_container .vital_signs_header{margin-bottom:.8em}@media (min-width: 600px), print{.homepage_dashboard_modal.vital_signs_container .vital_signs_header{margin-bottom:1.5em}}.homepage_dashboard_modal.vital_signs_container a{color:#6bbed8;font-size:.9em}.homepage_dashboard_modal.vital_signs_container .more_link{margin:1em 0;float:right;font-weight:600;white-space:nowrap}.homepage_dashboard_modal.vital_signs_container .more_link::after{content:" ›"}.homepage_dashboard_modal.vital_signs_container .grid_layout{display:flex;flex-direction:column;background:rgba(62,23,0,0.75);position:relative;width:100%;margin:0;padding:2em;height:calc(100vh - 36px);z-index:5;left:0;top:100vh;opacity:0}@media (min-width: 769px), print{.homepage_dashboard_modal.vital_signs_container .grid_layout{padding:2em 15%}}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container .grid_layout{display:block;padding:2em 2em 2.5em;max-height:100%;height:auto;background:rgba(39,20,5,0.95);top:4.5em;max-width:550px;width:48%;left:100%}}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container .grid_layout{max-width:700px;width:48%}}@media (min-width: 1700px){.homepage_dashboard_modal.vital_signs_container .grid_layout{max-width:750px;width:48%}}.homepage_dashboard_modal.vital_signs_container .background_area{width:100%;height:100%;position:absolute;top:0;left:0;filter:none;display:none}.homepage_dashboard_modal.vital_signs_container .background_area.blur{filter:blur(3px)}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container .background_area{display:none}}.homepage_dashboard_modal.vital_signs_container .module_title,.homepage_dashboard_modal.vital_signs_container .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .homepage_dashboard_modal.vital_signs_container .carousel_title{margin-bottom:0.4em;text-align:left;color:#efe9e6;font-weight:400;font-size:1.6em;width:90%}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container .module_title,.homepage_dashboard_modal.vital_signs_container .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .homepage_dashboard_modal.vital_signs_container .carousel_title{width:100%}}.homepage_dashboard_modal.vital_signs_container a.close_button{position:absolute;top:.8em;right:.8em;z-index:99;width:44px;height:44px}.homepage_dashboard_modal.vital_signs_container a.close_button .close_icon{display:block;height:100%;position:relative}.homepage_dashboard_modal.vital_signs_container a.close_button .close_icon:before{transform:rotate(-45deg);content:'';position:absolute;height:2px;width:100%;top:calc(50% - 1px);left:0;background:#fff;opacity:.8}.homepage_dashboard_modal.vital_signs_container a.close_button .close_icon:after{transform:rotate(45deg);content:'';position:absolute;height:2px;width:100%;top:calc(50% - 1px);left:0;background:#fff;opacity:.8}.no-touchevents .homepage_dashboard_modal.vital_signs_container a.close_button:hover{opacity:1}@media (min-width: 600px), print{.homepage_dashboard_modal.vital_signs_container a.close_button{top:1em;right:1em}}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container a.close_button{background:rgba(37,21,8,0.8)}}@media (min-width: 1700px){.homepage_dashboard_modal.vital_signs_container a.close_button{top:1.2em;right:1.2em;width:60px;height:60px}}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser{overflow:visible;padding-bottom:2em}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .loader{font-size:15px;color:#fcb963;display:block;margin:1em 0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser h3,.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser h4{color:#fcb963;font-weight:400;font-size:0.9em;margin:0 0 0.1em;text-transform:uppercase;letter-spacing:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser p{color:#efe9e6;font-weight:300}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .desc_title{font-weight:600;margin-bottom:.3em;display:block}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser article{margin:0 0 0.5em}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser article header{margin:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser article header .subtitle{color:#fcb963}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser article .description{color:#efe9e6;font-weight:300}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser article .description:not(p){font-size:1.8em}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list{margin-bottom:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_content{padding:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_title{margin-bottom:.5em;text-transform:uppercase;font-size:.9em;font-weight:400;color:#fcb963}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_image{width:30%;float:right;margin:0 0 1em 1.8em}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_image .caption{font-size:0.7em;display:block;margin-top:0.4em;color:#beb0a4}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text{width:100%;float:none}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text p,.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text .description{color:#efe9e6;font-weight:300}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text p,.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text .description a{font-size:16px}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text p{margin:1em 0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text p:first-of-type{margin-top:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text p:last-of-type{margin-bottom:0}.homepage_dashboard_modal.vital_signs_container .vital_signs_teaser .item_list .list_text a.more_or_less{color:#efe9e6;text-decoration:underline}.homepage_dashboard_modal.vital_signs_container .overlay_nav li{display:inline-block;color:#94623a;font-size:1.2em;cursor:pointer;transition:all 200ms;margin-right:0.8em}.homepage_dashboard_modal.vital_signs_container .overlay_nav li.current,.homepage_dashboard_modal.vital_signs_container .overlay_nav li:hover{color:white}.homepage_dashboard_modal.vital_signs_container .overlay_nav .nav_spacer{font-size:0.4em;vertical-align:middle;margin:0 .5em;color:#94623a}.homepage_dashboard_modal.vital_signs_container .column_container{display:block;overflow:auto;overflow-x:hidden;padding-right:2em;max-width:100%;max-height:calc(90% - 150px)}@media (min-height: 800px){.homepage_dashboard_modal.vital_signs_container .column_container{max-height:calc(97% - 150px)}}@media (min-width: 769px) and (min-height: 700px), print and (min-height: 700px){.homepage_dashboard_modal.vital_signs_container .column_container{max-height:calc(100% - 129px)}}@media (min-width: 1200px){.homepage_dashboard_modal.vital_signs_container .column_container{width:100%;overflow-y:auto;height:auto;max-height:calc(80vh - 320px)}}@media (min-width: 1200px) and (min-height: 700px){.homepage_dashboard_modal.vital_signs_container .column_container{max-height:calc(88vh - 320px)}}.homepage_dashboard_modal.vital_signs_container .column_container::-webkit-scrollbar-track{background-color:rgba(252,185,99,0.2)}.homepage_dashboard_modal.vital_signs_container .column_container::-webkit-scrollbar{width:5px;left:10px}.homepage_dashboard_modal.vital_signs_container .column_container::-webkit-scrollbar-thumb{background-color:#eaeaea}.homepage_dashboard_modal.vital_signs_container .content_col{position:relative;float:left;width:100%}.homepage_dashboard_modal.vital_signs_container .content_col hr{border-top:1px solid rgba(255,255,255,0.2)}.homepage_dashboard_modal.vital_signs_container .content_col .wysiwyg_content&gt;*{color:#efe9e6}.homepage_dashboard_modal.vital_signs_container .content_col .wysiwyg_content&gt;*:last-child{margin-bottom:0}.homepage_dashboard_modal.vital_signs_container .content_col .wysiwyg_content p{color:#efe9e6}.homepage_dashboard_modal.vital_signs_container .content_col .wysiwyg_content h4{font-size:0.9em;font-weight:400;margin-top:0}.homepage_dashboard_modal.vital_signs_container .dsn_status_module+p{margin-top:0}#vital_signs_modal::-webkit-scrollbar-track{background-color:rgba(252,185,99,0.2)}#vital_signs_modal::-webkit-scrollbar{width:4px;left:10px}#vital_signs_modal::-webkit-scrollbar-thumb{background-color:#eaeaea}section.vital_signs_menu{overflow:hidden;position:relative;background-color:transparent;color:white;z-index:20;height:95px;text-align:center;visibility:hidden;margin-top:-95px}section.vital_signs_menu .slick-slider{width:80%;margin-left:auto;margin-right:auto;overflow:hidden}@media (min-width: 480px){section.vital_signs_menu .slick-slider{width:84%}}@media (min-width: 600px), print{section.vital_signs_menu .slick-slider{width:calc(100% - 85px)}}@media (min-width: 769px), print{section.vital_signs_menu .slick-slider{width:90%}}@media (min-width: 1024px), print{section.vital_signs_menu .slick-slider{width:92%}}@media (min-width: 1700px){section.vital_signs_menu .slick-slider{width:1350px}}section.vital_signs_menu .slick-slider .slick-list{z-index:22}section.vital_signs_menu .slick-slider .slick-track{margin-left:auto;margin-right:auto}section.vital_signs_menu .slick-navigation{display:block;position:absolute;top:0;width:100%;height:100%;z-index:21}@media screen and (min-width: 1550px){section.vital_signs_menu .slick-navigation{display:none}}section.vital_signs_menu .slick-navigation .slick-prev,section.vital_signs_menu .slick-navigation .slick-next{position:absolute;background-color:transparent;font-size:1.8em;color:#808080;height:77px;background-image:url("https://mars.nasa.gov/assets/arrows_blue_sprite@2x.png");background-repeat:no-repeat;background-size:135px;width:34px;text-indent:-9999px;top:4px;z-index:999999}section.vital_signs_menu .slick-navigation .slick-prev{left:.7%;border-right:1px solid rgba(216,216,216,0.2);background-position:-27px 19px}@media (min-width: 1024px), print{section.vital_signs_menu .slick-navigation .slick-prev{left:.8%}}@media (min-width: 1200px){section.vital_signs_menu .slick-navigation .slick-prev{left:.9%}}section.vital_signs_menu .slick-navigation .slick-prev:hover{background-position:0px 19px}section.vital_signs_menu .slick-navigation .slick-next{right:.7%;border-left:1px solid rgba(216,216,216,0.2);background-position:-75px 19px}@media (min-width: 1024px), print{section.vital_signs_menu .slick-navigation .slick-next{right:.8%}}@media (min-width: 1200px){section.vital_signs_menu .slick-navigation .slick-next{right:.9%}}section.vital_signs_menu .slick-navigation .slick-next:hover{background-position:-102px 19px}section.vital_signs_menu ul .slick-slide{height:95px;margin:0}section.vital_signs_menu li{display:inline-block;vertical-align:top;position:relative;width:100%}section.vital_signs_menu li:last-child .image_and_description_container{border-right:none}section.vital_signs_menu .image_and_description_container{display:block;overflow:visible;cursor:pointer;width:100%;height:75%;padding-right:.5em;padding-left:1em}@media (min-width: 480px){section.vital_signs_menu .image_and_description_container{border-right:1px solid rgba(216,216,216,0.2)}}@media (min-width: 1700px){section.vital_signs_menu .image_and_description_container{border-right:none}}.no-touchevents section.vital_signs_menu .image_and_description_container:hover .readout .overlay_icon{background:url("https://mars.nasa.gov/assets/dashboard_expand_hover.png") no-repeat;background-size:100%}section.vital_signs_menu .image_and_description_container.current .readout .overlay_icon{background:url("https://mars.nasa.gov/assets/dashboard_contract.png") no-repeat;background-size:100%}.no-touchevents section.vital_signs_menu .image_and_description_container.current:hover .readout .overlay_icon{background:url("https://mars.nasa.gov/assets/dashboard_contract_hover.png") no-repeat !important;background-size:100% !important}section.vital_signs_menu .readout .subtitle{color:#4e8fa4;font-size:.78em;font-weight:500;text-transform:uppercase}section.vital_signs_menu .readout{color:white;position:relative;text-align:left;top:11px;margin:0 auto;float:left}section.vital_signs_menu .readout .text_container{float:left;margin-right:10px}section.vital_signs_menu .readout .title{font-size:1.4em;margin-bottom:0;letter-spacing:-.03em}@media (min-width: 480px){section.vital_signs_menu .readout .title{font-size:1.5em}}@media (min-width: 600px), print{section.vital_signs_menu .readout .title{font-size:1.2em}}@media (min-width: 769px), print{section.vital_signs_menu .readout .title{font-size:1.3em}}@media (min-width: 850px){section.vital_signs_menu .readout .title{font-size:1.1em}}@media (min-width: 950px){section.vital_signs_menu .readout .title{font-size:1.2em}}@media (min-width: 1700px){section.vital_signs_menu .readout .title{font-size:1.5em}}section.vital_signs_menu .readout .overlay_icon{position:absolute;width:37px;height:37px;background:url("https://mars.nasa.gov/assets/dashboard_expand.png") no-repeat;background-size:100%;display:inline-block}#vital_signs_modal.visible ~ .vital_signs_menu{z-index:40}div.modal_open{display:none !important}.image_of_the_day{overflow:hidden;z-index:10;padding:0}.image_of_the_day .window{width:100%;position:absolute;overflow:hidden;height:100%;padding:1em}.image_of_the_day .window.mobile{height:100%;min-height:100%}.image_of_the_day #featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}.image_of_the_day a.image_day{width:100%;height:100%;position:absolute;top:0;left:0;z-index:11;text-indent:-999px}.image_of_the_day .grid_layout{height:100%;position:relative}@media (min-width: 1024px), print{.image_of_the_day .grid_layout{width:86%}}.image_of_the_day .floating_text_area{position:absolute;bottom:0;width:100%;text-align:left}@media (min-width: 600px), print{.image_of_the_day .floating_text_area{bottom:1.3rem;text-align:left}}.image_of_the_day header{z-index:12;width:100%}.image_of_the_day header .header_link{display:inline-block;width:100%}@media (min-width: 600px), print{.image_of_the_day header .header_link{width:70%}}.image_of_the_day header .header_link:hover{text-decoration:none}.image_of_the_day header .category_title{color:white;font-size:0.9em;margin-bottom:0.3em}@media (min-width: 600px), print{.image_of_the_day header .category_title{margin-bottom:0.7em}}.image_of_the_day header .media_feature_title{font-size:1.75rem;font-weight:200;margin-bottom:.3rem}@media (min-width: 600px), print{.image_of_the_day header .media_feature_title{margin-bottom:0;font-size:3rem}}.image_of_the_day header .multimedia_link{display:inline-block;text-transform:uppercase;font-size:0.9em;color:white;text-align:right;font-weight:600;right:0;position:relative}@media (min-width: 600px), print{.image_of_the_day header .multimedia_link{bottom:10px;width:28%;position:absolute}}.image_of_the_day header .multimedia_link a{color:white}.image_of_the_day .outline_button,.image_of_the_day .primary_media_feature .floating_text_area .button,.primary_media_feature .floating_text_area .image_of_the_day .button,.image_of_the_day .primary_media_feature .floating_text_area .outline_button,.primary_media_feature .floating_text_area .image_of_the_day .outline_button,.image_of_the_day .banner_header_overlay .button,.banner_header_overlay .image_of_the_day .button{opacity:1}.filter_bar{z-index:20}.filter_bar .section_search{padding-bottom:1em;max-width:380px;width:100%;margin:0 auto}@media (min-width: 1024px), print{.filter_bar .section_search{width:auto;max-width:none;display:block !important;padding-bottom:0}}.filter_bar .section_search .search_submit{right:0px;top:-1px}.filter_bar.fixed{position:fixed;top:0;left:0;width:100%;-webkit-box-shadow:0 4px 4px -1px rgba(0,0,0,0.15);-moz-box-shadow:0 4px 4px -2px rgba(0,0,0,0.15);box-shadow:0 4px 4px -2px rgba(0,0,0,0.15)}.filter_bar .search_binder{width:100%;max-width:304px;position:relative;margin-left:auto;margin-right:auto;margin:0 auto .7em 0}@media (min-width: 480px){.filter_bar .search_binder{margin:0 0 .7em 0}}@media (min-width: 1024px), print{.filter_bar .search_binder{position:relative;vertical-align:top;display:inline-block;width:35%;margin-right:1%;max-width:350px}}.filter_bar input.search_field{width:100%}.filter_bar input.search_field::-webkit-input-placeholder{color:#bbb !important}.filter_bar input.search_field::-moz-placeholder{color:#bbb !important}.filter_bar input.search_field:-moz-placeholder{color:#bbb !important}.filter_bar input.search_field:-ms-input-placeholder{color:#bbb !important}.filter_bar select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin:0 auto .5em;float:none}.filter_bar select::-ms-expand{display:none}.filter_bar select option{padding:0.5em 1em}@media (min-width: 1024px), print{.filter_bar select{margin-bottom:0;width:30%;max-width:284px}.filter_bar select+select{margin-left:1%}}.filter_bar header{display:inline-block;width:100%;text-align:left}@media (min-width: 600px), print{.filter_bar header{text-align:center}}@media (min-width: 1024px), print{.filter_bar header{display:none}}.filter_bar .arrow_box{display:inline-block;position:absolute;padding:4px;cursor:pointer;right:0;bottom:7px;float:none;transition:all .2s}@media (min-width: 600px), print{.filter_bar .arrow_box{text-align:center}}.filter_bar .arrow_box.rotate_up{transform:rotate(180deg)}.filter_bar .arrow_box.rotate_right{transform:rotate(270deg)}.filter_bar .arrow_box.rotate_left{transform:rotate(90deg)}.filter_bar .arrow_box .arrow_down{display:block;border-left:8px solid transparent;border-right:8px solid transparent;border-top:8px solid #8597B1}.advanced_search .filter_bar .section_search{max-width:none}.advanced_search .filter_bar .suggestion_text{margin-top:0}.advanced_search .filter_bar .search_row+.search_row{margin-top:1em}@media (min-width: 600px), print{.advanced_search .filter_bar .search_row+.search_row{margin-top:0}}@media (min-width: 600px), print{.advanced_search .filter_bar .filter1{width:32.20339%;float:left;margin-right:1.69492%}}@media (min-width: 600px), print{.advanced_search .filter_bar .search_binder{width:49.15254%;float:left;margin-right:1.69492%}}@media (min-width: 600px), print{.advanced_search .filter_bar .conjunction{width:15.25424%;float:right;margin-right:0}}.advanced_search .filter_bar .search_field{background-color:transparent;border:1px solid #C1C1C1;padding-right:1.1em}.advanced_search footer{margin-top:1.5em}.rollover_description{opacity:0;height:0;z-index:1;overflow:hidden;transition:opacity .4s}.rollover_description .rollover_description_inner{height:100%;overflow:hidden}.slide{position:relative;min-height:100%}.slide .overlay_arrow{display:none}@media (min-width: 769px), print{.no-touchevents .slide:hover .rollover_description{padding:.9rem;position:absolute;opacity:1;height:auto;top:0;right:0;width:100%;height:100%;color:white;background-color:rgba(0,0,0,0.9);cursor:pointer;font-size:.95rem;line-height:1.3}.no-touchevents .slide:hover .rollover_description p{line-height:inherit;font-size:inherit;color:white}.no-touchevents .slide:hover .rollover_description p:first-child{margin-top:0}.no-touchevents .slide:hover .rollover_title{font-size:1.6em;font-weight:700;margin-bottom:.2em}.no-touchevents .slide:hover .overlay_arrow{height:14px;width:14px;position:absolute;right:14px;bottom:14px;display:block}.no-touchevents .slide:hover .overlay_arrow img{display:block}}.list_view .rollover_description{display:none}.fancybox-overlay,#fancybox-lock{background:#000 !important}.fancybox-mb-video.fancybox-wrap,.fancybox-mb-info.fancybox-wrap{background:#000}.fancybox-mb-video.fancybox-wrap .fancybox-prev span,.fancybox-mb-info.fancybox-wrap .fancybox-prev span{background-image:url("https://mars.nasa.gov/assets/arrow_left_darktheme.png") !important;background-position:0 !important}.fancybox-mb-video.fancybox-wrap .fancybox-next span,.fancybox-mb-info.fancybox-wrap .fancybox-next span{background-image:url("https://mars.nasa.gov/assets/arrow_right_darktheme.png") !important;background-position:0 !important}.fancybox-mb-video.fancybox-wrap .fancybox-inner,.fancybox-mb-info.fancybox-wrap .fancybox-inner{border:0}.fancybox-mb-video.fancybox-wrap .fancybox-title-float-wrap,.fancybox-mb-info.fancybox-wrap .fancybox-title-float-wrap{position:relative;right:auto;left:auto}.fancybox-mb-video.fancybox-wrap .fancybox-title-float-wrap .child,.fancybox-mb-info.fancybox-wrap .fancybox-title-float-wrap .child{display:block;margin:auto;white-space:normal;padding:1em 0;line-height:normal}.fancybox-mb-video.fancybox-wrap .fancybox-skin,.fancybox-mb-info.fancybox-wrap .fancybox-skin{background-color:black}.fancybox-mb-video.fancybox-wrap .fancybox-title-inside,.fancybox-mb-info.fancybox-wrap .fancybox-title-inside{text-align:left}.fancybox-mb-video.fancybox-wrap .fancybox-nav{top:-15%}@media (min-width: 1px) and (print: 769px){.fancybox-mb-video.fancybox-wrap,.fancybox-mb-info.fancybox-wrap{margin:0 !important;width:95% !important;margin:0 auto !important}.fancybox-mb-video.fancybox-wrap .fancybox-nav,.fancybox-mb-info.fancybox-wrap .fancybox-nav{display:none}.fancybox-mb-video.fancybox-wrap .fancybox-inner,.fancybox-mb-info.fancybox-wrap .fancybox-inner{width:100% !important;height:auto !important}.fancybox-mb-video.fancybox-wrap .fancybox-image,.fancybox-mb-info.fancybox-wrap .fancybox-image{width:100% !important;height:auto !important;margin:0 auto !important}.fancybox-mb-video.fancybox-wrap{left:0 !important;right:0 !important;position:relative !important;margin:0 auto !important;padding:0 !important;border:none}.fancybox-mb-video.fancybox-wrap .fancybox-inner{margin:0 !important}.fancybox-mb-video.fancybox-wrap .fancybox-iframe{height:600px !important}}#fancybox_video .player{min-height:200px;margin-bottom:1.5em}@media (min-width: 480px){#fancybox_video .player{min-height:300px}}@media (min-width: 600px), print{#fancybox_video .player{min-height:400px}}#fancybox_info{margin-top:1.5em}#fancybox_info,#fancybox_video{color:white}#fancybox_info p,#fancybox_info .description,#fancybox_video p,#fancybox_video .description{color:white}#fancybox_info .image_caption,#fancybox_info .image_caption p,#fancybox_video .image_caption,#fancybox_video .image_caption p{color:#aaa;font-size:.9em}#fancybox_info .image_details,#fancybox_video .image_details{overflow:hidden;display:inline-block;width:100%;min-width:inherit}#fancybox_info .image_details .text,#fancybox_video .image_details .text{float:left;text-align:left;width:100%;margin-top:10px}@media (min-width: 769px), print{#fancybox_info .image_details .text,#fancybox_video .image_details .text{margin-top:0}}@media (min-width: 1024px), print{#fancybox_info .image_details .text,#fancybox_video .image_details .text{width:60%}}#fancybox_info .image_details .text .title,#fancybox_video .image_details .text .title{font-size:1.235em;margin-bottom:.1em;line-height:1.3em;font-weight:700}@media (min-width: 600px), print{#fancybox_info .image_details .text .title,#fancybox_video .image_details .text .title{font-size:1.425em;margin-bottom:.18em}}@media (min-width: 769px), print{#fancybox_info .image_details .text .title,#fancybox_video .image_details .text .title{font-size:1.615em;margin-bottom:.26em}}@media (min-width: 1024px), print{#fancybox_info .image_details .text .title,#fancybox_video .image_details .text .title{font-size:1.71em;margin-bottom:.29em}}@media (min-width: 1200px){#fancybox_info .image_details .text .title,#fancybox_video .image_details .text .title{font-size:1.805em;margin-bottom:.32em}}#fancybox_info .image_details .buttons,#fancybox_video .image_details .buttons{width:100%;float:right}@media (min-width: 1024px), print{#fancybox_info .image_details .buttons,#fancybox_video .image_details .buttons{width:40%}}#fancybox_info .image_details .buttons .inner_buttons,#fancybox_video .image_details .buttons .inner_buttons{float:left}@media (min-width: 1024px), print{#fancybox_info .image_details .buttons .inner_buttons,#fancybox_video .image_details .buttons .inner_buttons{float:right}}#fancybox_info .image_details .buttons .addthis_toolbox,#fancybox_video .image_details .buttons .addthis_toolbox{border-radius:4px;overflow:hidden}#fancybox_info .image_details .buttons .addthis_toolbox img,#fancybox_video .image_details .buttons .addthis_toolbox img{height:37px !important;width:auto !important}@media (min-width: 1024px), print{#fancybox_info .image_details .buttons .addthis_toolbox img,#fancybox_video .image_details .buttons .addthis_toolbox img{height:38px !important}}#fancybox_info .image_details .buttons .close_button,#fancybox_video .image_details .buttons .close_button{margin-left:12px}#fancybox_info .image_details .buttons a.button,#fancybox_info .image_details .buttons a.outline_button,#fancybox_video .image_details .buttons a.button,#fancybox_video .image_details .buttons a.outline_button{padding-left:16px;padding-right:16px}#fancybox_info .image_details .buttons .addthis_toolbox,#fancybox_info .image_details .buttons a.button,#fancybox_info .image_details .buttons a.outline_button,#fancybox_video .image_details .buttons .addthis_toolbox,#fancybox_video .image_details .buttons a.button,#fancybox_video .image_details .buttons a.outline_button{float:left;margin-left:0;margin-right:12px}@media (min-width: 1024px), print{#fancybox_info .image_details .buttons .addthis_toolbox,#fancybox_info .image_details .buttons a.button,#fancybox_info .image_details .buttons a.outline_button,#fancybox_video .image_details .buttons .addthis_toolbox,#fancybox_video .image_details .buttons a.button,#fancybox_video .image_details .buttons a.outline_button{float:right;margin-left:12px;margin-right:0}}@media (min-width: 1024px), print{#fancybox_info .image_details .buttons .addthis_toolbox,#fancybox_info .image_details .buttons a.button,#fancybox_info .image_details .buttons a.outline_button,#fancybox_info .image_details .buttons .close_button,#fancybox_video .image_details .buttons .addthis_toolbox,#fancybox_video .image_details .buttons a.button,#fancybox_video .image_details .buttons a.outline_button,#fancybox_video .image_details .buttons .close_button{margin-bottom:12px}}#fancybox_info .close_button,#fancybox_video .close_button{padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -25px 0px;background-size:300px;z-index:8060;position:relative;display:block;float:right}#fancybox_info .close_button:hover,#fancybox_info .close_button.active,#fancybox_info .close_button.current,#fancybox_video .close_button:hover,#fancybox_video .close_button.active,#fancybox_video .close_button.current{background-position:-25px 0px}figure{margin-bottom:1em;max-width:100%}@media (min-width: 769px), print{figure{margin-bottom:2em}}figure figcaption,figure figcaption p{margin-top:.8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{figure figcaption,figure figcaption p{font-size:.88em}}.explore_overlay_page figcaption,.explore_overlay_page figcaption p{color:#b0b4b9}@media (max-width: 480px){figure.lede.full_width{width:100%}figure.lede.full_width figcaption{margin-left:auto;margin-right:auto}}#secondary_column aside figure{margin-bottom:1em}#secondary_column aside figure figcaption{margin-bottom:0}.inline_caption{margin-top:.8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.inline_caption{font-size:.88em}}.content_page #page_header{margin-bottom:1.5em}@media (min-width: 769px), print{.content_page #page_header{margin-bottom:2em}}.content_page #page_header .author{margin:.5em 0 1.8em}.content_page .release_date{font-size:1em;color:#222;text-transform:none}.content_page .category_title{color:#222}.content_page .category_title a{color:#257cdf}.content_page .audio_player{margin-bottom:1em}.content_page .main_feature .master-slider,.content_page .jpl_carousel .master-slider{width:100%;height:300px}@media (min-width: 600px), print{.content_page .main_feature .master-slider,.content_page .jpl_carousel .master-slider{height:400px}}.content_page .main_feature .master-slider .gradient_container_bottom,.content_page .jpl_carousel .master-slider .gradient_container_bottom{height:80px}.content_page .main_feature .master-slider .ms-nav-next,.content_page .main_feature .master-slider .ms-nav-prev,.content_page .jpl_carousel .master-slider .ms-nav-next,.content_page .jpl_carousel .master-slider .ms-nav-prev{display:none}@media (min-width: 769px), print{.content_page .main_feature .master-slider .ms-nav-next,.content_page .main_feature .master-slider .ms-nav-prev,.content_page .jpl_carousel .master-slider .ms-nav-next,.content_page .jpl_carousel .master-slider .ms-nav-prev{display:block}}.content_page .main_feature .master-slider .ms-bullets,.content_page .jpl_carousel .master-slider .ms-bullets{bottom:30px}.content_page .main_feature .master-slider .ms-bullets-count,.content_page .jpl_carousel .master-slider .ms-bullets-count{right:-50%;position:absolute}.content_page .main_feature .master-slider .ms-bullet,.content_page .jpl_carousel .master-slider .ms-bullet{background-color:white;background-image:none;border-radius:50% 50% 50% 50%;height:10px;width:10px;opacity:0.5;margin:0 10px}.content_page .main_feature .master-slider .ms-bullet:hover,.content_page .main_feature .master-slider .ms-bullet.ms-bullet-selected,.content_page .jpl_carousel .master-slider .ms-bullet:hover,.content_page .jpl_carousel .master-slider .ms-bullet.ms-bullet-selected{opacity:1.0}.content_page #primary_column{margin-bottom:5.26316%}@media (min-width: 600px), print{.content_page #primary_column{width:61.53846%;float:left;margin-right:2.5641%;margin-bottom:0}}@media (min-width: 769px), print{.content_page #primary_column{width:64.40678%;float:left;margin-right:1.69492%}}@media (min-width: 1024px), print{.content_page #primary_column{width:61.86441%;float:left;margin-right:1.69492%}}@media (min-width: 1200px){.content_page #primary_column{width:59.32203%;float:left;margin-right:1.69492%}}@media (min-width: 600px), print{.content_page #secondary_column{width:35.89744%;float:right;margin-right:0}}@media (min-width: 769px), print{.content_page #secondary_column{width:32.20339%;float:right;margin-right:0}}.content_page.full_width #primary_column,.content_page.full_width #secondary_column{width:100%}.content_page.feature{padding:2em 0 5.3em}.content_page.feature #secondary_column{display:none}.content_page.feature #primary_column{width:64.40678%;float:left;margin-right:1.69492%;margin:auto;padding:1em 0 5.3em;float:none}#secondary_column&gt;:first-child{margin-top:0}#secondary_column{word-wrap:break-word}#secondary_column aside{margin-bottom:7.14286%}#secondary_column aside:last-child{margin-bottom:0}#secondary_column aside.boxed{border:1px solid #C1C1C1;padding:5.26316%}#secondary_column aside.none{border:0;padding:0}#secondary_column aside&gt;:last-child{margin-bottom:0}#secondary_column aside.links_module li{margin-bottom:.5em}#secondary_column aside.downloads_module .download{margin-bottom:1em}#secondary_column aside.downloads_module .download:last-of-type{margin-bottom:0}#secondary_column aside.downloads_module .button,#secondary_column aside.downloads_module .outline_button{margin-top:1em}#secondary_column aside.list_view_module a{text-decoration:none}#secondary_column aside.list_view_module ul{margin-bottom:1.5em}#secondary_column aside.list_view_module li{padding:.6em 0}#secondary_column aside.list_view_module li:last-child{padding-bottom:0}#secondary_column aside.list_view_module .list_image{float:right;margin-left:4%;margin-bottom:.5em;width:32%}@media (min-width: 600px), print{#secondary_column aside.list_view_module .list_image{margin-left:0;margin-bottom:0;float:left;width:31.03448%;float:left;margin-right:3.44828%}}@media (min-width: 600px), print{#secondary_column aside.list_view_module .list_text{width:65.51724%;float:right;margin-right:0}}#secondary_column aside.list_view_module .list_title{letter-spacing:-.01em;font-weight:700}#secondary_column aside.list_view_module .list_title:hover{color:#222}#secondary_column aside.sig_events_module h4{margin-bottom:1em}#secondary_column aside.sig_events_module h4:last-child{margin-bottom:0}#secondary_column aside.sig_events_module ul{margin-bottom:0}#secondary_column aside.sig_events_module ul li{margin-bottom:.5em}#secondary_column .inline_image{margin-bottom:7.14286%}#secondary_column .inline_image .inline_caption{display:block;margin-top:.8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{#secondary_column .inline_image .inline_caption{font-size:.88em}}#secondary_column .related_content_module{margin:0 0 7.14286% 0;padding:5.26316%;width:100%;border:1px solid #bebebe}#secondary_column .related_content_module li{width:100%;border-bottom:1px solid #bebebe}#secondary_column .related_content_module li:last-child{border-bottom:none;padding-bottom:0}#secondary_column .related_content_module li:first-child{border-top:none}#secondary_column .related_content_module&gt;:last-child{margin-bottom:0}a.main_image_enlarge,a.inline_image_enlarge{display:block;position:relative;height:100%}a.main_image_enlarge .enlarge_icon,a.inline_image_enlarge .enlarge_icon{position:absolute;border-radius:6px;border:1px solid rgba(200,200,200,0.8);left:15px;bottom:15px;width:40px;height:40px;background-color:rgba(0,0,0,0.5);background-image:url("https://mars.nasa.gov/assets/zoom_icon.png");background-size:50%;background-repeat:no-repeat;background-position:50%;opacity:0;transition:opacity 0.2s ease-in}a.main_image_enlarge:hover .enlarge_icon,a.inline_image_enlarge:hover .enlarge_icon{opacity:0.8}body #fancybox-lock{z-index:200000}.article_nav{display:none}@media (min-width: 1024px), print{.article_nav{display:block;position:relative;z-index:11}.article_nav .article_nav_block{position:fixed;height:86px;display:inline-block;top:42.5%}.article_nav .article_nav_block .link_box{width:40px;background-color:#e4e9ef;display:inline;height:100%}.article_nav .article_nav_block .article_details{display:inline;width:250px;background-color:#FFF;text-decoration:none;color:#000;padding:10px;background-color:#e4e9ef}.article_nav .article_nav_block .article_details .img{margin-bottom:6px}.article_nav .article_nav_block .article_details .title{font-weight:700;font-size:.9em}.article_nav .article_nav_block.prev{left:0}.article_nav .article_nav_block.prev .link_box{float:left}.article_nav .article_nav_block.prev .article_details{float:left;display:none}.article_nav .article_nav_block.next{right:0}.article_nav .article_nav_block.next .link_box{float:right}.article_nav .article_nav_block.next .article_details{display:none;float:right}.no-touchevents .article_nav .article_nav_block:hover .article_details{display:block}}.feature_pages{padding:1em 0 3.8em}.feature_pages #page_header{width:94%;max-width:600px;margin-left:auto;margin-right:auto}@media (min-width: 769px), print{.feature_pages #page_header{width:80%}}@media (min-width: 1200px){.feature_pages #page_header{width:55%}}.feature_pages #primary_column{width:100%;margin:0}.feature_pages .wysiwyg_content&gt;*{width:94%;max-width:600px;margin-left:auto;margin-right:auto}@media (min-width: 769px), print{.feature_pages .wysiwyg_content&gt;*{width:80%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content&gt;*{width:55%}}.feature_pages .wysiwyg_content p{font-size:18px;line-height:28px}@media (min-width: 769px), print{.feature_pages .wysiwyg_content p{font-size:19px;line-height:30px}}.feature_pages .wysiwyg_content&gt;ul:not(.item_list_module):not(.item_grid_module),.feature_pages .wysiwyg_content&gt;ol{list-style-position:outside;padding-left:1em}.feature_pages .wysiwyg_content&gt;ul:not(.item_list_module):not(.item_grid_module) ul,.feature_pages .wysiwyg_content&gt;ul:not(.item_list_module):not(.item_grid_module) ol,.feature_pages .wysiwyg_content&gt;ol ul,.feature_pages .wysiwyg_content&gt;ol ol{list-style-position:outside}.top_feature_area{text-align:center;position:relative}.top_feature_area .header_overlay{position:absolute;width:100%;padding:0 1%;top:46%;color:white;transform:translateY(-50%)}@media (min-width: 769px), print{.top_feature_area .header_overlay{padding:0 4%}}.top_feature_area .header_overlay .article_title{font-size:1.2em;margin-bottom:0}@media (min-width: 480px){.top_feature_area .header_overlay .article_title{font-size:1.6em}}@media (min-width: 600px), print{.top_feature_area .header_overlay .article_title{font-size:1.9em}}@media (min-width: 769px), print{.top_feature_area .header_overlay .article_title{font-size:2.2em}}@media (min-width: 1024px), print{.top_feature_area .header_overlay .article_title{font-size:2.8em}}@media (min-width: 1200px){.top_feature_area .header_overlay .article_title{font-size:3.2em}}@media (min-width: 1700px){.top_feature_area .header_overlay .article_title{font-size:3.4em}}.top_feature_area .header_overlay .sub_title{font-size:1.2em}@media (min-width: 480px){.top_feature_area .header_overlay .sub_title{font-size:1.5em}}@media (min-width: 769px), print{.top_feature_area .header_overlay .sub_title{font-size:1.9em}}.top_feature_area .header_overlay .author{padding:0.2em 0.5em 0.5em 0.6em;background-color:rgba(0,0,0,0.5);margin:0.2em auto 1em;display:inline-block}@media (min-width: 480px){.top_feature_area .header_overlay .author{margin-top:0.4em}}@media (min-width: 600px), print{.top_feature_area .header_overlay .author{margin-top:0.7em}}@media (min-width: 769px), print{.top_feature_area .header_overlay .author{padding:0.3em 0.5em 0.5em 0.7em;max-width:360px;margin-top:1em}}@media (min-width: 1024px), print{.top_feature_area .header_overlay .author{padding:0.4em 0.6em 0.6em 0.8em;max-width:400px;margin-top:1.5em}}@media (min-width: 1200px){.top_feature_area .header_overlay .author{margin-top:1.8em}}.top_feature_area .header_overlay .author p{color:white;margin:0;font-size:0.8em}@media (min-width: 769px), print{.top_feature_area .header_overlay .author p{font-size:0.95em}}.top_feature_area .article_title{margin-bottom:0.9em}.top_feature_area .category_title{color:#707070;margin-bottom:1em}.top_feature_area a.category_title{color:#257cdf}.top_feature_area .feature_header:first-child{width:94%;max-width:600px;margin-left:auto;margin-right:auto;padding:3em 0 1.7em}@media (min-width: 769px), print{.top_feature_area .feature_header:first-child{width:80%}}@media (min-width: 1200px){.top_feature_area .feature_header:first-child{width:55%}}@media (min-width: 480px){.top_feature_area .feature_header:first-child{padding:4em 0 2em}}.top_feature_area .feature_header:first-child:after{content:"";display:block;height:1px;width:56%;border-bottom:5px solid;max-width:200px;margin:1.7em auto 0.3em}.top_feature_area .feature_header.no_main_image{padding-bottom:.9em}.top_feature_area .release_date{text-transform:none;color:#222;margin-left:0.1em}.top_feature_area .header_overlay+.feature_header{width:94%;max-width:600px;margin-left:auto;margin-right:auto;text-align:left}@media (min-width: 769px), print{.top_feature_area .header_overlay+.feature_header{width:80%}}@media (min-width: 1200px){.top_feature_area .header_overlay+.feature_header{width:55%}}.top_feature_area figure.lede.full_width figcaption{margin:.8em;text-align:left;font-size:1em}.feature_pages .wysiwyg_content .mb_expand{width:100%;max-width:none}.feature_pages .wysiwyg_content .mb_expand .expandable_element_link{width:94%;max-width:600px;margin-left:auto;margin-right:auto;display:block}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .mb_expand .expandable_element_link{width:80%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .mb_expand .expandable_element_link{width:55%}}.feature_pages .wysiwyg_content .mb_expand .expandable_element{display:none;max-width:none;width:100%}.feature_pages .wysiwyg_content .mb_expand .expandable_element&gt;*{width:94%;max-width:600px;margin-left:auto;margin-right:auto}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .mb_expand .expandable_element&gt;*{width:80%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .mb_expand .expandable_element&gt;*{width:55%}}.countdown .unit{font-weight:300;display:inline-block;position:relative;padding:0 9px 0 0;vertical-align:middle;text-align:center}.countdown .unit+.unit:before{content:" : ";position:absolute;left:-4px}.countdown .unit:first-of-type{padding-left:0}.countdown .unit:last-of-type{padding-right:0}.countdown .unit span{font-weight:600;padding:0 1em;clear:both;display:block;font-size:11px;text-transform:uppercase;text-align:center;margin-bottom:.3rem}.countdown .completed{font-size:1.8em;font-weight:300;margin-top:3px}.feature_pages .countdown,.content_page .countdown{border-top:1px solid #E8E8E8;border-bottom:1px solid #E8E8E8;text-align:center;padding:0.7em 0 0.9em;margin-top:2.7em;margin-bottom:2.7em}.feature_pages .countdown&gt;div,.content_page .countdown&gt;div{display:inline-block;vertical-align:middle}.feature_pages .countdown_title,.content_page .countdown_title{width:100%}@media (min-width: 600px), print{.feature_pages .countdown_title,.content_page .countdown_title{width:auto;margin-right:.8em}}#secondary_column .countdown{margin-bottom:7.14286%;text-align:left}#secondary_column .countdown .countdown_title{margin-right:0;width:100%}#explore_overlay .countdown{border-top-color:#212121;border-bottom-color:#212121}.curtain_module{margin:3em 0;clear:both}.curtain_module .curtain_caption_container{background-color:#eee;padding:1em}.curtain_module .curtain_title{margin:0 0 .5em 0}.curtain_module .curtain_subtitle{color:#777777;margin:0 0 0.5em 0}.feature_pages .curtain_module{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.feature_pages .curtain_module{max-width:600px}}.feature_pages .curtain_module.full-bleed,.feature_pages .curtain_module.full_width,.feature_pages .curtain_module.wide,.feature_pages .curtain_module.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .curtain_module.full-bleed,.feature_pages .curtain_module.full_width,.feature_pages .curtain_module.wide,.feature_pages .curtain_module.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .curtain_module.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .curtain_module.column-width{max-width:600px}}.feature_pages .curtain_module.full-bleed{width:100%;max-width:none}.feature_pages .curtain_module.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .curtain_module.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .curtain_module.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .curtain_module.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .curtain_module.full_width{width:55%}}.feature_pages .curtain_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .curtain_module.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .curtain_module.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .curtain_module.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .curtain_module.column-width{max-width:calc(600px + 15%)}}.feature_pages .curtain_module.left,.feature_pages .curtain_module.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .curtain_module.left,.feature_pages .curtain_module.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .curtain_module.left,.feature_pages .curtain_module.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .curtain_module.left,.feature_pages .curtain_module.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .curtain_module.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .curtain_module.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .curtain_module.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .curtain_module.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .curtain_module.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .curtain_module.right{margin-right:20%}}.feature_pages .curtain_module.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .curtain_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .curtain_module.parallax_module .caption{font-size:.88em}}.feature_pages .curtain_module.parallax_module img{height:auto !important}.feature_pages .curtain_module.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .curtain_module.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .curtain_module.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .curtain_module.parallax_module .window .featured_image{position:absolute}}.explore_overlay_page .curtain_module .curtain_caption_container{background-color:#232323}.wysiwyg_content .related_content_module,#secondary_column .related_content_module{font-weight:700}.wysiwyg_content .related_content_module ul,#secondary_column .related_content_module ul{margin:0}.wysiwyg_content .related_content_module li,#secondary_column .related_content_module li{padding:1em 0;border-bottom:1px solid #E5E5E5}.wysiwyg_content .related_content_module li:first-child,#secondary_column .related_content_module li:first-child{border-top:1px solid #E5E5E5}.wysiwyg_content .related_content_module .module_title,.wysiwyg_content .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .wysiwyg_content .related_content_module .carousel_title,#secondary_column .related_content_module .module_title,#secondary_column .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #secondary_column .related_content_module .carousel_title{font-size:1.2em;text-align:left;margin-top:0}.wysiwyg_content .related_content_module .list_image,#secondary_column .related_content_module .list_image{width:25%;display:inline-block}.wysiwyg_content .related_content_module .list_image+.list_text,#secondary_column .related_content_module .list_image+.list_text{width:67%;position:relative;display:inline-block;margin-left:4%;vertical-align:middle}.wysiwyg_content .related_content_module{max-width:100%;margin-top:1.4em;margin-bottom:1.4em}@media (min-width: 769px), print{.wysiwyg_content .related_content_module{margin-top:2em;margin-bottom:2em}}.wysiwyg_content .related_content_module.left,.wysiwyg_content .related_content_module.right{float:none}@media (min-width: 480px){.wysiwyg_content .related_content_module.left,.wysiwyg_content .related_content_module.right{max-width:50%}}@media (min-width: 1200px){.wysiwyg_content .related_content_module.left,.wysiwyg_content .related_content_module.right{max-width:40%}}@media (min-width: 480px){.wysiwyg_content .related_content_module.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.wysiwyg_content .related_content_module.right{float:right;margin:1em 0 1.5em 2.5em}}.wysiwyg_content .related_content_module.full-bleed,.wysiwyg_content .related_content_module.full_width,.wysiwyg_content .related_content_module.wide,.wysiwyg_content .related_content_module.parallax,.wysiwyg_content .related_content_module.column-width{clear:both}.wysiwyg_content .related_content_module.parallax_module{width:100%;position:relative}.wysiwyg_content .related_content_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.wysiwyg_content .related_content_module.parallax_module .caption{font-size:.88em}}.explore_overlay_page .wysiwyg_content .related_content_module.parallax_module .caption{color:#b0b4b9}.wysiwyg_content .related_content_module .sidebar_title,.wysiwyg_content #secondary_column .related_content_module .module_title,#secondary_column .wysiwyg_content .related_content_module .module_title,.wysiwyg_content #secondary_column .related_content_module .main_carousel.module .carousel_header .carousel_title,#secondary_column .wysiwyg_content .related_content_module .main_carousel.module .carousel_header .carousel_title,.wysiwyg_content .main_carousel.module .carousel_header #secondary_column .related_content_module .carousel_title,.main_carousel.module .carousel_header #secondary_column .wysiwyg_content .related_content_module .carousel_title,.wysiwyg_content .right_col .related_content_module .module_title,.right_col .wysiwyg_content .related_content_module .module_title,.wysiwyg_content .right_col .related_content_module .main_carousel.module .carousel_header .carousel_title,.right_col .wysiwyg_content .related_content_module .main_carousel.module .carousel_header .carousel_title,.wysiwyg_content .main_carousel.module .carousel_header .right_col .related_content_module .carousel_title,.main_carousel.module .carousel_header .right_col .wysiwyg_content .related_content_module .carousel_title{margin-top:0;font-size:1.5em}.wysiwyg_content .related_content_module.full_width{border:1px solid #D2D2D2;padding:5.26316%}@media (min-width: 600px), print{.wysiwyg_content .related_content_module.full_width{padding:2em}}.wysiwyg_content .related_content_module.full_width li{width:100%}.wysiwyg_content .related_content_module.full_width li:first-child{border-top:none}.wysiwyg_content .related_content_module.full_width li:last-child{border-bottom:transparent 0;padding-bottom:0}.wysiwyg_content .related_content_module.full_width .module_title,.wysiwyg_content .related_content_module.full_width .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .wysiwyg_content .related_content_module.full_width .carousel_title{margin-top:0;font-size:1.5em}.feature_pages .wysiwyg_content .related_content_module{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .related_content_module{max-width:600px}}.feature_pages .wysiwyg_content .related_content_module.full-bleed,.feature_pages .wysiwyg_content .related_content_module.full_width,.feature_pages .wysiwyg_content .related_content_module.wide,.feature_pages .wysiwyg_content .related_content_module.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .related_content_module.full-bleed,.feature_pages .wysiwyg_content .related_content_module.full_width,.feature_pages .wysiwyg_content .related_content_module.wide,.feature_pages .wysiwyg_content .related_content_module.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .wysiwyg_content .related_content_module.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .related_content_module.column-width{max-width:600px}}.feature_pages .wysiwyg_content .related_content_module.full-bleed{width:100%;max-width:none}.feature_pages .wysiwyg_content .related_content_module.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .wysiwyg_content .related_content_module.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .wysiwyg_content .related_content_module.full_width{width:55%}}.feature_pages .wysiwyg_content .related_content_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .related_content_module.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .related_content_module.column-width{max-width:calc(600px + 15%)}}.feature_pages .wysiwyg_content .related_content_module.left,.feature_pages .wysiwyg_content .related_content_module.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .related_content_module.left,.feature_pages .wysiwyg_content .related_content_module.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.left,.feature_pages .wysiwyg_content .related_content_module.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .related_content_module.left,.feature_pages .wysiwyg_content .related_content_module.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .related_content_module.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .related_content_module.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .related_content_module.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .wysiwyg_content .related_content_module.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .related_content_module.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .related_content_module.right{margin-right:20%}}.feature_pages .wysiwyg_content .related_content_module.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .wysiwyg_content .related_content_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.parallax_module .caption{font-size:.88em}}.feature_pages .wysiwyg_content .related_content_module.parallax_module img{height:auto !important}.feature_pages .wysiwyg_content .related_content_module.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .wysiwyg_content .related_content_module.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .wysiwyg_content .related_content_module.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .related_content_module.parallax_module .window .featured_image{position:absolute}}.feature_pages .wysiwyg_content .related_content_module .module_title,.feature_pages .wysiwyg_content .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .feature_pages .wysiwyg_content .related_content_module .carousel_title{margin-bottom:0.8em}.vital_signs .related_content_module{font-weight:normal;margin-bottom:1em}.vital_signs .related_content_module li{border:none;padding:0.4em 0;font-size:0.9em}.vital_signs .related_content_module .module_title,.vital_signs .related_content_module .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .vital_signs .related_content_module .carousel_title{font-size:1.2em;margin-bottom:.4em}.vital_signs .related_content_module .list_image{display:none}.vital_signs .related_content_module .list_text{width:100%;float:none;margin-left:10px}.vital_signs .related_content_module .list_text:before{content:"›";color:#42a0f2;margin-left:-10px}.explore_overlay_page .related_content_module.full_width{border-color:#353535}.explore_overlay_page .related_content_module ul li{border-bottom:1px solid #212121}.explore_overlay_page .related_content_module ul li:first-child{border-top:1px solid #212121}.wysiwyg_content .carousel_module{max-width:100%;margin-top:1.4em;margin-bottom:1.4em;overflow:hidden;clear:both;height:275px}@media (min-width: 769px), print{.wysiwyg_content .carousel_module{margin-top:2em;margin-bottom:2em}}.wysiwyg_content .carousel_module.left,.wysiwyg_content .carousel_module.right{float:none}@media (min-width: 480px){.wysiwyg_content .carousel_module.left,.wysiwyg_content .carousel_module.right{max-width:50%}}@media (min-width: 1200px){.wysiwyg_content .carousel_module.left,.wysiwyg_content .carousel_module.right{max-width:40%}}@media (min-width: 480px){.wysiwyg_content .carousel_module.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.wysiwyg_content .carousel_module.right{float:right;margin:1em 0 1.5em 2.5em}}.wysiwyg_content .carousel_module.full-bleed,.wysiwyg_content .carousel_module.full_width,.wysiwyg_content .carousel_module.wide,.wysiwyg_content .carousel_module.parallax,.wysiwyg_content .carousel_module.column-width{clear:both}.wysiwyg_content .carousel_module.parallax_module{width:100%;position:relative}.wysiwyg_content .carousel_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.wysiwyg_content .carousel_module.parallax_module .caption{font-size:.88em}}.explore_overlay_page .wysiwyg_content .carousel_module.parallax_module .caption{color:#b0b4b9}.wysiwyg_content .carousel_module .gradient_container_top{display:none}.wysiwyg_content .carousel_module.medium_mid{height:500px}.wysiwyg_content .carousel_module.medium_large,.wysiwyg_content .carousel_module.large{height:600px}.wysiwyg_content .carousel_module.xlarge,.wysiwyg_content .carousel_module.xxlarge{height:700px}.wysiwyg_content .carousel_module .master-slider{width:100%;height:100%}.wysiwyg_content .carousel_module:last-child{margin-bottom:0}.wysiwyg_content .carousel_module header{position:relative;margin-bottom:0;padding:0 22px}.wysiwyg_content .carousel_module header:after{background:url("https://mars.nasa.gov/assets/arrow_up_white_@2x.png") no-repeat;content:"";position:absolute;right:0;top:.9em;background-size:12px;height:7.5px;width:12px}.wysiwyg_content .carousel_module .media_feature_title{color:white;font-size:1.4em;margin:0}.wysiwyg_content .carousel_module .media_feature_title a{color:white;text-decoration:none}.wysiwyg_content .carousel_module .media_feature_title:hover{cursor:pointer}.wysiwyg_content .carousel_module .media_feature_title:after{background:url("https://mars.nasa.gov/assets/arrow_up_white_@2x.png") no-repeat;content:"";margin-left:14px;background-size:12px;height:7.5px;width:12px;display:inline-block}.wysiwyg_content .carousel_module .subtitle{font-size:1.1em;margin:0.4em 0 .4em}.wysiwyg_content .carousel_module .description{display:block;max-height:130px;overflow-y:auto}.wysiwyg_content .carousel_module .description a{color:#69B9FF}.wysiwyg_content .carousel_module.medium_large .description,.wysiwyg_content .carousel_module.large .description,.wysiwyg_content .carousel_module.xlarge .description,.wysiwyg_content .carousel_module.xxlarge .description{max-height:none;overflow:hidden;margin-bottom:0;display:none}.wysiwyg_content .carousel_module.medium_large .floating_text_area.open header,.wysiwyg_content .carousel_module.large .floating_text_area.open header,.wysiwyg_content .carousel_module.xlarge .floating_text_area.open header,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.open header{margin-bottom:.7em}.wysiwyg_content .carousel_module.medium_large .floating_text_area.open header .media_feature_title:after,.wysiwyg_content .carousel_module.large .floating_text_area.open header .media_feature_title:after,.wysiwyg_content .carousel_module.xlarge .floating_text_area.open header .media_feature_title:after,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.open header .media_feature_title:after{transform:rotate(180deg)}.wysiwyg_content .carousel_module.small .floating_text_area,.wysiwyg_content .carousel_module.medium .floating_text_area,.wysiwyg_content .carousel_module.medium_mid .floating_text_area{background:linear-gradient(transparent, rgba(0,0,0,0.6));background-size:100%;background-repeat:no-repeat;background-position:bottom}.wysiwyg_content .carousel_module.small header,.wysiwyg_content .carousel_module.medium header,.wysiwyg_content .carousel_module.medium_mid header{cursor:pointer;margin-bottom:.4em}.wysiwyg_content .carousel_module.small .description,.wysiwyg_content .carousel_module.medium .description,.wysiwyg_content .carousel_module.medium_mid .description{display:none;cursor:pointer}.wysiwyg_content .carousel_module.small .media_feature_title:after,.wysiwyg_content .carousel_module.medium .media_feature_title:after,.wysiwyg_content .carousel_module.medium_mid .media_feature_title:after{display:none}.wysiwyg_content .carousel_module.small .gradient_container_bottom,.wysiwyg_content .carousel_module.medium .gradient_container_bottom,.wysiwyg_content .carousel_module.medium_mid .gradient_container_bottom{display:none}.wysiwyg_content .carousel_module .floating_text_area{width:100%;padding:2em 1.4em 2.8em;bottom:0;text-align:center;margin-left:auto;margin-right:auto;color:white}.wysiwyg_content .carousel_module.medium_large .floating_text_area,.wysiwyg_content .carousel_module.large .floating_text_area,.wysiwyg_content .carousel_module.xlarge .floating_text_area,.wysiwyg_content .carousel_module.xxlarge .floating_text_area{padding:1.4em;text-align:left;bottom:5em;background-color:black;width:auto;max-width:500px}.wysiwyg_content .carousel_module.medium_large .floating_text_area.left,.wysiwyg_content .carousel_module.medium_large .floating_text_area.bottom_left,.wysiwyg_content .carousel_module.large .floating_text_area.left,.wysiwyg_content .carousel_module.large .floating_text_area.bottom_left,.wysiwyg_content .carousel_module.xlarge .floating_text_area.left,.wysiwyg_content .carousel_module.xlarge .floating_text_area.bottom_left,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.left,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.bottom_left{left:5%}.wysiwyg_content .carousel_module.medium_large .floating_text_area.right,.wysiwyg_content .carousel_module.medium_large .floating_text_area.bottom_right,.wysiwyg_content .carousel_module.large .floating_text_area.right,.wysiwyg_content .carousel_module.large .floating_text_area.bottom_right,.wysiwyg_content .carousel_module.xlarge .floating_text_area.right,.wysiwyg_content .carousel_module.xlarge .floating_text_area.bottom_right,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.right,.wysiwyg_content .carousel_module.xxlarge .floating_text_area.bottom_right{right:5%}.wysiwyg_content .carousel_module.medium_large .floating_text_area header,.wysiwyg_content .carousel_module.large .floating_text_area header,.wysiwyg_content .carousel_module.xlarge .floating_text_area header,.wysiwyg_content .carousel_module.xxlarge .floating_text_area header{padding:0}.wysiwyg_content .carousel_module.medium_large .floating_text_area header:after,.wysiwyg_content .carousel_module.large .floating_text_area header:after,.wysiwyg_content .carousel_module.xlarge .floating_text_area header:after,.wysiwyg_content .carousel_module.xxlarge .floating_text_area header:after{content:none}.wysiwyg_content .carousel_module .floating_text_area.open header:after{transform:rotate(180deg)}.wysiwyg_content .carousel_module .ms-nav-prev,.wysiwyg_content .carousel_module .ms-nav-next{margin-top:-40px}.wysiwyg_content .carousel_module .ms-slide-bgvideocont{background-color:#000}.wysiwyg_content .carousel_module .ms-slide-bgvideocont video{max-width:none}.wysiwyg_content .carousel_module .ms-nav-next,.wysiwyg_content .carousel_module .ms-nav-prev{display:none}.wysiwyg_content .carousel_module.medium_mid .ms-nav-next,.wysiwyg_content .carousel_module.medium_mid .ms-nav-prev,.wysiwyg_content .carousel_module.medium_large .ms-nav-next,.wysiwyg_content .carousel_module.medium_large .ms-nav-prev,.wysiwyg_content .carousel_module.large .ms-nav-next,.wysiwyg_content .carousel_module.large .ms-nav-prev,.wysiwyg_content .carousel_module.xlarge .ms-nav-next,.wysiwyg_content .carousel_module.xlarge .ms-nav-prev,.wysiwyg_content .carousel_module.xxlarge .ms-nav-next,.wysiwyg_content .carousel_module.xxlarge .ms-nav-prev{display:block}.no-touchevents .wysiwyg_content .carousel_module.medium .ms-nav-next,.no-touchevents .wysiwyg_content .carousel_module.medium .ms-nav-prev,.no-touchevents .wysiwyg_content .carousel_module.small .ms-nav-next,.no-touchevents .wysiwyg_content .carousel_module.small .ms-nav-prev{display:block}.wysiwyg_content .carousel_module .ms-nav-prev,.wysiwyg_content .carousel_module .ms-nav-next{width:40px;height:80px;margin-top:-60px}.wysiwyg_content .carousel_module.medium_large .ms-nav-next,.wysiwyg_content .carousel_module.medium_large .ms-nav-prev,.wysiwyg_content .carousel_module.large .ms-nav-next,.wysiwyg_content .carousel_module.large .ms-nav-prev,.wysiwyg_content .carousel_module.xlarge .ms-nav-next,.wysiwyg_content .carousel_module.xlarge .ms-nav-prev,.wysiwyg_content .carousel_module.xxlarge .ms-nav-next,.wysiwyg_content .carousel_module.xxlarge .ms-nav-prev{margin-top:-80px}.wysiwyg_content .carousel_module .ms-nav-prev{background:url("https://mars.nasa.gov/assets/arrow_left_darktheme.png");background-size:40px 95px;background-color:rgba(32,32,32,0.9);background-position:0;left:0;border-top-right-radius:6px;border-bottom-right-radius:6px}.wysiwyg_content .carousel_module .ms-nav-next{background:url("https://mars.nasa.gov/assets/arrow_right_darktheme.png");background-size:40px 95px;background-color:rgba(32,32,32,0.9);background-position:0;right:0;border-top-left-radius:6px;border-bottom-left-radius:6px}.wysiwyg_content .carousel_module .ms-bullets{left:0;right:0;margin:0 auto;bottom:1.2em;z-index:10}.wysiwyg_content .carousel_module.medium_mid .ms-bullets{bottom:1.5em}.wysiwyg_content .carousel_module.medium_large .ms-bullets,.wysiwyg_content .carousel_module.large .ms-bullets,.wysiwyg_content .carousel_module.xlarge .ms-bullets,.wysiwyg_content .carousel_module.xxlarge .ms-bullets{bottom:2.2em}.wysiwyg_content .carousel_module .ms-bullet{background-color:white;background-image:none;border-radius:50% 50% 50% 50%;height:8px;width:8px;opacity:0.5;margin:0 10px}.wysiwyg_content .carousel_module .ms-bullet:hover{opacity:1.0}.wysiwyg_content .carousel_module .ms-bullet-selected{opacity:1.0}.wysiwyg_content .carousel_module .ms-slide-layers{left:0 !important}.wysiwyg_content .carousel_module .ms-container,.wysiwyg_content .carousel_module .ms-slide-layers{max-width:none !important}.feature_pages .wysiwyg_content .carousel_module{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .carousel_module{max-width:600px}}.feature_pages .wysiwyg_content .carousel_module.full-bleed,.feature_pages .wysiwyg_content .carousel_module.full_width,.feature_pages .wysiwyg_content .carousel_module.wide,.feature_pages .wysiwyg_content .carousel_module.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .carousel_module.full-bleed,.feature_pages .wysiwyg_content .carousel_module.full_width,.feature_pages .wysiwyg_content .carousel_module.wide,.feature_pages .wysiwyg_content .carousel_module.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .wysiwyg_content .carousel_module.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .carousel_module.column-width{max-width:600px}}.feature_pages .wysiwyg_content .carousel_module.full-bleed{width:100%;max-width:none}.feature_pages .wysiwyg_content .carousel_module.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .wysiwyg_content .carousel_module.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .wysiwyg_content .carousel_module.full_width{width:55%}}.feature_pages .wysiwyg_content .carousel_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .carousel_module.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .carousel_module.column-width{max-width:calc(600px + 15%)}}.feature_pages .wysiwyg_content .carousel_module.left,.feature_pages .wysiwyg_content .carousel_module.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .carousel_module.left,.feature_pages .wysiwyg_content .carousel_module.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.left,.feature_pages .wysiwyg_content .carousel_module.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .carousel_module.left,.feature_pages .wysiwyg_content .carousel_module.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .carousel_module.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .carousel_module.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .carousel_module.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .wysiwyg_content .carousel_module.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .carousel_module.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .carousel_module.right{margin-right:20%}}.feature_pages .wysiwyg_content .carousel_module.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .wysiwyg_content .carousel_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.parallax_module .caption{font-size:.88em}}.feature_pages .wysiwyg_content .carousel_module.parallax_module img{height:auto !important}.feature_pages .wysiwyg_content .carousel_module.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .wysiwyg_content .carousel_module.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .wysiwyg_content .carousel_module.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .carousel_module.parallax_module .window .featured_image{position:absolute}}.explore_overlay_page .full_width .carousel_module{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.explore_overlay_page .full_width .carousel_module{max-width:600px}}.explore_overlay_page .full_width .carousel_module.full-bleed,.explore_overlay_page .full_width .carousel_module.full_width,.explore_overlay_page .full_width .carousel_module.wide,.explore_overlay_page .full_width .carousel_module.parallax{clear:both}@media (min-width: 600px), print{.explore_overlay_page .full_width .carousel_module.full-bleed,.explore_overlay_page .full_width .carousel_module.full_width,.explore_overlay_page .full_width .carousel_module.wide,.explore_overlay_page .full_width .carousel_module.parallax{margin-top:5em;margin-bottom:5em}}.explore_overlay_page .full_width .carousel_module.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.explore_overlay_page .full_width .carousel_module.column-width{max-width:600px}}.explore_overlay_page .full_width .carousel_module.full-bleed{width:100%;max-width:none}.explore_overlay_page .full_width .carousel_module.full-bleed figcaption{margin:.8em .8em 0 .8em}.explore_overlay_page .full_width .carousel_module.full_width{clear:both}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.explore_overlay_page .full_width .carousel_module.full_width{width:55%}}.explore_overlay_page .full_width .carousel_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.wide{width:95%}}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.explore_overlay_page .full_width .carousel_module.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.explore_overlay_page .full_width .carousel_module.column-width{max-width:calc(600px + 15%)}}.explore_overlay_page .full_width .carousel_module.left,.explore_overlay_page .full_width .carousel_module.right{max-width:94%}@media (min-width: 600px), print{.explore_overlay_page .full_width .carousel_module.left,.explore_overlay_page .full_width .carousel_module.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.left,.explore_overlay_page .full_width .carousel_module.right{width:27%;max-width:27%}}@media (min-width: 1700px){.explore_overlay_page .full_width .carousel_module.left,.explore_overlay_page .full_width .carousel_module.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.explore_overlay_page .full_width .carousel_module.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.explore_overlay_page .full_width .carousel_module.left{margin-left:15%}}@media (min-width: 1700px){.explore_overlay_page .full_width .carousel_module.left{margin-left:20%}}@media (min-width: 480px){.explore_overlay_page .full_width .carousel_module.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.explore_overlay_page .full_width .carousel_module.right{margin-right:15%}}@media (min-width: 1700px){.explore_overlay_page .full_width .carousel_module.right{margin-right:20%}}.explore_overlay_page .full_width .carousel_module.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.explore_overlay_page .full_width .carousel_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.parallax_module .caption{font-size:.88em}}.explore_overlay_page .full_width .carousel_module.parallax_module img{height:auto !important}.explore_overlay_page .full_width .carousel_module.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.explore_overlay_page .full_width .carousel_module.parallax_module .window.mobile{height:auto;min-height:100%}.explore_overlay_page .full_width .carousel_module.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.parallax_module .window .featured_image{position:absolute}}.explore_overlay_page .full_width .carousel_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.explore_overlay_page .full_width .carousel_module.wide{width:95%;max-width:1200px}}.wysiwyg_content .image_module{max-width:100%;margin-top:1.4em;margin-bottom:1.4em}@media (min-width: 769px), print{.wysiwyg_content .image_module{margin-top:2em;margin-bottom:2em}}.wysiwyg_content .image_module.left,.wysiwyg_content .image_module.right{float:none}@media (min-width: 480px){.wysiwyg_content .image_module.left,.wysiwyg_content .image_module.right{max-width:50%}}@media (min-width: 1200px){.wysiwyg_content .image_module.left,.wysiwyg_content .image_module.right{max-width:40%}}@media (min-width: 480px){.wysiwyg_content .image_module.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.wysiwyg_content .image_module.right{float:right;margin:1em 0 1.5em 2.5em}}.wysiwyg_content .image_module.full-bleed,.wysiwyg_content .image_module.full_width,.wysiwyg_content .image_module.wide,.wysiwyg_content .image_module.parallax,.wysiwyg_content .image_module.column-width{clear:both}.wysiwyg_content .image_module.parallax_module{width:100%;position:relative}.wysiwyg_content .image_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.wysiwyg_content .image_module.parallax_module .caption{font-size:.88em}}.explore_overlay_page .wysiwyg_content .image_module.parallax_module .caption{color:#b0b4b9}.feature_pages .wysiwyg_content .image_module{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .image_module{max-width:600px}}.feature_pages .wysiwyg_content .image_module.full-bleed,.feature_pages .wysiwyg_content .image_module.full_width,.feature_pages .wysiwyg_content .image_module.wide,.feature_pages .wysiwyg_content .image_module.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .image_module.full-bleed,.feature_pages .wysiwyg_content .image_module.full_width,.feature_pages .wysiwyg_content .image_module.wide,.feature_pages .wysiwyg_content .image_module.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .wysiwyg_content .image_module.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .image_module.column-width{max-width:600px}}.feature_pages .wysiwyg_content .image_module.full-bleed{width:100%;max-width:none}.feature_pages .wysiwyg_content .image_module.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .wysiwyg_content .image_module.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .wysiwyg_content .image_module.full_width{width:55%}}.feature_pages .wysiwyg_content .image_module.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .image_module.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .image_module.column-width{max-width:calc(600px + 15%)}}.feature_pages .wysiwyg_content .image_module.left,.feature_pages .wysiwyg_content .image_module.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .image_module.left,.feature_pages .wysiwyg_content .image_module.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.left,.feature_pages .wysiwyg_content .image_module.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .image_module.left,.feature_pages .wysiwyg_content .image_module.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .image_module.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .image_module.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .image_module.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .wysiwyg_content .image_module.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .image_module.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .image_module.right{margin-right:20%}}.feature_pages .wysiwyg_content .image_module.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .wysiwyg_content .image_module.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.parallax_module .caption{font-size:.88em}}.feature_pages .wysiwyg_content .image_module.parallax_module img{height:auto !important}.feature_pages .wysiwyg_content .image_module.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .wysiwyg_content .image_module.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .wysiwyg_content .image_module.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .image_module.parallax_module .window .featured_image{position:absolute}}.image_module figure.inline_figure{margin-bottom:0}.item_grid_module{clear:both;margin:3em auto}@media (min-width: 769px), print{.item_grid_module{margin:4em auto}}.item_grid_module li{visibility:hidden}.item_grid_module .grid_content{margin-bottom:20px}.item_grid_module .grid_content .grid_image{text-align:center}@media (min-width: 600px), print{.item_grid_module .grid_content .grid_image{text-align:left}}.item_grid_module .grid_content .grid_image img{width:auto}@media (min-width: 600px), print{.item_grid_module .grid_content .grid_image img{width:100%}}.item_grid_module .grid_content .caption{font-size:.88em;padding:1em;color:#565455}@media (min-width: 600px), print{.item_grid_module .grid_content .caption{background-color:#D8D6D7}}.feature_pages .wysiwyg_content .item_grid_module{max-width:none;width:90% !important;left:5%;margin-left:auto;margin-right:auto}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .item_grid_module{width:98% !important;left:1%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .item_grid_module{width:95% !important;left:2.5%}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .item_grid_module{width:85% !important;left:0}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .item_grid_module{width:75% !important}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .item_grid_module{width:65% !important}}.explore_overlay_page .item_grid_module .caption{background-color:transparent;color:#9C9FA4}.grid_gallery.list_view .item_grid{display:none}.grid_gallery.masonry_view .item_list{display:none}.grid_gallery.masonry_view .grid_text .caption{background-color:#e5ecf4}.grid_gallery.masonry_view .detail_link_button{position:absolute;top:50%;left:50%;transform:translate(-50%, -50%);padding:0.7em 1.5em;border:2px solid white;border-radius:12px;font-size:0.9em;font-weight:bold}.view_selectors .nav_item.masonry_icon{background-position:-62px -62px}.no-touchevents .view_selectors .nav_item.masonry_icon:hover{background-position:-62px -12px}.masonry_view .view_selectors .nav_item.masonry_icon{background-position:-62px -12px}blockquote{clear:both;color:#000}blockquote .quote{font-style:italic;margin-bottom:.5em;font-size:1.5em;line-height:1.4em;font-weight:700}blockquote footer{font-size:1em;text-align:left}blockquote cite{font-style:normal}.explore_overlay_page blockquote{color:#FFF !important}.explore_overlay_page blockquote.inspirational{color:#FFF}.feature_pages .wysiwyg_content blockquote{width:80%}@media (min-width: 769px), print{.feature_pages .wysiwyg_content blockquote{width:65%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content blockquote{width:40%}}.wysiwyg_content .video_player_container,.wysiwyg_content figure.embedded_video{max-width:100%;margin-top:1.4em;margin-bottom:1.4em}@media (min-width: 769px), print{.wysiwyg_content .video_player_container,.wysiwyg_content figure.embedded_video{margin-top:2em;margin-bottom:2em}}.wysiwyg_content .video_player_container.left,.wysiwyg_content .video_player_container.right,.wysiwyg_content figure.embedded_video.left,.wysiwyg_content figure.embedded_video.right{float:none}@media (min-width: 480px){.wysiwyg_content .video_player_container.left,.wysiwyg_content .video_player_container.right,.wysiwyg_content figure.embedded_video.left,.wysiwyg_content figure.embedded_video.right{max-width:50%}}@media (min-width: 1200px){.wysiwyg_content .video_player_container.left,.wysiwyg_content .video_player_container.right,.wysiwyg_content figure.embedded_video.left,.wysiwyg_content figure.embedded_video.right{max-width:40%}}@media (min-width: 480px){.wysiwyg_content .video_player_container.left,.wysiwyg_content figure.embedded_video.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.wysiwyg_content .video_player_container.right,.wysiwyg_content figure.embedded_video.right{float:right;margin:1em 0 1.5em 2.5em}}.wysiwyg_content .video_player_container.full-bleed,.wysiwyg_content .video_player_container.full_width,.wysiwyg_content .video_player_container.wide,.wysiwyg_content .video_player_container.parallax,.wysiwyg_content .video_player_container.column-width,.wysiwyg_content figure.embedded_video.full-bleed,.wysiwyg_content figure.embedded_video.full_width,.wysiwyg_content figure.embedded_video.wide,.wysiwyg_content figure.embedded_video.parallax,.wysiwyg_content figure.embedded_video.column-width{clear:both}.wysiwyg_content .video_player_container.parallax_module,.wysiwyg_content figure.embedded_video.parallax_module{width:100%;position:relative}.wysiwyg_content .video_player_container.parallax_module .caption,.wysiwyg_content figure.embedded_video.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.wysiwyg_content .video_player_container.parallax_module .caption,.wysiwyg_content figure.embedded_video.parallax_module .caption{font-size:.88em}}.explore_overlay_page .wysiwyg_content .video_player_container.parallax_module .caption,.explore_overlay_page .wysiwyg_content figure.embedded_video.parallax_module .caption{color:#b0b4b9}.feature_pages .wysiwyg_content .video_player_container,.feature_pages .wysiwyg_content figure.embedded_video{width:94%;max-width:100%;margin:3em auto;float:none}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container,.feature_pages .wysiwyg_content figure.embedded_video{max-width:600px}}.feature_pages .wysiwyg_content .video_player_container.full-bleed,.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content .video_player_container.wide,.feature_pages .wysiwyg_content .video_player_container.parallax,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed,.feature_pages .wysiwyg_content figure.embedded_video.full_width,.feature_pages .wysiwyg_content figure.embedded_video.wide,.feature_pages .wysiwyg_content figure.embedded_video.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container.full-bleed,.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content .video_player_container.wide,.feature_pages .wysiwyg_content .video_player_container.parallax,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed,.feature_pages .wysiwyg_content figure.embedded_video.full_width,.feature_pages .wysiwyg_content figure.embedded_video.wide,.feature_pages .wysiwyg_content figure.embedded_video.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .wysiwyg_content .video_player_container.column-width,.feature_pages .wysiwyg_content figure.embedded_video.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container.column-width,.feature_pages .wysiwyg_content figure.embedded_video.column-width{max-width:600px}}.feature_pages .wysiwyg_content .video_player_container.full-bleed,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed{width:100%;max-width:none}.feature_pages .wysiwyg_content .video_player_container.full-bleed figcaption,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content figure.embedded_video.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content figure.embedded_video.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content figure.embedded_video.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .wysiwyg_content .video_player_container.full_width,.feature_pages .wysiwyg_content figure.embedded_video.full_width{width:55%}}.feature_pages .wysiwyg_content .video_player_container.wide,.feature_pages .wysiwyg_content figure.embedded_video.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.wide,.feature_pages .wysiwyg_content figure.embedded_video.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.column-width,.feature_pages .wysiwyg_content figure.embedded_video.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .video_player_container.column-width,.feature_pages .wysiwyg_content figure.embedded_video.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .video_player_container.column-width,.feature_pages .wysiwyg_content figure.embedded_video.column-width{max-width:calc(600px + 15%)}}.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content figure.embedded_video.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content figure.embedded_video.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content figure.embedded_video.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.right{margin-right:20%}}.feature_pages .wysiwyg_content .video_player_container.parallax_module,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .wysiwyg_content .video_player_container.parallax_module .caption,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.parallax_module .caption,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .caption{font-size:.88em}}.feature_pages .wysiwyg_content .video_player_container.parallax_module img,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module img{height:auto !important}.feature_pages .wysiwyg_content .video_player_container.parallax_module .window,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .wysiwyg_content .video_player_container.parallax_module .window.mobile,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .wysiwyg_content .video_player_container.parallax_module .window .featured_image,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.parallax_module .window .featured_image,.feature_pages .wysiwyg_content figure.embedded_video.parallax_module .window .featured_image{position:absolute}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:44%}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:33%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .video_player_container.left,.feature_pages .wysiwyg_content .video_player_container.right,.feature_pages .wysiwyg_content figure.embedded_video.left,.feature_pages .wysiwyg_content figure.embedded_video.right{width:25%}}.feature_pages .wysiwyg_content .video_player_container.full-width&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.full-bleed&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.wide&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-width&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.wide&gt;iframe{max-height:98vh}@media (min-height: 400px){.feature_pages .wysiwyg_content .video_player_container.full-width&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.full-bleed&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.wide&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-width&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.wide&gt;iframe{max-height:300px}}@media (min-height: 600px){.feature_pages .wysiwyg_content .video_player_container.full-width&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.full-bleed&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.wide&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-width&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.wide&gt;iframe{max-height:400px}}@media (min-height: 800px){.feature_pages .wysiwyg_content .video_player_container.full-width&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.full-bleed&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.wide&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-width&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.wide&gt;iframe{max-height:600px}}@media (min-height: 1000px){.feature_pages .wysiwyg_content .video_player_container.full-width&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.full-bleed&gt;iframe,.feature_pages .wysiwyg_content .video_player_container.wide&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-width&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.full-bleed&gt;iframe,.feature_pages .wysiwyg_content figure.embedded_video.wide&gt;iframe{max-height:90vh}}.wysiwyg_content figure.embedded_video&gt;iframe{width:100%;min-height:300px}@media (min-width: 769px), print{.wysiwyg_content figure.embedded_video&gt;iframe{min-height:400px}}@media (min-width: 1200px){.wysiwyg_content figure.embedded_video&gt;iframe{min-height:500px}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content .embedded_video iframe.webvr_module{min-height:400px}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content .embedded_video iframe.webvr_module{min-height:500px}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .embedded_video iframe.webvr_module{min-height:600px}}@media (min-width: 1700px){.feature_pages .wysiwyg_content .embedded_video iframe.webvr_module{min-height:700px}}.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.left,.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.right{width:100%;max-width:100%}.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.left&gt;iframe,.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.right&gt;iframe{min-height:300px}@media (min-width: 769px), print{.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.left,.content_page:not(.feature_pages) .wysiwyg_content figure.embedded_video.right{width:50%}}.content_page:not(.feature_pages) .wysiwyg_content .video_player_container.left,.content_page:not(.feature_pages) .wysiwyg_content .video_player_container.right{width:100%;max-width:100%}@media (min-width: 769px), print{.content_page:not(.feature_pages) .wysiwyg_content .video_player_container.left,.content_page:not(.feature_pages) .wysiwyg_content .video_player_container.right{width:50%}}.webvr-button{padding:0px !important;margin:12px}img[title="Configure viewer"]{margin-left:-12px !important}.feature_pages .wysiwyg_content #scene_container{width:94%;max-width:100%;margin:3em auto;float:none;position:relative}@media (min-width: 600px), print{.feature_pages .wysiwyg_content #scene_container{max-width:600px}}.feature_pages .wysiwyg_content #scene_container.full-bleed,.feature_pages .wysiwyg_content #scene_container.full_width,.feature_pages .wysiwyg_content #scene_container.wide,.feature_pages .wysiwyg_content #scene_container.parallax{clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content #scene_container.full-bleed,.feature_pages .wysiwyg_content #scene_container.full_width,.feature_pages .wysiwyg_content #scene_container.wide,.feature_pages .wysiwyg_content #scene_container.parallax{margin-top:5em;margin-bottom:5em}}.feature_pages .wysiwyg_content #scene_container.column-width{max-width:94%;margin-top:3em;margin-bottom:3em;clear:both}@media (min-width: 600px), print{.feature_pages .wysiwyg_content #scene_container.column-width{max-width:600px}}.feature_pages .wysiwyg_content #scene_container.full-bleed{width:100%;max-width:none}.feature_pages .wysiwyg_content #scene_container.full-bleed figcaption{margin:.8em .8em 0 .8em}.feature_pages .wysiwyg_content #scene_container.full_width{clear:both}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.full_width{width:94%;max-width:600px;margin-left:auto;margin-right:auto}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px), print and (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.full_width{width:80%}}@media (min-width: 769px) and (min-width: 1200px), print and (min-width: 1200px){.feature_pages .wysiwyg_content #scene_container.full_width{width:55%}}.feature_pages .wysiwyg_content #scene_container.wide{width:98%;max-width:none}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.wide{width:95%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.column-width{max-width:calc(600px + 6%)}}@media (min-width: 1024px), print{.feature_pages .wysiwyg_content #scene_container.column-width{max-width:calc(600px + 10%)}}@media (min-width: 1200px){.feature_pages .wysiwyg_content #scene_container.column-width{max-width:calc(600px + 15%)}}.feature_pages .wysiwyg_content #scene_container.left,.feature_pages .wysiwyg_content #scene_container.right{max-width:94%}@media (min-width: 600px), print{.feature_pages .wysiwyg_content #scene_container.left,.feature_pages .wysiwyg_content #scene_container.right{width:50%;max-width:50%}}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.left,.feature_pages .wysiwyg_content #scene_container.right{width:27%;max-width:27%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content #scene_container.left,.feature_pages .wysiwyg_content #scene_container.right{width:25%;max-width:25%}}@media (min-width: 600px), print{.feature_pages .wysiwyg_content #scene_container.left{float:left;margin:1em 2.5em 1.5em 0;margin-left:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content #scene_container.left{margin-left:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content #scene_container.left{margin-left:20%}}@media (min-width: 480px){.feature_pages .wysiwyg_content #scene_container.right{float:right;margin:1em 0 1.5em 2.5em;margin-right:3%}}@media (min-width: 1200px){.feature_pages .wysiwyg_content #scene_container.right{margin-right:15%}}@media (min-width: 1700px){.feature_pages .wysiwyg_content #scene_container.right{margin-right:20%}}.feature_pages .wysiwyg_content #scene_container.parallax_module{position:relative;overflow:hidden;z-index:10;padding-bottom:0;width:100%;max-width:none}.feature_pages .wysiwyg_content #scene_container.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.parallax_module .caption{font-size:.88em}}.feature_pages .wysiwyg_content #scene_container.parallax_module img{height:auto !important}.feature_pages .wysiwyg_content #scene_container.parallax_module .window{width:100%;height:auto;position:absolute;overflow:hidden;padding:2em}.feature_pages .wysiwyg_content #scene_container.parallax_module .window.mobile{height:auto;min-height:100%}.feature_pages .wysiwyg_content #scene_container.parallax_module .window .featured_image{z-index:9;top:0;left:0;height:100%;overflow:hidden}@media (min-width: 769px), print{.feature_pages .wysiwyg_content #scene_container.parallax_module .window .featured_image{position:absolute}}.feature_pages .wysiwyg_content #scene_container:before{display:block;content:"";width:100%;padding-top:75%}.feature_pages .wysiwyg_content #scene_container&gt;.content{position:absolute;top:0;left:0;right:0;bottom:0}.content_page:not(.feature_pages) .wysiwyg_content #scene_container{max-width:100%;margin-top:1.4em;margin-bottom:1.4em;position:relative}@media (min-width: 769px), print{.content_page:not(.feature_pages) .wysiwyg_content #scene_container{margin-top:2em;margin-bottom:2em}}.content_page:not(.feature_pages) .wysiwyg_content #scene_container.left,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.right{float:none}@media (min-width: 480px){.content_page:not(.feature_pages) .wysiwyg_content #scene_container.left,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.right{max-width:50%}}@media (min-width: 1200px){.content_page:not(.feature_pages) .wysiwyg_content #scene_container.left,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.right{max-width:40%}}@media (min-width: 480px){.content_page:not(.feature_pages) .wysiwyg_content #scene_container.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.content_page:not(.feature_pages) .wysiwyg_content #scene_container.right{float:right;margin:1em 0 1.5em 2.5em}}.content_page:not(.feature_pages) .wysiwyg_content #scene_container.full-bleed,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.full_width,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.wide,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.parallax,.content_page:not(.feature_pages) .wysiwyg_content #scene_container.column-width{clear:both}.content_page:not(.feature_pages) .wysiwyg_content #scene_container.parallax_module{width:100%;position:relative}.content_page:not(.feature_pages) .wysiwyg_content #scene_container.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.content_page:not(.feature_pages) .wysiwyg_content #scene_container.parallax_module .caption{font-size:.88em}}.explore_overlay_page .content_page:not(.feature_pages) .wysiwyg_content #scene_container.parallax_module .caption{color:#b0b4b9}.content_page:not(.feature_pages) .wysiwyg_content #scene_container:before{display:block;content:"";width:100%;padding-top:75%}.content_page:not(.feature_pages) .wysiwyg_content #scene_container&gt;.content{position:absolute;top:0;left:0;right:0;bottom:0}.content_page.atlas_detail #scene_container{max-width:100%;margin-top:1.4em;margin-bottom:1.4em;position:relative}@media (min-width: 769px), print{.content_page.atlas_detail #scene_container{margin-top:2em;margin-bottom:2em}}.content_page.atlas_detail #scene_container.left,.content_page.atlas_detail #scene_container.right{float:none}@media (min-width: 480px){.content_page.atlas_detail #scene_container.left,.content_page.atlas_detail #scene_container.right{max-width:50%}}@media (min-width: 1200px){.content_page.atlas_detail #scene_container.left,.content_page.atlas_detail #scene_container.right{max-width:40%}}@media (min-width: 480px){.content_page.atlas_detail #scene_container.left{float:left;margin:1em 2.5em 1.5em 0}}@media (min-width: 480px){.content_page.atlas_detail #scene_container.right{float:right;margin:1em 0 1.5em 2.5em}}.content_page.atlas_detail #scene_container.full-bleed,.content_page.atlas_detail #scene_container.full_width,.content_page.atlas_detail #scene_container.wide,.content_page.atlas_detail #scene_container.parallax,.content_page.atlas_detail #scene_container.column-width{clear:both}.content_page.atlas_detail #scene_container.parallax_module{width:100%;position:relative}.content_page.atlas_detail #scene_container.parallax_module .caption{margin:.8em .8em 0 .8em;font-size:.8em;color:#5a6470}@media (min-width: 769px), print{.content_page.atlas_detail #scene_container.parallax_module .caption{font-size:.88em}}.explore_overlay_page .content_page.atlas_detail #scene_container.parallax_module .caption{color:#b0b4b9}.content_page.atlas_detail #scene_container:before{display:block;content:"";width:100%;padding-top:36.36364%}.content_page.atlas_detail #scene_container&gt;.content{position:absolute;top:0;left:0;right:0;bottom:0}.magic_shell_page .wysiwyg_content #scene_container{height:100%}#scene_container{position:relative;overflow:hidden;width:100%}#scene_container .loading_webgl{width:100%;height:100%;top:0;left:0;right:0;bottom:0;position:absolute;z-index:21;opacity:1.0;color:white;background-color:black;text-align:center;-webkit-transition:opacity 2s, visibility 2s;-moz-transition:opacity 2s, visibility 2s;transition:opacity 2s, visibility 2s}#scene_container .loading_webgl .loading_text{position:relative;top:50%;padding-bottom:6px;color:grey}#scene_container .loading_webgl .load_bar{position:relative;top:50%;left:0;width:0%;height:2px;background-color:#FFFFFF;opacity:0.5}#gl_layer{margin:0 auto}#gl_layer,#gl_fallback_layer{-webkit-tap-highlight-color:transparent;-webkit-tap-highlight-color:transparent;position:absolute;overflow:hidden;top:0;left:0;right:0;bottom:0;width:100%;height:100%;display:none}#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{position:absolute;padding-top:8px;top:-120px;left:50%;width:90%;text-align:center;visibility:hidden;-webkit-transform:translateX(-50%);-ms-transform:translateX(-50%);transform:translateX(-50%);-webkit-transition:top 1s, visibility 1s;-moz-transition:top 1s, visibility 1s;transition:top 1s, visibility 1s;z-index:20}@media (min-width: 480px){#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{padding-top:26px}}@media only screen and (min-width: 480px) and (orientation: landscape){#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{padding-top:0px}}@media (min-width: 600px), print{#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{padding-top:34px}}@media only screen and (min-width: 600px) and (orientation: landscape){#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{padding-top:4px}}@media (min-width: 769px), print{#gl_layer .module_title_wrapper,#gl_fallback_layer .module_title_wrapper{padding-top:34px}}#gl_layer .module_title_wrapper.active,#gl_fallback_layer .module_title_wrapper.active{top:0px;visibility:visible}#gl_layer .module_title_wrapper .gradient_overlay_top,#gl_fallback_layer .module_title_wrapper .gradient_overlay_top{position:absolute;width:120%;height:250%;top:0;left:50%;opacity:.8;background:-moz-linear-gradient(top, #000 0%, #000 14%, transparent 99%, transparent 100%);background:-webkit-gradient(linear, left top, left bottom, color-stop(0%, #000), color-stop(14%, #000), color-stop(99%, transparent), color-stop(100%, transparent));background:-webkit-linear-gradient(top, #000 0%, #000 14%, transparent 99%, transparent 100%);background:-o-linear-gradient(top, #000 0%, #000 14%, transparent 99%, transparent 100%);background:-ms-linear-gradient(top, #000 0%, #000 14%, transparent 99%, transparent 100%);background:linear-gradient(to bottom, #000 0%, #000 14%, transparent 99%, transparent 100%);filter:progid:DXImageTransform.Microsoft.gradient( startColorstr='#000000', endColorstr='#00000000',GradientType=0 );-webkit-transform:translateX(-50%);-ms-transform:translateX(-50%);transform:translateX(-50%)}#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:20px;margin-right:0.4em;position:relative;color:#FFFFFF;font-family:Whitney,Helvetica,Arial,sans-serif;vertical-align:middle}@media (min-width: 480px){#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:22px}}@media (min-width: 600px), print{#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:34px}}@media only screen and (min-width: 600px) and (orientation: landscape){#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:24px}}@media (min-width: 769px), print{#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:28px}}@media (min-width: 1024px), print{#gl_layer .module_title_wrapper .module_title,#gl_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_layer .module_title_wrapper .carousel_title,#gl_fallback_layer .module_title_wrapper .module_title,#gl_fallback_layer .module_title_wrapper .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header #gl_fallback_layer .module_title_wrapper .carousel_title{font-size:34px}}#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{cursor:pointer;position:relative;display:inline-block;padding:.1em;width:26px;height:26px;vertical-align:middle}@media (min-width: 480px){#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{width:28px;height:28px}}@media (min-width: 600px), print{#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{width:36px;height:36px}}@media only screen and (min-width: 600px) and (orientation: landscape){#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{width:30px;height:30px}}@media (min-width: 769px), print{#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{width:36px;height:36px}}@media (min-width: 1024px), print{#gl_layer .module_title_wrapper a.close_focus,#gl_fallback_layer .module_title_wrapper a.close_focus{width:42px;height:42px}}#gl_layer .module_title_wrapper a.close_focus .close_icon,#gl_fallback_layer .module_title_wrapper a.close_focus .close_icon{display:block;height:100%;position:relative;opacity:0.7}#gl_layer .module_title_wrapper a.close_focus .close_icon:before,#gl_fallback_layer .module_title_wrapper a.close_focus .close_icon:before{transform:rotate(-45deg);content:'';position:absolute;height:3px;width:100%;top:calc(50% - 1.5px);left:0;background:#fff;opacity:.8}#gl_layer .module_title_wrapper a.close_focus .close_icon:after,#gl_fallback_layer .module_title_wrapper a.close_focus .close_icon:after{transform:rotate(45deg);content:'';position:absolute;height:3px;width:100%;top:calc(50% - 1.5px);left:0;background:#fff;opacity:.8}#gl_layer .module_title_wrapper a.close_focus:hover .close_icon,#gl_fallback_layer .module_title_wrapper a.close_focus:hover .close_icon{opacity:1.0}#gl_layer .module_description_wrapper,#gl_fallback_layer .module_description_wrapper{position:absolute;padding-bottom:26px;bottom:-110px;left:50%;width:90%;text-align:center;visibility:hidden;-webkit-transform:translateX(-50%);-ms-transform:translateX(-50%);transform:translateX(-50%);-webkit-transition:bottom 1s, visibility 1s;-moz-transition:bottom 1s, visibility 1s;transition:bottom 1s, visibility 1s;z-index:20}@media (min-width: 480px){#gl_layer .module_description_wrapper,#gl_fallback_layer .module_description_wrapper{padding-bottom:66px}}@media only screen and (min-width: 480px) and (orientation: landscape){#gl_layer .module_description_wrapper,#gl_fallback_layer .module_description_wrapper{padding-bottom:12px}}@media (min-width: 769px), print{#gl_layer .module_description_wrapper,#gl_fallback_layer .module_description_wrapper{padding-bottom:66px}}#gl_layer .module_description_wrapper.active,#gl_fallback_layer .module_description_wrapper.active{bottom:0px;visibility:visible}#gl_layer .module_description_wrapper .gradient_overlay,#gl_fallback_layer .module_description_wrapper .gradient_overlay{position:absolute;width:120%;height:250%;bottom:0;left:50%;opacity:.8;background:-moz-linear-gradient(top, transparent 0%, transparent 7%, #000 100%);background:-webkit-gradient(linear, left top, left bottom, color-stop(0%, transparent), color-stop(7%, transparent), color-stop(100%, #000));background:-webkit-linear-gradient(top, transparent 0%, transparent 7%, #000 100%);background:-o-linear-gradient(top, transparent 0%, transparent 7%, #000 100%);background:-ms-linear-gradient(top, transparent 0%, transparent 7%, #000 100%);background:linear-gradient(to bottom, transparent 0%, transparent 7%, #000 100%);filter:progid:DXImageTransform.Microsoft.gradient( startColorstr='#00000000', endColorstr='#000000',GradientType=0 );-webkit-transform:translateX(-50%);-ms-transform:translateX(-50%);transform:translateX(-50%)}#gl_layer .module_description_wrapper .module_description,#gl_fallback_layer .module_description_wrapper .module_description{padding:0px 12px;font-size:14px;font-weight:300;position:relative;z-index:10;color:#FFFFFF;font-family:Whitney,Helvetica,Arial,sans-serif}@media (min-width: 600px), print{#gl_layer .module_description_wrapper .module_description,#gl_fallback_layer .module_description_wrapper .module_description{font-size:20px}}@media only screen and (min-width: 600px) and (orientation: landscape){#gl_layer .module_description_wrapper .module_description,#gl_fallback_layer .module_description_wrapper .module_description{font-size:16px}}@media (min-width: 1200px){#gl_layer .module_description_wrapper .module_description,#gl_fallback_layer .module_description_wrapper .module_description{font-size:22px}}#gl_layer .module_description_wrapper .explore,#gl_fallback_layer .module_description_wrapper .explore{letter-spacing:.1em;font-family:Whitney-Bold,Helvetica,Arial,sans-serif;font-size:0;vertical-align:baseline;margin:0px;color:#78BDFF;background:rgba(0,0,0,0.1);border-radius:2px;cursor:pointer;position:relative;z-index:10}#gl_layer .module_description_wrapper .explore:hover,#gl_fallback_layer .module_description_wrapper .explore:hover{color:#CFE7FF;background:rgba(0,0,0,0.2)}#gl_layer .module_description_wrapper .explore:after,#gl_fallback_layer .module_description_wrapper .explore:after{content:"››";font-size:20px}@media (min-width: 1024px), print{#gl_layer .module_description_wrapper .explore,#gl_fallback_layer .module_description_wrapper .explore{font-size:17px;padding:10px;border:1px solid #78BDFF;vertical-align:middle}#gl_layer .module_description_wrapper .explore:after,#gl_fallback_layer .module_description_wrapper .explore:after{content:none}}#gl_layer .label{position:absolute;padding:0;border:0;margin:0;max-width:300px;text-align:center;-webkit-transform:translate(-50%, -110%);-ms-transform:translate(-50%, -110%);transform:translate(-50%, -110%);font-size:16px;color:#FFFFFF;font-family:Whitney,Helvetica,Arial,sans-serif}#gl_fallback_layer .focus_layer{position:absolute;top:0;right:0;width:100%;height:100%;background-size:cover;background-position:center center;visibility:hidden;opacity:0;-webkit-transition:opacity 1s, visibility 1s;-moz-transition:opacity 1s, visibility 1s;transition:opacity 1s, visibility 1s;z-index:15}#gl_fallback_layer .focus_layer.active{visibility:visible;opacity:1}#gl_fallback_layer .overlay_image{position:absolute;top:0;right:0;width:100%;height:100%;height:100%;background-size:cover;background-position:center center}.hotspot_container{position:absolute;height:100%;width:100%}.hotspot_container .hotspot_wrapper{position:absolute;top:0px;left:0px;width:350px;visibility:visible;-webkit-transform:translate(-12px, -12px);-ms-transform:translate(-12px, -12px);transform:translate(-12px, -12px)}.hotspot_container .hotspot_wrapper span{position:relative;visibility:visible;font-size:0.7em;color:#FFFFFF;opacity:0.0;transition:opacity 0.75s;left:6px}.hotspot_container .hotspot_wrapper.hidden{visibility:hidden}.hotspot_container .hotspot_wrapper .hotspot{width:24px;height:24px;opacity:0.8}.fallback_mode #gl_fallback_layer{height:auto}.fallback_mode #gl_fallback_layer img{height:auto}.image{height:300px;width:300px;border-color:#000000;border:4px solid #ffffff;border-radius:8px;transform:translate(-50%, -50%)}.wysiwyg_content p{line-height:1.4em}.wysiwyg_content p,.wysiwyg_content a{word-wrap:break-word}.wysiwyg_content table a{word-break:break-word}#primary_column .wysiwyg_content&gt;:first-child{margin-top:0}#primary_column .wysiwyg_content .inset_box{padding:.5em 2em;margin:2em 0;border:4px solid #DCE0E5}.wysiwyg_content h1,.wysiwyg_content h2,.wysiwyg_content h3,.wysiwyg_content h4{font-weight:700;letter-spacing:-.01em;margin:1.2em 0 .5em}@media (min-width: 769px), print{.wysiwyg_content h1,.wysiwyg_content h2,.wysiwyg_content h3,.wysiwyg_content h4{margin:1.5em 0 .5em}}.wysiwyg_content h1{font-size:2.2em}.wysiwyg_content h2{font-size:1.8em}.wysiwyg_content h3{font-size:1.4em}.wysiwyg_content h4{font-size:1.1em}.wysiwyg_content strong,.wysiwyg_content b,.wysiwyg_content .bold{font-weight:bold}.wysiwyg_content .content_title{font-size:1.04em;margin-bottom:.1em}@media (min-width: 600px), print{.wysiwyg_content .content_title{font-size:1.2em;margin-bottom:.18em}}@media (min-width: 769px), print{.wysiwyg_content .content_title{font-size:1.36em;margin-bottom:.26em}}@media (min-width: 1024px), print{.wysiwyg_content .content_title{font-size:1.44em;margin-bottom:.29em}}@media (min-width: 1200px){.wysiwyg_content .content_title{font-size:1.52em;margin-bottom:.32em}}.wysiwyg_content .article_teaser_body{font-size:1em}.wysiwyg_content .indent1{margin-left:3.5em}.wysiwyg_content .indent2{margin-left:7em}.wysiwyg_content .indent3{margin-left:10.5em}.wysiwyg_content .publish_date{font-weight:700}.wysiwyg_content .item_list_module{clear:both}.wysiwyg_content .expandable_element_link.style_1{font-size:.8em;font-weight:700;text-transform:uppercase}.wysiwyg_content .expandable_element{display:none}.wysiwyg_content table{border-spacing:1px;border-collapse:separate;font-size:15px;line-height:normal}.wysiwyg_content table th,.wysiwyg_content table td{padding:13px}.wysiwyg_content table th{background-color:#ddd;font-weight:600;text-align:left}.wysiwyg_content table td{background-color:#eee}.wysiwyg_content .table_wrapper{width:100%;margin:1em 0;-webkit-overflow-scrolling:touch}.wysiwyg_content .table_wrapper&gt;div::-webkit-scrollbar{height:12px}.wysiwyg_content .table_wrapper&gt;div::-webkit-scrollbar-track{box-shadow:0 0 2px rgba(0,0,0,0.15) inset;background:#f0f0f0}.wysiwyg_content .table_wrapper&gt;div::-webkit-scrollbar-thumb{border-radius:6px;background:#ccc}.wysiwyg_content .table_wrapper.has-scroll{position:relative;overflow:hidden}.wysiwyg_content .table_wrapper.has-scroll:after{position:absolute;top:0;left:100%;width:50px;height:100%;border-radius:10px 0 0 10px / 50% 0 0 50%;box-shadow:-5px 0 10px rgba(0,0,0,0.25);content:''}.wysiwyg_content .table_wrapper.has-scroll&gt;div{overflow-x:auto}.wysiwyg_content table.mb_table{border-collapse:collapse;width:100%}.wysiwyg_content table.mb_table td{background-color:transparent}.wysiwyg_content table.mb_table th{background-color:white;color:#f08d77;font-size:.75em;font-weight:500;text-align:left;padding:13px}@media (min-width: 600px), print{.wysiwyg_content table.mb_table th{font-size:.9em}}.wysiwyg_content table.mb_table tbody td{font-size:.9em}@media (min-width: 600px), print{.wysiwyg_content table.mb_table tbody td{font-size:1.1em}}.wysiwyg_content table.mb_table tr:nth-child(even){background-color:#edf4fb}.wysiwyg_content table.mb_table tr:nth-child(odd){background-color:#ffffff}.wysiwyg_content table.mb_table td{border:1px solid #d2d2d2;padding:.8em}.wysiwyg_content table.mb_table td:first-child{border-left:transparent}.wysiwyg_content table.mb_table td:last-child{border-right:transparent}.wysiwyg_content table.small_table,.wysiwyg_content table.mb_table.small_table{font-size:.75em;padding:.6em}#main_container form.gsc-search-box{padding:0}#main_container form.gsc-search-box td.gsc-input{padding:0}#main_container table[class^="gsc-"] td,#main_container table[class^="gcsc-"] td{background-color:transparent}#main_container.placeholder{-webkit-font-smoothing:antialiased}#main_container:-moz-placeholder{-webkit-font-smoothing:antialiased}#main_container::-moz-placeholder{-webkit-font-smoothing:antialiased}#main_container::-webkit-input-placeholder{-webkit-font-smoothing:antialiased}#main_container:-ms-input-placeholder{-webkit-font-smoothing:antialiased}#main_container .gsc-control-cse table{margin:0}#main_container input.gsc-input{padding:10px 12px;border-radius:6px;font-size:15px}#main_container input.gsc-search-button{border-color:#fff;background-color:#3b788b;padding:10px 14px 10px;height:38px;color:white;font-size:15px;font-weight:500;border-radius:6px;text-transform:uppercase}#main_container input.gsc-search-button:hover{background-color:#5097ad}#main_container .gsc-selected-option-container{width:auto !important;max-width:none}#main_container td.gsc-clear-button{padding-left:4px}#main_container .cse .gsc-control-cse,#main_container .gsc-control-cse{padding:0}#main_container .cse .gsc-control-cse tr,#main_container .gsc-control-cse tr{background:none !important}#main_container td.gsc-result-info-container{padding-left:0}#main_container .gs-no-results-result .gs-snippet,#main_container .gs-error-result .gs-snippet{padding:5px 0;margin:5px 0;border:none;background-color:transparent}#main_container .gsc-webResult.gsc-results{margin-top:0px}#main_container div.gsc-webResult.gsc-result{border-bottom:1px solid #CFD7E1;padding-bottom:16px;padding-top:16px;padding-left:0;margin-bottom:0px;margin-top:0px}#main_container td.gsc-table-cell-snippet-close{padding:0}#main_container div.gs-title{padding:0;height:auto;line-height:1.4em;text-decoration:none}#main_container .gs-result a.gs-title,#main_container .gs-result a.gs-title b{color:#388FDA;text-decoration:none;font-weight:700;letter-spacing:-.035em;height:auto;padding:0}@media (min-width: 600px), print{#main_container .gs-result a.gs-title,#main_container .gs-result a.gs-title b{font-size:18px}}@media (min-width: 769px), print{#main_container .gs-result a.gs-title,#main_container .gs-result a.gs-title b{font-size:20px}}#main_container a.gs-title:hover{color:#115FA3;text-decoration:underline}#main_container a.gs-title:hover b{color:#115FA3}#main_container .gs-webResult .gs-snippet,#main_container .gs-imageResult .gs-snippet,#main_container .gs-fileFormatType{color:#333;line-height:1.4em}@media (min-width: 1024px), print{#main_container .gs-webResult .gs-snippet,#main_container .gs-imageResult .gs-snippet,#main_container .gs-fileFormatType{font-size:15px}}#main_container .gs-webResult div.gs-visibleUrl,#main_container .gs-imageResult div.gs-visibleUrl{color:#888}#main_container .gsc-table-cell-thumbnail{padding:0 6px 0 0}@media (min-width: 600px), print{#main_container .gsc-table-cell-thumbnail{padding:0 12px 0 0}}@media (min-width: 1024px), print{#main_container .gsc-table-cell-thumbnail{padding:0 16px 0 0}}#main_container .gs-web-image-box{width:100px}@media (min-width: 600px), print{#main_container .gs-web-image-box{padding:0;width:125px}}#main_container img.gs-image,#main_container .gs-promotion-image-box img.gs-promotion-image{border:none;width:100%;height:auto;max-width:none;max-height:none}#main_container a.gs-image{display:block}#main_container .gsc-results .gsc-cursor-box{padding-top:2px}#main_container .gsc-results .gsc-cursor-box .gsc-cursor-page{color:#388FDA;font-size:17px}#main_container .gsc-results .gsc-cursor-box .gsc-cursor-current-page{color:#333;background-color:transparent;text-shadow:none;padding:0}#main_container .gsc-adBlock{display:none !important}.aaa{border:0 solid red}.shareline{width:100%;margin:1.7em 0 2.7em;display:block;position:relative;clear:both}.shareline .shareline_heading{margin-bottom:.5em;position:relative;margin-top:0}.shareline.top_attached_sl{margin-bottom:0}.shareline.bottom_attached_sl{margin-top:-1px;margin-top:0}.shareline.bottom_attached_sl article{border-top:none}.shareline.bottom_attached_sl .shareline_heading{display:none}.shareline article{padding:17px 0em 18px;border-top:1px solid #BEBEBE;border-bottom:1px solid #BEBEBE;position:relative;overflow:visible}.shareline .share_container{display:inline-block;vertical-align:top;width:75px}.shareline .share_container .selector{display:inline-block}.shareline .share_container .selected{display:inline-block;position:relative;top:1px;height:25px;width:25px;cursor:pointer}.shareline .share_container .selected:before{font-size:32px;margin-top:-3px}.shareline .share_container .arrow_box{display:inline-block;position:relative;padding:9px 11px;cursor:pointer}.shareline .share_container .arrow_down{width:0;height:0;border-top:6px solid transparent !important;border-bottom:6px solid transparent !important;border-left:8px solid #b2b2b2;display:inline-block;transform:rotate(90deg)}.shareline .share_container .arrow_down:hover{border-color:black}.shareline .share_options{display:none;position:absolute;top:47px;left:0;z-index:2;background-color:#FFF;border:1px solid #BEBEBE;padding:5px 6px 0 6px}.shareline .share_options .share_btn{width:40px;height:40px;font-size:40px;display:inline;margin:0.1em;cursor:pointer}.shareline a.fi-social-twitter,.shareline a.fi-social-facebook{color:#2b2b2b;text-decoration:none}.shareline .share_text{display:inline-block;vertical-align:middle;width:calc(100% - 75px);color:#555;font-size:95%}#explore_overlay .shareline a.fi-social-twitter,#explore_overlay .shareline a.fi-social-facebook,#explore_overlay .shareline .share_text,.explore_overlay_page .shareline a.fi-social-twitter,.explore_overlay_page .shareline a.fi-social-facebook,.explore_overlay_page .shareline .share_text{color:#FFF}#explore_overlay .shareline .share_options a.fi-social-twitter,#explore_overlay .shareline .share_options a.fi-social-facebook,.explore_overlay_page .shareline .share_options a.fi-social-twitter,.explore_overlay_page .shareline .share_options a.fi-social-facebook{color:#2b2b2b}#explore_overlay .shareline .arrow_down:hover,.explore_overlay_page .shareline .arrow_down:hover{border-color:white}#explore_overlay .shareline article,.explore_overlay_page .shareline article{border-color:#6d6b6b}.keypoint .share_container{width:28px;margin-top:-1px;margin-left:1em}.keypoint .keypoint_icon{font-size:1.2em}section.missions_teaser{background:#edecec;z-index:10}section.missions_teaser header{margin-bottom:2em}section.missions_teaser ul.missions_circles{text-align:center;margin-bottom:2em}section.missions_teaser ul.missions_circles li.mission_item{display:block;margin-left:auto;margin-right:auto;width:300px;text-decoration:none;margin-bottom:3%;border-radius:150px;overflow:hidden}@media (max-width: 480px){section.missions_teaser ul.missions_circles li.mission_item{width:290px;border-radius:145px}}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles li.mission_item{width:210px;border-radius:105px;margin-right:2%;display:inline-block;margin-bottom:0}section.missions_teaser ul.missions_circles li.mission_item:last-child{margin-right:0}}@media (min-width: 1024px), print{section.missions_teaser ul.missions_circles li.mission_item{margin-right:3%;width:300px;border-radius:150px}}@media (min-width: 1200px){section.missions_teaser ul.missions_circles li.mission_item{margin-right:5%}}section.missions_teaser ul.missions_circles li.mission_item:first-child{border:5px solid #3b788b}section.missions_teaser ul.missions_circles li.mission_item:nth-child(2){border:5px solid #c25b28}section.missions_teaser ul.missions_circles li.mission_item:nth-child(3){border:5px solid #fda43c}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles li.mission_item .rollover_description{transition:opacity .4s}}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles li.mission_item .rollover_description .rollover_description_inner{font-size:0.9em}}@media (min-width: 1024px), print{section.missions_teaser ul.missions_circles li.mission_item .rollover_description .rollover_description_inner{font-size:1em}}section.missions_teaser ul.missions_circles li.mission_item .rollover_description *{color:white}section.missions_teaser ul.missions_circles li.mission_item:hover .rollover_description{display:none}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles li.mission_item:hover .rollover_description{display:block;opacity:1;height:100%;width:100%;z-index:1;top:0;right:0;overflow:hidden;position:absolute;background-color:rgba(0,0,0,0.6);border-radius:50%;padding:2em;color:white;font-weight:500;font-size:1.1em}}@media (min-width: 1024px), print{section.missions_teaser ul.missions_circles li.mission_item:hover .rollover_description{padding:6em 1.5em}}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles li.mission_item:hover li.mission_item .title{opacity:0}}section.missions_teaser ul.missions_circles a{height:300px;display:block;position:relative;text-decoration:none}@media (max-width: 480px){section.missions_teaser ul.missions_circles a{height:290px}}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles a{height:210px}}@media (min-width: 1024px), print{section.missions_teaser ul.missions_circles a{height:300px}}section.missions_teaser ul.missions_circles a .title{text-align:center;position:relative;display:block;top:80%;color:white;text-transform:uppercase;font-size:1.2em;font-weight:500;transition:opacity .4s}@media (min-width: 769px), print{section.missions_teaser ul.missions_circles a .title{top:72%}}@media (min-width: 1024px), print{section.missions_teaser ul.missions_circles a .title{top:80%}}section.missions_teaser footer .detail_link{display:block;margin:0.5em 1em 0 0;white-space:nowrap}@media (min-width: 769px), print{section.missions_teaser footer{float:right;text-align:right}}section.missions_teaser footer a{color:#4e8fa4}.wysiwyg_content .footnotes li{position:relative;font-size:.85em;margin-bottom:1em}.wysiwyg_content .footnotes li .footnote h2{margin:0;color:#222;font-size:1.2em}.wysiwyg_content .footnotes li p{margin:0.4em 0 0.6em}.wysiwyg_content .footnote{font-size:.8em}#secondary_column .footnote{font-size:.8em}.primary_media_feature{margin-bottom:0}@media (min-width: 769px), print{.primary_media_feature{padding:0}}.primary_media_feature.single{position:relative;margin-bottom:0;overflow:hidden}.primary_media_feature.single .feature_container{height:300px;background-size:cover;position:relative;z-index:3;background-position:center}@media (min-width: 769px), print{.primary_media_feature.single .feature_container{height:700px}}.primary_media_feature.single.video .play{display:none;position:absolute;top:47%;left:47%;top:calc(50%- 30px);left:calc(50%- 30px);top:-webkit-calc(50% - 30px);left:-webkit-calc(50% - 30px);width:60px;height:60px;padding-top:0;cursor:pointer;background:url("https://mars.nasa.gov/assets/play-button.png") 0 0 no-repeat;z-index:10}.primary_media_feature.single.video .player{width:100%;height:100%;position:absolute;top:0;left:0;z-index:2}.primary_media_feature.single .video_header_overlay{position:absolute;bottom:2em;margin:0 auto;left:0;right:0;width:auto;text-align:center;color:white;z-index:5}.primary_media_feature.single .video_header_overlay .media_feature_title{font-size:3em}.custom_banner_container{position:relative}.faq_section h2{margin-top:0}.faq_section ul.q_and_a{margin-bottom:1em}.faq_section ul.q_and_a .question{margin-bottom:1em}.faq_section ul.q_and_a .question:last-child{margin-bottom:0.6em}.faq_section ul.q_and_a .title_container{cursor:pointer}.faq_section ul.q_and_a .title{font-weight:700;font-size:1.1em}.faq_section ul.q_and_a .text.answer{visibility:hidden;position:absolute;left:-9999px}.faq_section ul.q_and_a .text.answer.open{visibility:visible;position:relative;left:0}.faq_section hr:last-child{display:none}.fullscreen_element{position:absolute;top:7px;right:7px;cursor:pointer;background-color:rgba(0,0,0,0.5);width:50px;height:50px;border-radius:5px;z-index:10}@media (min-width: 769px), print{.fullscreen_element{top:20px;right:20px}}.fullscreen_element .fullscreen-icon{height:25px;width:25px;background:url("https://mars.nasa.gov/assets/fullscreen_sprite@2x.png") 1px -25px;background-size:25px;margin:13px 0 0 13px}.fullscreen_element:hover .fullscreen-icon{background:url("https://mars.nasa.gov/assets/fullscreen_sprite@2x.png") 1px 0px;background-size:25px}.fullscreen_element.fullscreen-mode .fullscreen-icon{background:url("https://mars.nasa.gov/assets/fullscreen_sprite@2x.png") 1px -74px;background-size:25px}.fullscreen_element.fullscreen-mode:hover .fullscreen-icon{background:url("https://mars.nasa.gov/assets/fullscreen_sprite@2x.png") 1px -49px;background-size:25px}#timeline-embed:fullscreen{height:100%;width:100%;min-height:none;max-height:none}.triple_teaser{background-color:white;z-index:11}.triple_teaser .column{width:100%}@media (min-width: 769px), print{.triple_teaser .column{width:31.03448%;float:left}.triple_teaser .column:nth-child(3n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.triple_teaser .column:nth-child(3n+2){margin-left:34.48276%;margin-right:-100%;clear:none}.triple_teaser .column:nth-child(3n+3){margin-left:68.96552%;margin-right:-100%;clear:none}}@media (min-width: 1024px), print{.triple_teaser .column{width:28.57143%;float:left}.triple_teaser .column:nth-child(3n+1){margin-left:0;margin-right:-100%;clear:both;margin-left:0}.triple_teaser .column:nth-child(3n+2){margin-left:35.71429%;margin-right:-100%;clear:none}.triple_teaser .column:nth-child(3n+3){margin-left:71.42857%;margin-right:-100%;clear:none}}.triple_teaser .column:last-child{margin-bottom:1em}.triple_teaser .column+.column{margin-top:3em}@media (min-width: 769px), print{.triple_teaser .column+.column{margin-top:0}}.triple_teaser header{margin-bottom:1.3em}.triple_teaser .module_title,.triple_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .triple_teaser .carousel_title{text-align:left}@media (min-width: 600px), print{.triple_teaser .module_title,.triple_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .triple_teaser .carousel_title{font-size:2em}}@media (min-width: 600px), print{.triple_teaser footer{text-align:left}}.triple_teaser footer .detail_link{float:left;clear:both;text-align:left;white-space:nowrap}.triple_teaser .img_area{margin-bottom:1em;float:none;width:100%}.triple_teaser .item_list{margin-bottom:1em}.triple_teaser .item_list li{border-bottom:1px solid #BEBEBE;padding:3.44828% 0}.triple_teaser .item_list li:first-child{padding-top:0}.triple_teaser .item_list li:last-child{border-bottom:none}.triple_teaser .item_list .list_image{width:39.65517%;float:left;margin-right:3.44828%;margin-left:0}@media (min-width: 600px), print{.triple_teaser .item_list .list_image{width:31.03448%;float:left;margin-right:3.44828%}}.triple_teaser .item_list .list_text{width:56.89655%;float:right;margin-right:0}@media (min-width: 600px), print{.triple_teaser .item_list .list_text{width:65.51724%;float:right;margin-right:0}}.triple_teaser .item_list .list_text .date{color:#707070;font-size:0.8em;font-weight:500;margin-bottom:0.3em}.triple_teaser .item_list .list_text .date span:before{content:" \2022 "}.triple_teaser .item_list .list_text .title{font-size:1em;font-weight:500}.triple_teaser .upcoming_events .item_list li{padding:3.44828% 0}.triple_teaser .upcoming_events .item_list li:first-child{padding-top:0}.triple_teaser .upcoming_events .item_list .list_text .date{margin-bottom:.7em}.triple_teaser .follow_teaser .text_area{font-weight:300;font-size:1rem}.triple_teaser .follow_teaser .text_area footer{margin-top:2em}ul.item_list{margin-bottom:2em}ul.item_list .list_title{font-size:1.3em;font-weight:700;margin-bottom:.5em}ul.item_list .list_title a{color:#222}ul.item_list .text_only .list_text{width:100%;padding:0}ul.item_list&gt;li hr{margin:0}ul.item_list .list_image{width:37.5%;float:right;margin-right:0;margin-left:4.16667%;margin-bottom:.5em}@media (min-width: 600px), print{ul.item_list .list_image{margin-left:0;margin-bottom:0;width:35.89744%;float:left;margin-right:2.5641%}}@media (min-width: 769px), print{ul.item_list .list_image{width:22.41379%;float:left;margin-right:3.44828%}}@media (min-width: 1024px), print{ul.item_list .list_image{width:31.03448%;float:left;margin-right:3.44828%}}@media (min-width: 600px), print{ul.item_list .list_text{width:61.53846%;float:right;margin-right:0}}@media (min-width: 769px), print{ul.item_list .list_text{width:74.13793%;float:right;margin-right:0}}@media (min-width: 1024px), print{ul.item_list .list_text{width:65.51724%;float:right;margin-right:0}}ul.item_list .list_text h2,ul.item_list .list_text h3,ul.item_list .list_text h4{margin-top:0}ul.item_list .list_content{padding:1em 0}ul.item_list .list_description{margin-top:0}ul.item_list .description .long{display:none}ul.item_list .description .long p:first-of-type{margin-top:0}ul.people.item_list li.person{padding:4.16667% 0}ul.people.item_list li.person:first-child{padding-top:0}ul.people.item_list .person_header{margin-bottom:1.2em}ul.people.item_list .list_title.list_name{padding-top:7%}@media (min-width: 600px), print{ul.people.item_list .list_title.list_name{padding:0}}ul.people.item_list .person_title{font-weight:300}ul.people.item_list .description{clear:both}@media (min-width: 600px), print{ul.people.item_list .description{clear:none}}ul.people.item_list .person+.person{border-top:1px solid #BEBEBE}ul.item_list.text_item_list .list_text{width:100%}ul.item_list.text_item_list .list_text .date{margin-bottom:.3em}ul.item_list.text_item_list a{color:#257cdf}ul.item_list.text_item_list a:hover{text-decoration:underline}ul.item_list.text_item_list .publication_authors{margin-bottom:.4em}ul.item_list.text_item_list .citation{font-size:.85em;margin-bottom:.4em;font-weight:300}ul.item_list.text_item_list .publication_title{font-size:1.1em;font-weight:700;margin-bottom:.4em}ul.item_list.text_item_list .publication_title a{color:#222}ul.item_list.text_item_list .list_title a{color:#222}.explore_overlay_page .feature_pages .wysiwyg_content .item_list_module{margin-left:auto}@media (min-width: 1024px){.secondary_nav_desktop{overflow-x:auto}}@media (min-width: 1024px){.custom_banner_container .secondary_nav_desktop{overflow-x:visible}}@media (min-width: 1024px){.custom_banner_container .fixed_secondary_nav{overflow-x:auto}}nav.secondary_nav{font-weight:400}nav.secondary_nav .grid_layout{width:100%;padding-left:10px;padding-right:10px;max-width:none}@media (min-width: 600px), print{nav.secondary_nav .grid_layout{padding-left:17px;padding-right:17px}}nav.secondary_nav.secondary_nav_mobile{display:block;width:100%}nav.secondary_nav.secondary_nav_mobile select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin:0;background-color:#AFB3B9;width:100%;max-width:none;border-radius:0}nav.secondary_nav.secondary_nav_mobile select::-ms-expand{display:none}nav.secondary_nav.secondary_nav_mobile select option{padding:0.5em 1em}@media (min-width: 1024px){nav.secondary_nav.secondary_nav_mobile{display:none}}nav.secondary_nav.secondary_nav_desktop{display:none}@media (min-width: 1024px){nav.secondary_nav.secondary_nav_desktop{padding:0.8em 0 0.9em;display:block;margin:0;background-color:#eee;text-align:center}}nav.secondary_nav.secondary_nav_desktop .section_title{display:none}nav.secondary_nav.secondary_nav_desktop .section_title a{padding-left:0;text-decoration:none}nav.secondary_nav.secondary_nav_desktop li{display:inline-block;position:relative}nav.secondary_nav.secondary_nav_desktop a{color:#777;font-size:1em;font-weight:600;display:block;padding:.3em .3em}@media (min-width: 769px), print{nav.secondary_nav.secondary_nav_desktop a{padding:.3em .6em}}@media (min-width: 1200px){nav.secondary_nav.secondary_nav_desktop a{padding:.3em .9em}}@media (min-width: 1700px){nav.secondary_nav.secondary_nav_desktop a{font-size:1.1em}}.custom_banner_container nav.secondary_nav.secondary_nav_desktop a{color:white}nav.secondary_nav.secondary_nav_desktop ul{white-space:nowrap}nav.secondary_nav.secondary_nav_desktop li.current a,nav.secondary_nav.secondary_nav_desktop li:hover a{text-decoration:none;color:#222}nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav .grid_layout{display:flex;flex-wrap:nowrap;justify-content:space-between}@media (min-width: 1024px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav{position:fixed;width:100%;top:0;left:0;z-index:100;box-shadow:0 4px 4px -2px rgba(0,0,0,0.15)}nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav.secondary_nav_desktop{padding:1em 0 0.8em;white-space:nowrap}nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav .section_title{display:inline-block;margin-top:3px;margin-right:1.6em;font-size:1.2em;flex-shrink:0}nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav .section_title a{padding:0;color:#2B2B2B}}@media (min-width: 1024px) and (min-width: 1700px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav .section_title{margin-top:6px}}@media (min-width: 1024px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav ul{display:inline-block;text-align:right;width:100%}}@media (min-width: 1024px) and (min-width: 1024px), print and (min-width: 1024px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a{padding:.3em .5em;font-size:0.9em}}@media (min-width: 1024px) and (min-width: 1200px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a{padding:.3em .6em;font-size:0.95em}}@media (min-width: 1024px) and (min-width: 1700px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a{padding:.3em .8em;font-size:1em}}@media (min-width: 1024px){nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li.current a,nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav a:hover,nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav .section_title a:hover{text-decoration:none;color:#2B2B2B}nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li:last-of-type a{padding-right:0}}.custom_banner_container nav.secondary_nav.secondary_nav_desktop,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop{text-align:center;margin:0;background-color:transparent}.custom_banner_container nav.secondary_nav.secondary_nav_desktop li,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop li{margin-bottom:6px}.custom_banner_container nav.secondary_nav.secondary_nav_desktop li a,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop li a{color:white}.custom_banner_container nav.secondary_nav.secondary_nav_desktop li.current:after,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop li.current:after{bottom:-60px;left:50%;border:solid transparent;content:" ";height:0;width:0;position:absolute;pointer-events:none;border-top-color:black;border-width:20px;margin-left:-20px}.custom_banner_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav{background-color:#e4e7ec}.custom_banner_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li.current:after,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li.current:after{content:none}.custom_banner_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a{color:#fff}.custom_banner_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a:hover,.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li a:hover{color:#2B2B2B}.custom_banner_container nav.secondary_nav.secondary_nav_mobile,.homepage_feature_container nav.secondary_nav.secondary_nav_mobile{text-align:center;padding:0 2.5%}.custom_banner_container nav.secondary_nav.secondary_nav_mobile select,.homepage_feature_container nav.secondary_nav.secondary_nav_mobile select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin:0.9em 0 1.1em}.custom_banner_container nav.secondary_nav.secondary_nav_mobile select::-ms-expand,.homepage_feature_container nav.secondary_nav.secondary_nav_mobile select::-ms-expand{display:none}.custom_banner_container nav.secondary_nav.secondary_nav_mobile select option,.homepage_feature_container nav.secondary_nav.secondary_nav_mobile select option{padding:0.5em 1em}.homepage_feature_container nav.secondary_nav{position:absolute;bottom:0;z-index:2}.homepage_feature_container nav.secondary_nav.secondary_nav_desktop{width:100%}.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav{bottom:auto}.homepage_feature_container nav.secondary_nav.secondary_nav_desktop.fixed_secondary_nav li.current:after{content:none}#explore_overlay nav.secondary_nav{display:none}nav.tertiary_nav{position:relative;margin-top:1.2em;font-weight:400}nav.tertiary_nav ul li{position:relative;display:inline-block;vertical-align:middle}nav.tertiary_nav ul li:hover a,nav.tertiary_nav ul li.current a{color:black}nav.tertiary_nav ul li+li:before{content:" | ";padding:0 .5em;vertical-align:middle;font-weight:100;color:#BEBEBE}nav.tertiary_nav ul a{display:inline-block;vertical-align:middle;color:#909090}nav.tertiary_nav ul a:hover{text-decoration:none}@media (min-width: 769px), print{nav.tertiary_nav ul a{font-size:1.2em}}@media (min-width: 1024px), print{nav.tertiary_nav ul a{font-size:1.3em}}@media (min-width: 1200px){nav.tertiary_nav ul a{font-size:1.4em}}.tertiary_nav_mobile{display:block;text-align:center}@media (min-width: 600px), print{.tertiary_nav_mobile{text-align:left}}.tertiary_nav_mobile select{position:relative;padding:.5em 2em .5em 1em;font-size:16px;border:0;height:40px;vertical-align:middle;color:white;-webkit-appearance:none;-o-appearance:none;-moz-appearance:none;background:#3b788b url("https://mars.nasa.gov/assets/arrows_select_box@2x.png") no-repeat 95% 10px;background-position:right .8em top 10px;background-size:9px;font-weight:700;cursor:pointer;width:100%;border-radius:5px;max-width:304px;margin:0 auto}.tertiary_nav_mobile select::-ms-expand{display:none}.tertiary_nav_mobile select option{padding:0.5em 1em}@media (min-width: 769px), print{.tertiary_nav_mobile{display:none}}.tertiary_nav_desktop{display:none}@media (min-width: 769px), print{.tertiary_nav_desktop{display:block}}.condense_control{color:#257cdf}.condense_control:hover{text-decoration:underline}.condense_control:before{content:' › ';white-space:nowrap}section.intro{display:block;background-color:#000;position:absolute;top:0;left:0;z-index:99;width:100%;overflow:hidden;height:100vh}section.intro .brand_area{background:url("https://mars.nasa.gov/assets/logo_nasa_trio@2x.png") no-repeat;background-size:100%;z-index:301;position:absolute;top:2%;left:4%;width:60%;height:80%;max-width:500px}@media (min-width: 769px), print{section.intro .brand_area{width:45%;left:2%}}section.intro img{object-fit:cover;height:100%;width:100%}@media (min-width: 480px){section.intro img{margin-top:0}}body.intro_screen_visible .vital_signs_menu,body.intro_screen_visible .more_bar{z-index:100}body.intro_screen_visible .vital_signs_menu .overlay_icon{display:none}body.intro_screen_visible section.more_bar .title,body.intro_screen_visible section.more_bar .arrow_down{display:none}body.intro_screen_visible section.more_bar:after{content:"loading...";display:inline-block;padding:0.6em 0;font-size:.9em}html.explore_overlay_open,body.explore_overlay_open{overflow:hidden;width:100%;height:100%;position:fixed;-ms-overflow-style:-ms-autohiding-scrollbar}#explore_overlay{position:fixed;top:0;left:0;height:100vh;width:100%;z-index:1000001;overflow-x:hidden;visibility:hidden;opacity:0;padding:0 0 6px;background-color:rgba(0,0,0,0.9)}.overlay_loaded #explore_overlay{background-color:#000}#explore_overlay.visible{visibility:visible}#explore_overlay .content{position:relative;height:100%;visibility:hidden;opacity:0}#explore_overlay .content.visible{visibility:visible}#explore_overlay .content&gt;iframe{top:0;left:0;position:absolute}#explore_overlay .loading{position:absolute;left:50%;top:42vh;transform:translateX(-50%);width:auto;text-align:center;display:none}#explore_overlay .loading img{width:44px;height:44px}#explore_overlay .loading p{font-family:Whitney, Helvetica, Arial, sans-serif;position:relative;color:#76aee6;font-size:14px;letter-spacing:0.1em}#explore_overlay .loading .spinner div{background:#ccc !important}#explore_overlay .background_area{-webkit-tap-highlight-color:transparent;-webkit-tap-highlight-color:transparent;width:100%;height:100%;position:absolute;top:0;left:0;cursor:pointer;z-index:-1}#explore_overlay.lightbox_overlay{background-color:rgba(0,0,0,0.75);text-align:center;padding:0}#explore_overlay.lightbox_overlay .content{position:fixed;background-color:#1f1f1f;width:95%;height:92% !important;max-width:1400px;margin:2em auto 0;border-radius:4px;left:auto;right:calc(50vw - 47.5%)}@media only screen and (min-width: 1480px){#explore_overlay.lightbox_overlay .content{right:calc(50vw - 697px)}}@media only screen and (max-device-width: 1024px) and (-webkit-min-device-pixel-ratio: 1){#explore_overlay.lightbox_overlay .content{overflow-y:scroll;-webkit-overflow-scrolling:touch}}.overlay_close_button{position:absolute;top:1em;right:1.1em;z-index:1000003;width:40px;height:40px;position:fixed;background:#000;text-decoration:none;text-align:center;line-height:1em;transition:.3s opacity;visibility:hidden;opacity:0}.overlay_close_button.visible{visibility:visible}.no-touchevents .overlay_close_button:hover{opacity:1}.overlay_close_button .close_icon{display:block;height:100%;position:relative}.overlay_close_button .close_icon:before{transform:rotate(-45deg);content:'';position:absolute;height:1px;width:100%;top:calc(50% - .5px);left:0;background:#fff;opacity:.8}.overlay_close_button .close_icon:after{transform:rotate(45deg);content:'';position:absolute;height:1px;width:100%;top:calc(50% - .5px);left:0;background:#fff;opacity:.8}@media (min-width: 769px), print{.overlay_close_button{width:60px;height:60px;top:1.1em;right:1.1em}}@media (min-width: 1700px){.overlay_close_button{width:70px;height:70px;top:1.2em;right:1.2em}}.overlay_close_button.lightbox_overlay{background-color:#1f1f1f;top:2.5em;right:calc(50vw - 45.5%)}@media (min-width: 600px), print{.overlay_close_button.lightbox_overlay{right:calc(50vw - 45%)}}@media (min-width: 769px), print{.overlay_close_button.lightbox_overlay{top:2.6em;width:50px;height:50px}}@media (min-width: 1024px), print{.overlay_close_button.lightbox_overlay{right:calc(50vw - 45.5%)}}@media only screen and (min-width: 1480px){.overlay_close_button.lightbox_overlay{right:calc(50vw - 680px)}}@media (min-width: 1700px){.overlay_close_button.lightbox_overlay{top:2.7em;width:60px;height:60px}}#iframe_overlay,#iframe_overlay body{height:100%;overflow-y:auto;-webkit-overflow-scrolling:touch;font-weight:400}#iframe_overlay{width:1px;min-width:100%;word-wrap:break-word;color:#e4e3e3}#iframe_overlay p,#iframe_overlay .release_date{color:#e4e3e3}#iframe_overlay hr{border-color:#3c3c3c}#iframe_overlay a{color:#42a0f2}#iframe_overlay .header_mask{display:none}#iframe_overlay .explore_overlay_page{padding-bottom:4em}#iframe_overlay .done_btn{text-align:center}#iframe_overlay .done_btn button{color:#6bbed8;margin:2em 0;padding:.3em .7em .4em;background:none;cursor:pointer;letter-spacing:1px;font-weight:300;outline:none;position:relative;font-size:1.8em;border:1px solid #6bbed8;transition:color 200ms, border-color 200ms}#iframe_overlay .done_btn button::after{content:'Close'}#iframe_overlay .done_btn button:hover{color:#82ddf9;border-color:#82ddf9}#iframe_overlay .left_col,#iframe_overlay .right_col{position:relative;float:left}#iframe_overlay .left_col{width:100%}@media (min-width: 769px), print{#iframe_overlay .left_col{width:65%;border-right:1px solid #BEBEBE;padding-right:1em}}@media (min-width: 1200px){#iframe_overlay .left_col{padding-right:3em}}#iframe_overlay .right_col{width:100%}#iframe_overlay .right_col p{color:#868686}#iframe_overlay .right_col p b{color:#222}@media (min-width: 769px), print{#iframe_overlay .right_col{width:35%;padding-left:1em;left:-1px}}@media (min-width: 1200px){#iframe_overlay .right_col{padding-left:3em}}#iframe_overlay .suggested_features{display:none}#iframe_overlay #secondary_column aside.boxed{border-color:#6d6b6b}#iframe_overlay #secondary_column .related_content_module{border-color:#5a5a5a}#iframe_overlay #secondary_column .related_content_module li{border-color:#3c3c3c;padding:.8em 0}#iframe_overlay .article_nav{display:none}.info_tabs_module{position:relative;color:black;background:url("https://mars.nasa.gov/assets/mars_landscape.jpg") center top no-repeat;background-size:cover}@media (min-width: 769px), print{.info_tabs_module{height:620px;background-position:center bottom}}.info_tabs_module div[data-react-class="InfoTabs"]{height:100%}.info_tabs_module .grid_layout{padding-bottom:10em}@media (min-width: 769px), print{.info_tabs_module .grid_layout{padding-bottom:0}}.info_tabs_module .gradient_container_bottom{display:none}.info_tabs_module .info_tabs{padding:2.7em 0 5em;height:100%}@media (min-width: 769px), print{.info_tabs_module .info_tabs{padding:5.3em 0 5em}}.info_tabs_module .col1,.info_tabs_module .col2{width:100%}@media (min-width: 769px), print{.info_tabs_module .col1,.info_tabs_module .col2{width:48%}}.info_tabs_module .col2{display:none;float:right;margin-top:2rem}@media (min-width: 769px), print{.info_tabs_module .col2{display:block;margin-top:0}}.info_tabs_module .col1{float:left}.info_tabs_module .info_tabs_header{width:100%;margin-bottom:1.8em;display:inline-block;text-align:center}@media (min-width: 769px), print{.info_tabs_module .info_tabs_header{text-align:left;margin-bottom:3em}}.info_tabs_module .info_tabs_header h2{font-size:1.69em;margin-bottom:0em;font-weight:300}@media (min-width: 600px), print{.info_tabs_module .info_tabs_header h2{font-size:1.95em;margin-bottom:0em}}@media (min-width: 769px), print{.info_tabs_module .info_tabs_header h2{font-size:2.21em;margin-bottom:0em}}@media (min-width: 1024px), print{.info_tabs_module .info_tabs_header h2{font-size:2.34em;margin-bottom:0em}}@media (min-width: 1200px){.info_tabs_module .info_tabs_header h2{font-size:2.47em;margin-bottom:0em}}.info_tabs_module .info_tabs_links{padding-left:2rem;position:relative;z-index:2;font-size:1.3rem;font-weight:300;width:90%}@media (min-width: 769px), print{.info_tabs_module .info_tabs_links{width:auto}}.info_tabs_module .info_tabs_links li{cursor:pointer;position:relative;margin-bottom:0.3em}.info_tabs_module .info_tabs_links .tab_title{margin-bottom:0.2em;letter-spacing:-0.02em}.info_tabs_module .info_tabs_links .info_tabs_link:before{content:'';width:0;height:0;border-top:7px solid transparent !important;border-bottom:7px solid transparent !important;border-left:11px solid #943b2b;display:inline-block;transform:none;position:absolute;left:-20px;top:7px;opacity:0;transition:all 200ms}.no-touchevents .info_tabs_module .info_tabs_links .info_tabs_link:not(.active):hover:before,.info_tabs_module .info_tabs_links .active:before{opacity:1;left:-28px}.no-touchevents .info_tabs_module .info_tabs_links .info_tabs_link:not(.active):hover:before{opacity:.7}.info_tabs_module .info_tabs_detail,.info_tabs_module .active .mobile_tab_detail{float:right;max-height:320px;overflow-y:auto;padding-right:1em;position:relative;z-index:2;font-weight:300;width:100%;-webkit-overflow-scrolling:touch}.info_tabs_module .info_tabs_detail::-webkit-scrollbar,.info_tabs_module .active .mobile_tab_detail::-webkit-scrollbar{width:5px}.info_tabs_module .info_tabs_detail::-webkit-scrollbar-thumb,.info_tabs_module .active .mobile_tab_detail::-webkit-scrollbar-thumb{background-color:rgba(107,107,107,0.6)}.info_tabs_module .info_tabs_detail::-webkit-scrollbar-track,.info_tabs_module .active .mobile_tab_detail::-webkit-scrollbar-track{background-color:rgba(157,157,157,0.4)}@media (min-width: 769px), print{.info_tabs_module .info_tabs_detail,.info_tabs_module .active .mobile_tab_detail{padding-right:3em;top:0.8em}}.info_tabs_module .info_tabs_detail .info_tabs_title,.info_tabs_module .active .mobile_tab_detail .info_tabs_title{display:none}@media (min-width: 769px), print{.info_tabs_module .info_tabs_detail .info_tabs_title,.info_tabs_module .active .mobile_tab_detail .info_tabs_title{display:block;font-size:1.1em;margin-bottom:1em;margin-top:0;text-transform:uppercase}}.info_tabs_module .mobile_tab_detail{display:none}.info_tabs_module .active .mobile_tab_detail{font-size:0.95rem;float:none;display:block}@media (min-width: 769px), print{.info_tabs_module .active .mobile_tab_detail{display:none}}.info_tabs_module .active .tab_title{margin-bottom:.5em}@media (min-width: 769px), print{.info_tabs_module .active .tab_title{margin-bottom:.2em}}.info_tabs_module .info_tabs_content *:first-child,.info_tabs_module .active .mobile_tab_detail *:first-child{margin-top:0}.info_tabs_module .info_tabs_content&gt;h2,.info_tabs_module .info_tabs_content&gt;h3,.info_tabs_module .info_tabs_content&gt;h4,.info_tabs_module .info_tabs_content&gt;p,.info_tabs_module .active .mobile_tab_detail&gt;h2,.info_tabs_module .active .mobile_tab_detail&gt;h3,.info_tabs_module .active .mobile_tab_detail&gt;h4,.info_tabs_module .active .mobile_tab_detail&gt;p{margin:1em 0}.info_tabs_module .info_tabs_content&gt;h2,.info_tabs_module .info_tabs_content&gt;h3,.info_tabs_module .info_tabs_content&gt;h4,.info_tabs_module .active .mobile_tab_detail&gt;h2,.info_tabs_module .active .mobile_tab_detail&gt;h3,.info_tabs_module .active .mobile_tab_detail&gt;h4{font-size:1.1em;margin-top:2em}@media (min-width: 769px), print{.info_tabs_module .info_tabs_content&gt;h2,.info_tabs_module .info_tabs_content&gt;h3,.info_tabs_module .info_tabs_content&gt;h4,.info_tabs_module .active .mobile_tab_detail&gt;h2,.info_tabs_module .active .mobile_tab_detail&gt;h3,.info_tabs_module .active .mobile_tab_detail&gt;h4{margin-top:0}}.info_tabs_module .info_tabs_content&gt;h3,.info_tabs_module .info_tabs_content&gt;h4,.info_tabs_module .active .mobile_tab_detail&gt;h3,.info_tabs_module .active .mobile_tab_detail&gt;h4{margin-top:1.5em}.info_tabs_module .info_tabs_content&gt;p:last-child{margin-bottom:1em}.info_tabs_module .info_tabs_content ol{margin-bottom:0}.info_tabs_module .active .mobile_tab_detail&gt;p:last-child{margin-bottom:0}.info_tabs_module .less_option,.info_tabs_module .more_option{display:inline-block;font-size:.95rem;margin-bottom:0.5em}@media (min-width: 769px), print{.info_tabs_module .less_option,.info_tabs_module .more_option{display:none}}.info_tabs_module .less_option:after{content:"- less";display:block}.info_tabs_module .more_option:after{content:"+ more";display:block}.info_tabs_module footer{width:90%;max-width:1330px;position:absolute;bottom:50px;right:0;left:0;margin:auto;text-align:right}.info_tabs_module .more_link{font-size:1rem;font-weight:500;text-transform:uppercase;color:white;cursor:pointer}@media (min-width: 769px){.parallax_categorized_teaser .bubble_container{max-height:788px}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px){.parallax_categorized_teaser .bubble_container .oculus{transform:scale(0.8)}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(1){top:-30px;left:-20px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(2){top:-30px;left:224px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(3){top:214px;left:-20px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(4){top:214px;left:224px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .bubble_container .oculus{transform:scale(0.9)}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(1){top:0;left:119px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(2){top:70px;left:408px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(3){left:0;top:269px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(4){top:347px;left:290px}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .bubble_container .oculus{position:absolute;transform:scale(1)}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(1){top:0;left:179px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(2){top:104px;left:495px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(3){top:282px;left:0}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(4){top:390px;left:324px}}@media (min-width: 769px) and (min-width: 1700px){.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(1){top:0;left:229px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(2){top:134px;left:565px}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(3){top:272px;left:0}.parallax_categorized_teaser .bubble_container .oculus:nth-of-type(4){top:420px;left:354px}}@media (min-width: 769px){.parallax_categorized_teaser .bubble_container{height:75vh}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.two .oculus{transform:scale(0.8)}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(1){top:-30px;left:-20px}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(2){top:-30px;left:224px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.two .oculus{transform:scale(0.9)}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(1){top:30px;left:70px}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(2){top:30px;left:378px}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .definition_teasers.two .oculus{position:absolute;transform:scale(1)}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(1){top:40px;left:70px}.parallax_categorized_teaser .definition_teasers.two .oculus:nth-of-type(2){top:40px;left:422px}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.three .oculus{transform:scale(0.8)}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(1){top:-30px;left:100px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(2){top:185px;left:-20px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(3){top:185px;left:224px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.three .oculus{transform:scale(0.9)}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(1){top:0;left:204px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(2){top:280px;left:30px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(3){top:280px;left:378px}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .definition_teasers.three .oculus{position:absolute;transform:scale(1)}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(1){top:20px;left:226px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(2){top:310px;left:30px}.parallax_categorized_teaser .definition_teasers.three .oculus:nth-of-type(3){top:310px;left:432px}}@media (min-width: 769px){.parallax_categorized_teaser .definition_teasers.four{max-height:788px}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.four .oculus{transform:scale(0.8)}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(1){top:-30px;left:-20px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(2){top:-30px;left:224px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(3){top:214px;left:-20px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(4){top:214px;left:224px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.four .oculus{transform:scale(0.9)}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(1){top:0;left:119px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(2){top:70px;left:408px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(3){left:0;top:269px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(4){top:347px;left:290px}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .definition_teasers.four .oculus{position:absolute;transform:scale(1)}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(1){top:0;left:179px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(2){top:104px;left:495px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(3){top:282px;left:0}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(4){top:390px;left:324px}}@media (min-width: 769px) and (min-width: 1700px){.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(1){top:0;left:229px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(2){top:134px;left:565px}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(3){top:272px;left:0}.parallax_categorized_teaser .definition_teasers.four .oculus:nth-of-type(4){top:420px;left:354px}}@media (min-width: 769px) and (min-width: 769px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.five .oculus{transform:scale(0.8)}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(1){top:-30px;left:-20px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(2){top:-30px;left:224px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(3){top:214px;left:-20px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(4){top:214px;left:224px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(5){top:432px;left:102px}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .definition_teasers.five .oculus{transform:scale(0.9)}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(1){top:0;left:0}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(2){top:0;left:408px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(3){top:200px;left:204px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(4){top:400px;left:0}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(5){top:400px;left:408px}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .definition_teasers.five .oculus{position:absolute;transform:scale(1)}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(1){top:0;left:0}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(2){top:0;left:452px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(3){top:200px;left:226px}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(4){top:400px;left:0}.parallax_categorized_teaser .definition_teasers.five .oculus:nth-of-type(5){top:400px;left:452px}}.parallax_categorized_teaser{height:auto;background:url("https://mars.nasa.gov/assets/red_planet_bg.png") center no-repeat;background-size:cover;color:#eeaaa1;overflow:hidden;position:relative;padding-top:3em}@media (min-width: 769px){.parallax_categorized_teaser{height:calc(100vh - 74px);padding-top:4em;min-height:860px}}@media (min-width: 1200px){.parallax_categorized_teaser{padding-top:5em}}@media (min-width: 769px){.parallax_categorized_teaser .module_content{height:calc(100% - 36px)}}.parallax_categorized_teaser .module_content.fixed{position:fixed;top:82px;left:0}.parallax_categorized_teaser .module_title,.parallax_categorized_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .parallax_categorized_teaser .carousel_title{font-size:2em;font-weight:200;text-align:center;margin-bottom:0.85em}@media (min-width: 769px){.parallax_categorized_teaser .module_title,.parallax_categorized_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .parallax_categorized_teaser .carousel_title{text-align:left;margin-bottom:1em}}@media (min-width: 1200px){.parallax_categorized_teaser .module_title,.parallax_categorized_teaser .main_carousel.module .carousel_header .carousel_title,.main_carousel.module .carousel_header .parallax_categorized_teaser .carousel_title{font-size:2.2em}}.parallax_categorized_teaser .mobile_only{display:block}@media (min-width: 769px){.parallax_categorized_teaser .mobile_only{display:none}}.parallax_categorized_teaser .categorized_content{width:100%;top:0;left:0;right:0;bottom:0;margin-bottom:0}@media (min-width: 769px){.parallax_categorized_teaser .categorized_content{width:55%;max-width:795px;position:absolute;top:80px;left:38%}}@media (min-width: 769px) and (min-width: 600px), print and (min-width: 769px){.parallax_categorized_teaser .categorized_content{width:58%}}@media (min-width: 769px) and (min-width: 1024px), print and (min-width: 769px){.parallax_categorized_teaser .categorized_content{left:33.5%;width:61.2%}}@media (min-width: 769px) and (min-width: 1200px){.parallax_categorized_teaser .categorized_content{left:34.5%;top:100px}}@media (min-width: 769px) and (min-width: 1700px){.parallax_categorized_teaser .categorized_content{position:relative;left:0;top:20px}}.parallax_categorized_teaser .categorized_content .content_for{position:relative;display:none}@media (min-width: 769px){.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents{padding:0 40px 0 0;max-height:71vh;overflow:hidden;overflow-y:auto}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents::-webkit-scrollbar{width:5px}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents::-webkit-scrollbar-thumb{background-color:rgba(255,255,255,0.4)}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents::-webkit-scrollbar-track{background-color:rgba(255,255,255,0.1)}}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h2,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h3,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h4,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;p{margin:1em 1.5em}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h2,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h3,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h4{color:#eeaaa1;font-size:1.1em;font-weight:400;margin-top:2em}@media (min-width: 769px){.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h2,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h3,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h4{margin-top:0}}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h2{text-transform:uppercase}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h3,.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;h4{margin-top:1.5em}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;p{color:#e3e3e3;max-width:640px}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;p:last-child{margin-bottom:2em}@media (min-width: 769px){.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;p:last-child{margin-bottom:1em}}.parallax_categorized_teaser .categorized_content .content_for .bubble_container_contents&gt;p a{color:#54b3da}.parallax_categorized_teaser .categorized_content .content_for.current{display:block}@media (min-width: 769px){.parallax_categorized_teaser .categorized_content .content_for{position:absolute;display:block;opacity:0;left:1000px;top:0;transition:all 400ms;width:100%}.parallax_categorized_teaser .categorized_content .content_for.current{left:0;top:0;opacity:1;display:block !important}}.parallax_categorized_teaser .carousels{height:100%;width:100%}.parallax_categorized_teaser footer{position:absolute;height:50px;bottom:0;left:0;width:100vw;background-color:rgba(129,33,24,0.6)}.parallax_categorized_teaser .oculus{position:relative;text-align:center;background-size:cover;height:186px;padding:1.5em 0;border-bottom:3px solid #5fb4ce;margin-bottom:0;overflow:hidden;background-color:rgba(32,52,66,0.39);color:#c7d9de}@media (min-width: 769px){.parallax_categorized_teaser .oculus{padding:0;position:absolute;width:275px;height:275px}}.parallax_categorized_teaser .oculus h3.carousel_title{position:absolute;font-size:.9rem;text-align:center;margin:0 auto 12px;width:auto;font-weight:300}@media (max-width: 768px){.parallax_categorized_teaser .oculus h3.carousel_title{left:50%;transform:translateX(-50%)}}@media (min-width: 769px){.parallax_categorized_teaser .oculus h3.carousel_title{position:relative;width:50%;margin:24px auto 12px}}.parallax_categorized_teaser .oculus h3.description_title{font-size:.9rem;font-weight:400;margin:0 0 12px}@media (min-width: 769px){.parallax_categorized_teaser .oculus h3.description_title{margin:17px 0 12px}}.parallax_categorized_teaser .oculus span.mobile_only_title{font-weight:200}@media (min-width: 769px){.parallax_categorized_teaser .oculus span.mobile_only_title{display:none}}.parallax_categorized_teaser .oculus .slide_description{padding:0 2em;font-weight:400}.parallax_categorized_teaser .oculus .cols{display:flex;margin-left:-6px;margin-right:-6px}.parallax_categorized_teaser .oculus .cols .col{margin:0 auto;max-width:55%}.parallax_categorized_teaser .oculus .cols .val{font-size:2rem;font-weight:400;letter-spacing:-.05em}.parallax_categorized_teaser .oculus .cols .val.small_text{font-size:1.6rem}.parallax_categorized_teaser .oculus .cols .val_label{font-size:.9rem;font-weight:300}@media (min-width: 769px){.parallax_categorized_teaser .oculus{border:3px solid #5fb4ce;border-radius:50%}}.parallax_categorized_teaser .oculus .carousel_title{color:#445c64}.parallax_categorized_teaser .oculus .title,.parallax_categorized_teaser .oculus .hover{color:#c7d9de}.parallax_categorized_teaser .oculus .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .oculus .slick-prev:before,.parallax_categorized_teaser .oculus .slick-next:before{background-image:none}.parallax_categorized_teaser .oculus .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:none}.parallax_categorized_teaser .oculus.data{border-bottom:3px solid #5fb4ce;margin-bottom:0;overflow:hidden;background-color:rgba(32,52,66,0.39);color:#c7d9de}@media (min-width: 769px){.parallax_categorized_teaser .oculus.data{border:3px solid #5fb4ce;border-radius:50%}}.parallax_categorized_teaser .oculus.data .carousel_title{color:#445c64}.parallax_categorized_teaser .oculus.data .title,.parallax_categorized_teaser .oculus.data .hover{color:#c7d9de}.parallax_categorized_teaser .oculus.data .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .oculus.data .slick-prev:before,.parallax_categorized_teaser .oculus.data .slick-next:before{background-image:none}.parallax_categorized_teaser .oculus.data .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:none}.parallax_categorized_teaser .oculus.data h3.carousel_title{display:none}@media (min-width: 769px){.parallax_categorized_teaser .oculus.data h3.carousel_title{display:block}}.parallax_categorized_teaser .oculus.data .carousel_title{color:#6b93a0}.parallax_categorized_teaser .oculus.data .val{color:white}.parallax_categorized_teaser .oculus.data .note{margin-top:15px;font-size:.75rem;font-weight:300}@media (min-width: 769px){.parallax_categorized_teaser .oculus.data .slick-slide.slide{background-color:rgba(95,180,206,0.1)}}.parallax_categorized_teaser .oculus.images{border-bottom:3px solid #f56b60;margin-bottom:0;overflow:hidden;background-color:rgba(0,0,0,0.39);color:#f06c60;padding:0}@media (min-width: 769px){.parallax_categorized_teaser .oculus.images{border:3px solid #f56b60;border-radius:50%}}.parallax_categorized_teaser .oculus.images .carousel_title{color:#f06c60}.parallax_categorized_teaser .oculus.images .title,.parallax_categorized_teaser .oculus.images .hover{color:#ffddcf}.parallax_categorized_teaser .oculus.images .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .oculus.images .slick-prev:before,.parallax_categorized_teaser .oculus.images .slick-next:before{background-image:none}.parallax_categorized_teaser .oculus.images .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:none}.parallax_categorized_teaser .oculus.images h3.carousel_title{z-index:1;background-color:rgba(86,42,42,0.65);text-align:center;padding:5px 14px;border-radius:6px;margin-top:1.2em}@media (max-width: 768px){.parallax_categorized_teaser .oculus.images h3.carousel_title{color:#ffbdb7}}@media (min-width: 769px){.parallax_categorized_teaser .oculus.images h3.carousel_title{background-color:transparent;padding:0;margin-top:24px}}.parallax_categorized_teaser .oculus.images a.hover_state{display:block;height:100%;width:100%;position:absolute;top:0}.parallax_categorized_teaser .oculus.images .rollover_description{background-color:rgba(86,42,42,0.85);padding:1em 2em;text-align:left}.parallax_categorized_teaser .oculus.images .rollover_description .title{font-size:.9rem;color:#f5dddb}.parallax_categorized_teaser .oculus.compare{border-bottom:3px solid #fcb963;margin-bottom:0;overflow:hidden;background-color:rgba(0,0,0,0.39);color:#f7ca99;background:linear-gradient(to right, rgba(53,30,1,0.46) 0%, rgba(53,30,1,0.46) 49%, rgba(10,8,9,0.47) 50%, rgba(10,8,9,0.47) 100%)}@media (min-width: 769px){.parallax_categorized_teaser .oculus.compare{border:3px solid #fcb963;border-radius:50%}}.parallax_categorized_teaser .oculus.compare .carousel_title{color:#f7ca99}.parallax_categorized_teaser .oculus.compare .title,.parallax_categorized_teaser .oculus.compare .hover{color:#f7ca99}.parallax_categorized_teaser .oculus.compare .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f5b460;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .oculus.compare .slick-prev:before,.parallax_categorized_teaser .oculus.compare .slick-next:before{background-image:none}.parallax_categorized_teaser .oculus.compare .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f5b460;display:inline-block;transform:none}.parallax_categorized_teaser .oculus.compare h3.carousel_title{display:none}@media (min-width: 769px){.parallax_categorized_teaser .oculus.compare h3.carousel_title{display:block}}.parallax_categorized_teaser .oculus.compare .thing_one .val{color:white}.parallax_categorized_teaser .oculus.compare .thing_two{color:#66c8eb}.parallax_categorized_teaser .oculus.compare .thing{font-weight:300}.parallax_categorized_teaser .oculus.compare .details{font-size:.9rem;font-weight:300}.parallax_categorized_teaser .oculus.news{border-bottom:3px solid #f56b60;margin-bottom:0;overflow:hidden;background-color:rgba(0,0,0,0.39);color:#f06c60}@media (min-width: 769px){.parallax_categorized_teaser .oculus.news{border:3px solid #f56b60;border-radius:50%}}.parallax_categorized_teaser .oculus.news .carousel_title{color:#f06c60}.parallax_categorized_teaser .oculus.news .title,.parallax_categorized_teaser .oculus.news .hover{color:#ffddcf}.parallax_categorized_teaser .oculus.news .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .oculus.news .slick-prev:before,.parallax_categorized_teaser .oculus.news .slick-next:before{background-image:none}.parallax_categorized_teaser .oculus.news .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:none}.parallax_categorized_teaser .oculus.news .slide_description{text-align:left}@media (max-width: 768px){.parallax_categorized_teaser .oculus.news .slide_description{margin-top:32px;padding:0 18%}}.parallax_categorized_teaser .oculus.news .slide_description a{color:#f5dddb}.parallax_categorized_teaser .oculus.news .slide_description .description{font-size:.9rem;font-weight:400}@media (min-width: 769px){.parallax_categorized_teaser .oculus.news .slick-slide.slide{background-color:rgba(86,42,42,0.58)}}.parallax_categorized_teaser .more_bar{background-color:rgba(129,33,24,0.4);transition:background-color 200ms}.parallax_categorized_teaser .more_bar:hover{background-color:rgba(129,33,24,0.7)}.parallax_categorized_teaser .more_bar .arrow_down{padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -50px -100px;background-size:300px}.parallax_categorized_teaser .more_bar .arrow_down:hover,.parallax_categorized_teaser .more_bar .arrow_down.active,.parallax_categorized_teaser .more_bar .arrow_down.current{background-position:-50px -100px}.parallax_categorized_teaser .slick-prev,.parallax_categorized_teaser .slick-next{top:calc(50% - 20px)}@media (min-width: 769px){.parallax_categorized_teaser .slick-prev,.parallax_categorized_teaser .slick-next{top:83%}}.parallax_categorized_teaser .slick-slider .slick-prev{left:2%}@media (min-width: 769px){.parallax_categorized_teaser .slick-slider .slick-prev{left:calc(50% - 35px)}}.parallax_categorized_teaser .slick-slider .slick-next{right:2%}@media (min-width: 769px){.parallax_categorized_teaser .slick-slider .slick-next{right:calc(50% - 35px)}}.parallax_categorized_teaser .slick-slide.slide{overflow:hidden;height:186px}@media (min-width: 769px){.parallax_categorized_teaser .slick-slide.slide{height:160px}}.parallax_categorized_teaser .definition_teasers .oculus{cursor:pointer}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme{border-bottom:3px solid #fcb963;margin-bottom:0;overflow:hidden;background-color:rgba(0,0,0,0.39);color:#f7ca99}@media (min-width: 769px){.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme{border:3px solid #fcb963;border-radius:50%}}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .carousel_title{color:#f7ca99}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .title,.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .hover{color:#f7ca99}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f5b460;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .slick-prev:before,.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .slick-next:before{background-image:none}.parallax_categorized_teaser .definition_teasers .oculus.warm_color_theme .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f5b460;display:inline-block;transform:none}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme{border-bottom:3px solid #5fb4ce;margin-bottom:0;overflow:hidden;background-color:rgba(32,52,66,0.39);color:#c7d9de}@media (min-width: 769px){.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme{border:3px solid #5fb4ce;border-radius:50%}}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .carousel_title{color:#445c64}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .title,.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .hover{color:#c7d9de}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .slick-prev:before,.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .slick-next:before{background-image:none}.parallax_categorized_teaser .definition_teasers .oculus.cool_color_theme .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #5998ac;display:inline-block;transform:none}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme{border-bottom:3px solid #f56b60;margin-bottom:0;overflow:hidden;background-color:rgba(0,0,0,0.39);color:#f06c60}@media (min-width: 769px){.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme{border:3px solid #f56b60;border-radius:50%}}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .carousel_title{color:#f06c60}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .title,.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .hover{color:#ffddcf}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .slick-prev:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:rotate(180deg)}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .slick-prev:before,.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .slick-next:before{background-image:none}.parallax_categorized_teaser .definition_teasers .oculus.sunset_color_theme .slick-next:before{width:0;height:0;border-top:9px solid transparent !important;border-bottom:9px solid transparent !important;border-left:9px solid #f06c60;display:inline-block;transform:none}.parallax_categorized_teaser .definition_teasers .title{margin-bottom:12px;font-size:1.6em}.no-touchevents .parallax_categorized_teaser .definition_teasers .title{margin-bottom:0}.parallax_categorized_teaser .definition_teasers .hover{font-size:1.2em}.parallax_categorized_teaser .definition_teasers .title,.parallax_categorized_teaser .definition_teasers .hover{margin-top:0;margin-right:10%;margin-left:10%;top:30%;position:relative;font-weight:300}.parallax_categorized_teaser .definition_teasers .title_container{position:relative;top:50%;transform:translateY(-50%)}.no-touchevents .parallax_categorized_teaser .definition_teasers .hover{display:none}.parallax_categorized_teaser .definition_teasers .bg_container{position:absolute;top:0;left:0;width:100%;height:100%;opacity:0.25;background-size:cover;transition:filter .4s;filter:brightness(60%)}@media (min-width: 769px){.parallax_categorized_teaser .definition_teasers .bg_container{filter:brightness(100%)}}.no-touchevents .parallax_categorized_teaser .definition_teasers .oculus:hover .hover{display:block}.no-touchevents .parallax_categorized_teaser .definition_teasers .oculus:hover .title{display:none}.no-touchevents .parallax_categorized_teaser .definition_teasers .oculus:hover .bg_container{filter:brightness(60%)}.parallax_categorized_teaser .definition_teasers .mobile_detailed_definition{display:none;position:relative}.parallax_categorized_teaser .detailed_def_visible .detailed_definition{padding:1.4em .8em;width:98%;position:relative;display:none}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .detailed_definition{padding:0;max-width:680px;width:90%;left:3%;float:left;display:block}}@media (min-width: 1024px), print{.parallax_categorized_teaser .detailed_def_visible .detailed_definition{max-width:740px;left:6%}}.parallax_categorized_teaser .detailed_def_visible .mobile_detailed_definition{display:none;padding:0 1.5em;text-align:left}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .mobile_detailed_definition{display:none}}.parallax_categorized_teaser .detailed_def_visible .mobile_detailed_definition .mobile_detailed_definition_contents a{color:#54b3da}.parallax_categorized_teaser .detailed_def_visible .close_button{display:block;height:35px;width:33px;position:absolute;right:0;top:0;padding:3px;text-decoration:none;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:-moz-none;-ms-user-select:none;user-select:none}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .close_button{top:0;right:-5px}}.parallax_categorized_teaser .detailed_def_visible .close_button .close_icon{display:block;padding:0;cursor:pointer;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -25px 0;background-size:300px}.parallax_categorized_teaser .detailed_def_visible .close_button .close_icon:hover,.parallax_categorized_teaser .detailed_def_visible .close_button .close_icon.active,.parallax_categorized_teaser .detailed_def_visible .close_button .close_icon.current{background-position:-25px 0}.parallax_categorized_teaser .detailed_def_visible p{color:#e3e3e3}.parallax_categorized_teaser .detailed_def_visible .detailed_def_title{padding:0 3%;text-decoration:none;cursor:pointer;color:#eeaaa1;font-size:1.1em;font-weight:400;display:block;text-transform:uppercase}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .detailed_def_title{padding:0 10% 0 0;margin:.1em 0 0.5em;transition:color 200ms}.parallax_categorized_teaser .detailed_def_visible .detailed_def_title:hover{color:white}}.parallax_categorized_teaser .detailed_def_visible .definition_contents{padding:3%}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .definition_contents{padding:0 20px 0 0}}.parallax_categorized_teaser .detailed_def_visible .definition_contents h1,.parallax_categorized_teaser .detailed_def_visible .definition_contents h2,.parallax_categorized_teaser .detailed_def_visible .definition_contents h3,.parallax_categorized_teaser .detailed_def_visible .definition_contents h4,.parallax_categorized_teaser .detailed_def_visible .definition_contents h5{font-weight:500}.parallax_categorized_teaser .detailed_def_visible .definition_contents h3{font-size:1.1em;margin:0.6em 0 0.6em}.parallax_categorized_teaser .detailed_def_visible .definition_contents h3:first-child{margin-top:0}.parallax_categorized_teaser .detailed_def_visible .definition_contents a{color:#54b3da}.parallax_categorized_teaser .detailed_def_visible .oculus{display:block}@media (min-width: 769px){.parallax_categorized_teaser .detailed_def_visible .oculus{display:none}}.parallax_categorized_teaser .detailed_def_visible .oculus.current{height:auto;pointer-events:none}.parallax_categorized_teaser .detailed_def_visible .oculus.current a{pointer-events:auto}.parallax_categorized_teaser .detailed_def_visible .oculus.current h2.title,.parallax_categorized_teaser .detailed_def_visible .oculus.current .hover{display:none !important}.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition{display:block}.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition h1,.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition h2,.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition h3,.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition h4,.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition h5{font-weight:500}.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition .mobile_detailed_definition_title{margin-top:0;width:85%;font-weight:300}.parallax_categorized_teaser .detailed_def_visible .oculus.current .mobile_detailed_definition .close_button{pointer-events:auto;right:1em}.parallax_categorized_teaser .detailed_def_visible .detailed_def_nav{margin-bottom:1.5em;width:93%}.parallax_categorized_teaser .detailed_def_visible .detailed_def_nav li{cursor:pointer;display:inline-block;font-weight:300;font-size:1.1em;line-height:1.7em;margin-right:1em;transition:color 200ms}.parallax_categorized_teaser .detailed_def_visible .detailed_def_nav li:hover{color:white}.parallax_categorized_teaser .detailed_def_visible .detailed_def_nav li:last-child{margin-right:0}.parallax_categorized_teaser .detailed_def_visible .detailed_def_nav .current{color:white}#iframe_overlay .explore_overlay_page{background-color:black}@keyframes pulse{0%{opacity:1}50%{opacity:.3}100%{opacity:1}}.dsn_connection .target_name,.mars_relay_connection .target_name{display:inline-block;width:calc(100% - 61px);vertical-align:middle;white-space:normal}.dsn_connection .info_tip ~ .dsn_title,.dsn_connection .info_tip ~ .mars_relay_title,.mars_relay_connection .info_tip ~ .dsn_title,.mars_relay_connection .info_tip ~ .mars_relay_title{float:left}.dsn_connection .mars_relay_title,.dsn_connection .dsn_title,.mars_relay_connection .mars_relay_title,.mars_relay_connection .dsn_title{max-width:calc(100% - 30px);display:inline-block}.dsn_connection .signal,.mars_relay_connection .signal{font-size:13px;margin:0.7em 0;white-space:nowrap;font-weight:300}@media (min-width: 480px){.dsn_connection .signal,.mars_relay_connection .signal{font-size:15px}}.dsn_connection .signal .dsn_icon,.mars_relay_connection .signal .dsn_icon{display:inline-block;width:54px;height:54px;vertical-align:middle;margin-right:0.8em}.homepage_dashboard_modal .dsn_connection .signal .sending_receiving,.homepage_dashboard_modal .dsn_connection .signal .sending,.homepage_dashboard_modal .dsn_connection .signal .receiving,.homepage_dashboard_modal .mars_relay_connection .signal .sending_receiving,.homepage_dashboard_modal .mars_relay_connection .signal .sending,.homepage_dashboard_modal .mars_relay_connection .signal .receiving{color:white}.homepage_dashboard_modal .dsn_connection .signal .sending_receiving .target_name,.homepage_dashboard_modal .dsn_connection .signal .sending .target_name,.homepage_dashboard_modal .dsn_connection .signal .receiving .target_name,.homepage_dashboard_modal .mars_relay_connection .signal .sending_receiving .target_name,.homepage_dashboard_modal .mars_relay_connection .signal .sending .target_name,.homepage_dashboard_modal .mars_relay_connection .signal .receiving .target_name{color:white}.homepage_dashboard_modal .dsn_connection .signal .transmissions,.homepage_dashboard_modal .dsn_connection .signal .target_name,.homepage_dashboard_modal .mars_relay_connection .signal .transmissions,.homepage_dashboard_modal .mars_relay_connection .signal .target_name{color:#fcb963}.dsn_connection .signal .sending_receiving,.mars_relay_connection .signal .sending_receiving{animation:pulse 1s infinite}.dsn_connection .signal .sending_receiving .dsn_icon,.mars_relay_connection .signal .sending_receiving .dsn_icon{background:url("https://mars.nasa.gov/assets/dsn_ui_sprite@2x.png") -146px -130px no-repeat;background-size:300px}.dsn_connection .signal .sending_receiving .target_name:before,.mars_relay_connection .signal .sending_receiving .target_name:before{content:" SENDING/RECEIVING"}.dsn_connection .signal .sending,.mars_relay_connection .signal .sending{animation:pulse 1s infinite}.dsn_connection .signal .sending .dsn_icon,.mars_relay_connection .signal .sending .dsn_icon{background:url("https://mars.nasa.gov/assets/dsn_ui_sprite@2x.png") 0 -130px no-repeat;background-size:300px}.dsn_connection .signal .sending .target_name:before,.mars_relay_connection .signal .sending .target_name:before{content:" SENDING"}.dsn_connection .signal .receiving,.mars_relay_connection .signal .receiving{animation:pulse 1s infinite}.dsn_connection .signal .receiving .dsn_icon,.mars_relay_connection .signal .receiving .dsn_icon{background:url("https://mars.nasa.gov/assets/dsn_ui_sprite@2x.png") -73px -130px no-repeat;background-size:300px}.dsn_connection .signal .receiving .target_name:before,.mars_relay_connection .signal .receiving .target_name:before{content:" RECEIVING"}.dsn_connection .signal .disconnected .dsn_icon,.mars_relay_connection .signal .disconnected .dsn_icon{background:url("https://mars.nasa.gov/assets/dsn_ui_sprite@2x.png") -219px -130px no-repeat;background-size:300px}.dsn_connection .signal .disconnected .target_name:before,.mars_relay_connection .signal .disconnected .target_name:before{content:" AWAITING NEXT TRANSMISSION"}.homepage_dashboard_modal .dsn_connection .signal .dsn_icon,.homepage_dashboard_modal .mars_relay_connection .signal .dsn_icon{background-position-y:0px}#iframe_overlay .dsn_connection .signal .dsn_icon,#iframe_overlay .mars_relay_connection .signal .dsn_icon{background-position-y:-62px}.dsn_connection .info_tip,.mars_relay_connection .info_tip{display:inline-block;margin-left:10px}#secondary_column .dsn_connection .info_tip,#secondary_column .mars_relay_connection .info_tip{margin-left:0}@media (min-width: 1024px), print{#secondary_column .dsn_connection .info_tip,#secondary_column .mars_relay_connection .info_tip{margin-left:10px}}.dsn_connection .info_tip .info_icon,.mars_relay_connection .info_tip .info_icon{display:block;width:25px;height:25px;background:url("https://mars.nasa.gov/assets/ui_sprite@2x.png") -100px 0;background-size:300px;position:absolute;top:-1px}#secondary_column .dsn_connection .info_tip .info_icon,#secondary_column .mars_relay_connection .info_tip .info_icon{right:0;top:-3px}#primary_column .dsn_connection .info_tip .info_icon,#primary_column .mars_relay_connection .info_tip .info_icon{top:2px}#primary_column .dsn_connection .info_tip .info_text,#primary_column .mars_relay_connection .info_tip .info_text{top:-13px}.dsn_connection .info_tip .info_text,.mars_relay_connection .info_tip .info_text{display:none}.dsn_connection .info_tip.open .info_icon:before,.mars_relay_connection .info_tip.open .info_icon:before{display:none}@media (min-width: 480px){.dsn_connection .info_tip.open .info_icon:before,.mars_relay_connection .info_tip.open .info_icon:before{display:block;content:'';position:absolute;right:calc(50% - 7.5px);width:15px;height:15px;background:white;border-right:1px solid #c9c9c9;border-bottom:1px solid #c9c9c9;z-index:1;transform:rotate(45deg);top:-23px}}@media (min-width: 769px), print{.dsn_connection .info_tip.open .info_icon:before,.mars_relay_connection .info_tip.open .info_icon:before{top:-23px}}@media (min-width: 1200px){.dsn_connection .info_tip.open .info_icon:before,.mars_relay_connection .info_tip.open .info_icon:before{top:-24px}}.dsn_connection .info_tip.open .info_text,.mars_relay_connection .info_tip.open .info_text{display:block;position:absolute;color:#222;font-size:.9em;background-color:white;padding:1.3em;border:1px solid #c9c9c9;border-radius:4px;top:-17px;transform:translateY(-100%);width:100%;left:0}@media (min-width: 1024px){.dsn_connection .info_tip.open .info_text,.mars_relay_connection .info_tip.open .info_text{font-size:.8em}}.homepage_dashboard_modal .dsn_connection .info_tip.open .info_icon:before,.homepage_dashboard_modal .mars_relay_connection .info_tip.open .info_icon:before{background-color:#221307;border-right:1px solid #5f4326;border-bottom:1px solid #5f4326}.homepage_dashboard_modal .dsn_connection .info_tip.open .info_text,.homepage_dashboard_modal .mars_relay_connection .info_tip.open .info_text{color:#beb0a4;background-color:#221307;border:1px solid #5f4326}.mars_relay_connection .transmissions{display:inline-block;vertical-align:middle;width:calc(100% - 70px);white-space:normal}.parallax_categorized_teaser{font-weight:400}.parallax_categorized_teaser nav.mobile_catcont_nav{display:block}@media (min-width: 769px){.parallax_categorized_teaser nav.mobile_catcont_nav{display:none}}.parallax_categorized_teaser nav.desktop_catcont_nav{display:none}@media (min-width: 769px){.parallax_categorized_teaser nav.desktop_catcont_nav{display:block}}.parallax_categorized_teaser .nav_content_container{display:block}@media (min-width: 769px){.parallax_categorized_teaser .nav_content_container{max-width:1300px;width:94%;margin:auto;display:flex;height:100%}}.parallax_categorized_teaser nav.catcont_nav{width:260px;margin-right:3%}@media (min-width: 1024px), print{.parallax_categorized_teaser nav.catcont_nav{width:300px;margin-right:4%}}@media (min-width: 1200px){.parallax_categorized_teaser nav.catcont_nav{width:340px;margin-right:5.5%}}@media (min-width: 1700px){.parallax_categorized_teaser nav.catcont_nav{margin-right:7.5%}}.parallax_categorized_teaser nav.catcont_nav .section{background-color:rgba(129,33,24,0.4);margin-bottom:1px;cursor:pointer;text-align:center;padding:14px 5%;transition:background-color 200ms}.no-touchevents .parallax_categorized_teaser nav.catcont_nav .section:hover:not(.current){background-color:rgba(129,33,24,0.8)}@media (min-width: 480px){.parallax_categorized_teaser nav.catcont_nav .section{padding:14px 10%}}@media (min-width: 600px), print{.parallax_categorized_teaser nav.catcont_nav .section{padding:14px 15%}}@media (min-width: 769px){.parallax_categorized_teaser nav.catcont_nav .section{text-align:left;padding:10px 15px}}.parallax_categorized_teaser nav.catcont_nav .section .nav_title,.parallax_categorized_teaser nav.catcont_nav .section .title{text-transform:uppercase;font-size:15px;letter-spacing:0}.parallax_categorized_teaser nav.catcont_nav .section.default .category_info .description{display:block}.parallax_categorized_teaser nav.catcont_nav .category_info .description{display:none;font-size:16px;margin:.5em 0}@media (min-width: 480px){.parallax_categorized_teaser nav.catcont_nav .category_info .description{font-size:15px}}@media (min-width: 769px){.parallax_categorized_teaser nav.catcont_nav .current{background-color:#822118}.parallax_categorized_teaser nav.catcont_nav .current .category_info .title{color:white}}.parallax_categorized_teaser nav.mobile_catcont_nav{width:100%}.parallax_categorized_teaser nav.mobile_catcont_nav.current .category_info .description{display:block}.parallax_categorized_teaser nav.mobile_catcont_nav .category_info .description{margin:.5em 0;display:none}.-ms-.no-flexbox .grid_view.grid_gallery .list_image img{height:auto}
      </style>
      <style data-href="/assets/gulp/vendor/jquery.fancybox-364352e03618ba5a8da007665b1f0be31795293b22bc4d7c5974891d4976a137.css" media="screen">
       @charset "UTF-8";/*! fancyBox 3.0.0 Beta 1 fancyapps.com | fancyapps.com/fancybox/#license */#fancybox-loading,#fancybox-lock,.fancybox-wrap,.fancybox-skin,.fancybox-inner,.fancybox-error,.fancybox-image{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}.fancybox-wrap iframe,.fancybox-wrap object,.fancybox-wrap embed{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}a.fancybox-close,a.fancybox-expand{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}a.fancybox-nav{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}a.fancybox-nav span{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}.fancybox-tmp{padding:0;margin:0;border:0;outline:none;vertical-align:top;background-color:transparent;background-repeat:no-repeat;background-image:none;text-shadow:none}#fancybox-lock{position:fixed;top:0;left:0;right:0;bottom:0;z-index:8020;overflow-y:scroll;overflow-y:auto;overflow-x:auto;-webkit-transition:-webkit-transform 0.5s;-webkit-transform:translateX(0px)}.fancybox-lock-test{overflow-y:hidden !important}.fancybox-lock{overflow:hidden !important;width:auto}.fancybox-lock body{overflow:hidden !important}.fancybox-wrap{position:absolute;top:0;left:0;z-index:8020;-webkit-transform:translate3d(0, 0, 0)}.fancybox-opened{z-index:8030}.fancybox-skin{border-style:solid;border-color:#fff;background:#fff;color:#444}.fancybox-inner{position:relative;overflow:hidden;-webkit-overflow-scrolling:touch;width:100%;height:100%;max-width:100%;max-height:100%}.fancybox-spacer{position:absolute;top:100%;left:0;width:1px}.fancybox-image,.fancybox-iframe{display:block;width:100%;height:100%}.fancybox-image{max-width:100%;max-height:100%;zoom:1}a.fancybox-close{position:absolute;top:-23px;right:-23px;width:46px;height:46px;cursor:pointer;background-position:0 0;z-index:8040}a.fancybox-nav{position:absolute;top:0;width:50%;height:100%;cursor:pointer;text-decoration:none;-webkit-tap-highlight-color:transparent;z-index:8040;overflow:hidden}.fancybox-type-iframe a.fancybox-nav,.fancybox-type-inline a.fancybox-nav,.fancybox-type-html a.fancybox-nav{width:70px}a.fancybox-prev{left:-70px}a.fancybox-next{right:-70px}a.fancybox-nav span{position:absolute;top:50%;width:46px;height:46px;margin-top:-23px;cursor:pointer;z-index:8040}a.fancybox-prev span{left:0;background-position:0 -50px}a.fancybox-next span{right:0;background-position:0 -100px}.fancybox-mobile a.fancybox-nav{max-width:80px}.fancybox-desktop a.fancybox-nav{opacity:0.5;filter:alpha(opacity=50)}.fancybox-desktop a.fancybox-nav:hover{opacity:1;filter:alpha(opacity=100)}a.fancybox-expand{position:absolute;bottom:0;right:0;width:46px;height:46px;z-index:8050;opacity:0;filter:alpha(opacity=0);background-position:0 -150px;zoom:1;-webkit-transition:opacity .5s ease;-moz-transition:opacity .5s ease;-o-transition:opacity .5s ease;transition:opacity .5s ease}.fancybox-wrap:hover a.fancybox-expand{opacity:0.5;filter:alpha(opacity=50)}.fancybox-wrap a.fancybox-expand:hover{opacity:1;filter:alpha(opacity=100)}#fancybox-loading{position:fixed;top:50%;left:50%;margin-top:-30px;margin-left:-30px;width:60px;height:60px;background-color:#111;background-image:url(data:image/gif;base64,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);background-position:center center;opacity:0.85;filter:alpha(opacity=85);cursor:pointer;z-index:8060;-webkit-border-radius:8px;-moz-border-radius:8px;border-radius:8px}.fancybox-tmp{position:absolute !important;top:-99999px;left:-99999px;max-width:99999px;max-height:99999px;overflow:visible !important}.fancybox-title{font:normal 14px "Helvetica Neue",Helvetica,Arial,sans-serif;line-height:1.5;position:relative;text-shadow:none;z-index:8050;display:block;visibility:hidden}.fancybox-title-float-wrap{position:relative;margin-top:10px;text-align:center;zoom:1;left:-9999px}.fancybox-title-float-wrap&gt;div{display:inline-block;padding:7px 20px;font-weight:bold;color:#FFF;text-shadow:0 1px 2px #222;background:transparent;background:rgba(0,0,0,0.8);-webkit-border-radius:15px;-moz-border-radius:15px;border-radius:15px}.fancybox-title-outside-wrap{position:relative;margin-top:10px;color:#fff;text-shadow:0 1px rgba(0,0,0,0.5)}.fancybox-title-inside-wrap{padding-top:10px}.fancybox-title-over-wrap{position:absolute;bottom:0;left:0;color:#fff;padding:15px;background:#000;background:rgba(0,0,0,0.8);max-height:50%;overflow:auto}.fancybox-overlay{position:absolute;top:0;left:0;overflow:hidden;z-index:8010}.fancybox-overlay-fixed{position:fixed;width:100%;height:100%}.fancybox-default-skin{border-color:#f9f9f9;background:#f9f9f9}.fancybox-default-skin-open{box-shadow:0 10px 25px rgba(0,0,0,0.5)}.fancybox-default-overlay{background:#333;opacity:0.8;filter:alpha(opacity=80)}.fancybox-default a.fancybox-close,.fancybox-default a.fancybox-expand,.fancybox-default a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC4AAADICAYAAACXpNOoAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1RkZERjA4NTZBNEMxMUUyOTFGMkY4MEVGREQ0MkRDNCIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1RkZERjA4NDZBNEMxMUUyOTFGMkY4MEVGREQ0MkRDNCIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOkU2OUM1RDBBNEI2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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)}@media only screen and (-webkit-min-device-pixel-ratio: 2), only screen and (-moz-min-device-pixel-ratio: 2), only screen and (-o-min-device-pixel-ratio: 2 / 1), only screen and (min-device-pixel-ratio: 2), only screen and (min-resolution: 2dppx){.fancybox-default a.fancybox-close,.fancybox-default a.fancybox-expand,.fancybox-default a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFwAAAGQCAYAAAAjsgcjAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpCMTg4NzhCQTZBNEYxMUUyQTQ2NEQ0Nzc1M0U1REU1MSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDpCMTg4NzhCOTZBNEYxMUUyQTQ2NEQ0Nzc1M0U1REU1MSIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjE0QzZBQjVDNEU2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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);background-size:46px auto}}.fancybox-dark a.fancybox-close,.fancybox-dark a.fancybox-expand,.fancybox-dark a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC4AAADICAYAAACXpNOoAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1OTJGQjgwRDZBNEQxMUUyOEJDREM1NUU4QUUxNjBFMCIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1OTJGQjgwQzZBNEQxMUUyOEJDREM1NUU4QUUxNjBFMCIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOkU2OUM1RDBBNEI2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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)}.fancybox-dark-skin{background:#2A2A2A;border-color:#2A2A2A;color:#fff;border-radius:4px;box-shadow:0 0 10px rgba(0,0,0,0.3) inset !important}.fancybox-dark-overlay{background:#000;opacity:0.8;filter:alpha(opacity=80)}@media only screen and (-webkit-min-device-pixel-ratio: 2), only screen and (min--moz-device-pixel-ratio: 2), only screen and (-o-min-device-pixel-ratio: 2 / 1), only screen and (min-device-pixel-ratio: 2), only screen and (min-resolution: 192dpi), only screen and (min-resolution: 2dppx){.fancybox-dark a.fancybox-close,.fancybox-dark a.fancybox-expand,.fancybox-dark a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFwAAAGQCAYAAAAjsgcjAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDoyMzAwM0E4MDZBNEQxMUUyQUMyMDg1MkQ4RkQxRDJCNCIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDoyMzAwM0E3RjZBNEQxMUUyQUMyMDg1MkQ4RkQxRDJCNCIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOkU4OUM1RDBBNEI2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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);background-size:46px auto}}.fancybox-light a.fancybox-close,.fancybox-light a.fancybox-expand,.fancybox-light a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAC4AAADICAYAAACXpNOoAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDo1NjIzNzFGMDZBNTUxMUUyQkVBRUY3ODU0RDc4OTlCQyIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDo1NjIzNzFFRjZBNTUxMUUyQkVBRUY3ODU0RDc4OTlCQyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjE5QzZBQjVDNEU2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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)}.fancybox-light-skin-open{box-shadow:0 10px 25px rgba(0,0,0,0.5)}@media only screen and (-webkit-min-device-pixel-ratio: 2), only screen and (-moz-min-device-pixel-ratio: 2), only screen and (-o-min-device-pixel-ratio: 2 / 1), only screen and (min-device-pixel-ratio: 2), only screen and (min-resolution: 2dppx){    .fancybox-light a.fancybox-close{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFwAAAGQCAYAAAAjsgcjAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpEMEQwOUQ1MjZBNEUxMUUyQjJGNkY3NDBEMEE5NDY5NyIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDpEMEQwOUQ1MTZBNEUxMUUyQjJGNkY3NDBEMEE5NDY5NyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjE0QzZBQjVDNEU2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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);background-size:46px auto}.fancybox-light a.fancybox-expand,.fancybox-light a.fancybox-nav span{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFwAAAGQCAYAAAAjsgcjAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA2ZpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpGNzRGRjc2NzEwNERFMjExQTc0M0U0NzZGQkE0MTM5RSIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpEMEQwOUQ1MjZBNEUxMUUyQjJGNkY3NDBEMEE5NDY5NyIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDpEMEQwOUQ1MTZBNEUxMUUyQjJGNkY3NDBEMEE5NDY5NyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ1M2IChXaW5kb3dzKSI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOjE0QzZBQjVDNEU2QUUyMTE5NTdDREVCQjFFNDc0RjQzIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkY3NEZGNzY3MTA0REUyMTFBNzQzRTQ3NkZCQTQxMzlFIi8+IDwvcmRmOkRlc2NyaXB0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);background-size:46px auto}}.fancybox-light-overlay{opacity:0.9;filter:alpha(opacity=90);background:#555555;background:-moz-radial-gradient(center, ellipse cover, #999 0%, #555 100%);background:-webkit-gradient(radial, center center, 0px, center center, 100%, color-stop(0%, #999), color-stop(100%, #555));background:-webkit-radial-gradient(center, ellipse cover, #999 0%, #555 100%);background:-o-radial-gradient(center, ellipse cover, #999 0%, #555 100%);background:-ms-radial-gradient(center, ellipse cover, #999 0%, #555 100%)}
      </style>
      <style data-href="/assets/gulp/print-240f8bfaa7f6402dfd6c49ee3c1ffea57a89ddd4c8c90e2f2a5c7d63c5753e32.css" media="print">
       .print_only{display:block}body{overflow:hidden}.print_logo{position:absolute;top:0;left:0}.site_header_area{position:relative}.nav_is_fixed .site_header_area{position:absolute;top:0}.site_header_area .brand1,.site_header_area .brand2{display:none}.site_header_area .brand_area{width:23%}.site_header_area .grace_logo img.grace_logo_white{display:none}.custom_banner_container{height:68px}.custom_banner_container img{display:none}.custom_banner_container .banner_header_overlay{display:none}a[href]:after{content:""}.module{padding:1em 0}#sticky_nav_spacer{display:none}.nav_is_fixed #sticky_nav_spacer{display:block}.main_carousel.module .slick-slider .grid_layout{width:100%}.main_carousel.module .slick-slider .right-col{width:3.75in !important;float:left;margin-right:12px;margin-bottom:1em}.main_carousel.module .slick-slider .left-col{width:3.75in !important;float:left}.definition_teaser{display:none;color:white !important}.double_teaser .column{width:48%;float:left}.double_teaser .column+.column{margin-left:1%;margin-top:0}#home .site_header_area{position:relative}#site_footer{border-top:1px solid gray}#site_footer .upper_footer{padding:1em 0}#site_footer .footer_science_calendar footer{display:none}#site_footer .footer_science_calendar .col1,#site_footer .footer_science_calendar .col2,#site_footer .footer_science_calendar .col3{display:inline-block;width:30%;padding:1%}#site_footer .sitemap,#site_footer .share{display:none}#site_footer .lower_footer{height:auto}#site_footer .lower_footer .nav_container{display:none}#primary_column{width:60%;float:left;overflow:hidden;position:relative;display:block}#secondary_column{width:32%;float:right;position:relative;font-size:80%}.double_teaser .column{width:46%}.double_teaser .column+.column{float:right}.grid_view .module_title{display:block}body #page .grid_gallery.grid_view li.slide{width:19%;margin:1%;float:left;clear:none}body #page .grid_gallery.grid_view .bottom_gradient{margin-top:0}body #page .grid_gallery.grid_view .bottom_gradient div{margin-top:.3em}.gradient_line{display:none}.multi_teaser,.teasers_module,.multimedia_teaser,.filter_bar,.tertiary_nav_container,.secondary_nav_mobile,.carousel_teaser,.image_of_the_day,.view_selectors,.related,.primary_media_feature,.fancybox-overlay,#fancybox-lock,.suggested_features,.homepage_carousel,#site_footer .brand_area{display:none}
      </style>
      <script src="/assets/public_manifest-29f615d060fd77c4129af3d227623c24ff69d65ee804dd6d8735c19592f911f3.js">
      </script>
      <!--[if gt IE 8]><!-->
      <script src="/assets/not_ie8_manifest.js">
      </script>
      <style>
      </style>
      <!--[if !IE]>-->
      <script src="/assets/not_ie8_manifest.js">
      </script>
      <style>
      </style>
      <!--<![endif]-->
      <script src="/assets/vendor/jquery.fancybox-9d361e5f98a5c0f233a25df9252dbadea6897af1c0ef221d7465e1205941ea0d.js">
      </script>
      <script src="/assets/mb_manifest-86cde101f747092d1465039f6da3fd2930c66319387c196503d3f70665195392.js">
      </script>
      <!-- /twitter cards -->
      <meta content="summary_large_image" name="twitter:card"/>
      <meta content="News " name="twitter:title"/>
      <meta content="NASA’s real-time portal for Mars exploration, featuring the latest news, images, and discoveries from the Red Planet." name="twitter:description"/>
      <meta content="https://mars.nasa.gov/system/site_config_values/meta_share_images/1_142497main_PIA03154-200.jpg" name="twitter:image"/>
      <style type="text/css">
       .fancybox-margin{margin-right:0px;}
      </style>
      <style type="text/css">
       .at-icon{fill:#fff;border:0}.at-icon-wrapper{display:inline-block;overflow:hidden}a .at-icon-wrapper{cursor:pointer}.at-rounded,.at-rounded-element .at-icon-wrapper{border-radius:12%}.at-circular,.at-circular-element .at-icon-wrapper{border-radius:50%}.addthis_32x32_style .at-icon{width:2pc;height:2pc}.addthis_24x24_style .at-icon{width:24px;height:24px}.addthis_20x20_style .at-icon{width:20px;height:20px}.addthis_16x16_style .at-icon{width:1pc;height:1pc}#at16lb{display:none;position:absolute;top:0;left:0;width:100%;height:100%;z-index:1001;background-color:#000;opacity:.001}#at_complete,#at_error,#at_share,#at_success{position:static!important}.at15dn{display:none}#at15s,#at16p,#at16p form input,#at16p label,#at16p textarea,#at_share .at_item{font-family:arial,helvetica,tahoma,verdana,sans-serif!important;font-size:9pt!important;outline-style:none;outline-width:0;line-height:1em}* html #at15s.mmborder{position:absolute!important}#at15s.mmborder{position:fixed!important;width:250px!important}#at15s{background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABtJREFUeNpiZGBgaGAgAjAxEAlGFVJHIUCAAQDcngCUgqGMqwAAAABJRU5ErkJggg==);float:none;line-height:1em;margin:0;overflow:visible;padding:5px;text-align:left;position:absolute}#at15s a,#at15s span{outline:0;direction:ltr;text-transform:none}#at15s .at-label{margin-left:5px}#at15s .at-icon-wrapper{width:1pc;height:1pc;vertical-align:middle}#at15s .at-icon{width:1pc;height:1pc}.at4-icon{display:inline-block;background-repeat:no-repeat;background-position:top left;margin:0;overflow:hidden;cursor:pointer}.addthis_16x16_style .at4-icon,.addthis_default_style .at4-icon,.at4-icon,.at-16x16{width:1pc;height:1pc;line-height:1pc;background-size:1pc!important}.addthis_32x32_style .at4-icon,.at-32x32{width:2pc;height:2pc;line-height:2pc;background-size:2pc!important}.addthis_24x24_style .at4-icon,.at-24x24{width:24px;height:24px;line-height:24px;background-size:24px!important}.addthis_20x20_style .at4-icon,.at-20x20{width:20px;height:20px;line-height:20px;background-size:20px!important}.at4-icon.circular,.circular .at4-icon,.circular.aticon{border-radius:50%}.at4-icon.rounded,.rounded .at4-icon{border-radius:4px}.at4-icon-left{float:left}#at15s .at4-icon{text-indent:20px;padding:0;overflow:visible;white-space:nowrap;background-size:1pc;width:1pc;height:1pc;background-position:top left;display:inline-block;line-height:1pc}.addthis_vertical_style .at4-icon,.at4-follow-container .at4-icon{margin-right:5px}html&gt;body #at15s{width:250px!important}#at15s.atm{background:none!important;padding:0!important;width:10pc!important}#at15s_inner{background:#fff;border:1px solid #fff;margin:0}#at15s_head{position:relative;background:#f2f2f2;padding:4px;cursor:default;border-bottom:1px solid #e5e5e5}.at15s_head_success{background:#cafd99!important;border-bottom:1px solid #a9d582!important}.at15s_head_success a,.at15s_head_success span{color:#000!important;text-decoration:none}#at15s_brand,#at15sptx,#at16_brand{position:absolute}#at15s_brand{top:4px;right:4px}.at15s_brandx{right:20px!important}a#at15sptx{top:4px;right:4px;text-decoration:none;color:#4c4c4c;font-weight:700}#at15sptx:hover{text-decoration:underline}#at16_brand{top:5px;right:30px;cursor:default}#at_hover{padding:4px}#at_hover .at_item,#at_share .at_item{background:#fff!important;float:left!important;color:#4c4c4c!important}#at_share .at_item .at-icon-wrapper{margin-right:5px}#at_hover .at_bold{font-weight:700;color:#000!important}#at_hover .at_item{width:7pc!important;padding:2px 3px!important;margin:1px;text-decoration:none!important}#at_hover .at_item.athov,#at_hover .at_item:focus,#at_hover .at_item:hover{margin:0!important}#at_hover .at_item.athov,#at_hover .at_item:focus,#at_hover .at_item:hover,#at_share .at_item.athov,#at_share .at_item:hover{background:#f2f2f2!important;border:1px solid #e5e5e5;color:#000!important;text-decoration:none}.ipad #at_hover .at_item:focus{background:#fff!important;border:1px solid #fff}.at15t{display:block!important;height:1pc!important;line-height:1pc!important;padding-left:20px!important;background-position:0 0;text-align:left}.addthis_button,.at15t{cursor:pointer}.addthis_toolbox a.at300b,.addthis_toolbox a.at300m{width:auto}.addthis_toolbox a{margin-bottom:5px;line-height:initial}.addthis_toolbox.addthis_vertical_style{width:200px}.addthis_button_facebook_like .fb_iframe_widget{line-height:100%}.addthis_button_facebook_like iframe.fb_iframe_widget_lift{max-width:none}.addthis_toolbox a.addthis_button_counter,.addthis_toolbox a.addthis_button_facebook_like,.addthis_toolbox a.addthis_button_facebook_send,.addthis_toolbox a.addthis_button_facebook_share,.addthis_toolbox a.addthis_button_foursquare,.addthis_toolbox a.addthis_button_google_plusone,.addthis_toolbox a.addthis_button_linkedin_counter,.addthis_toolbox a.addthis_button_pinterest_pinit,.addthis_toolbox a.addthis_button_stumbleupon_badge,.addthis_toolbox a.addthis_button_tweet{display:inline-block}.at-share-tbx-element .google_plusone_iframe_widget&gt;span&gt;div{vertical-align:top!important}.addthis_toolbox span.addthis_follow_label{display:none}.addthis_toolbox.addthis_vertical_style span.addthis_follow_label{display:block;white-space:nowrap}.addthis_toolbox.addthis_vertical_style a{display:block}.addthis_toolbox.addthis_vertical_style.addthis_32x32_style a{line-height:2pc;height:2pc}.addthis_toolbox.addthis_vertical_style .at300bs{margin-right:4px;float:left}.addthis_toolbox.addthis_20x20_style span{line-height:20px}.addthis_toolbox.addthis_32x32_style span{line-height:2pc}.addthis_toolbox.addthis_pill_combo_style .addthis_button_compact .at15t_compact,.addthis_toolbox.addthis_pill_combo_style a{float:left}.addthis_toolbox.addthis_pill_combo_style a.addthis_button_tweet{margin-top:-2px}.addthis_toolbox.addthis_pill_combo_style .addthis_button_compact .at15t_compact{margin-right:4px}.addthis_default_style .addthis_separator{margin:0 5px;display:inline}div.atclear{clear:both}.addthis_default_style .addthis_separator,.addthis_default_style .at4-icon,.addthis_default_style .at300b,.addthis_default_style .at300bo,.addthis_default_style .at300bs,.addthis_default_style .at300m{float:left}.at300b img,.at300bo img{border:0}a.at300b .at4-icon,a.at300m .at4-icon{display:block}.addthis_default_style .at300b,.addthis_default_style .at300bo,.addthis_default_style .at300m{padding:0 2px}.at300b,.at300bo,.at300bs,.at300m{cursor:pointer}.addthis_button_facebook_like.at300b:hover,.addthis_button_facebook_like.at300bs:hover,.addthis_button_facebook_send.at300b:hover,.addthis_button_facebook_send.at300bs:hover{opacity:1}.addthis_20x20_style .at15t,.addthis_20x20_style .at300bs{overflow:hidden;display:block;height:20px!important;width:20px!important;line-height:20px!important}.addthis_32x32_style .at15t,.addthis_32x32_style .at300bs{overflow:hidden;display:block;height:2pc!important;width:2pc!important;line-height:2pc!important}.at300bs{overflow:hidden;display:block;background-position:0 0;height:1pc;width:1pc;line-height:1pc!important}.addthis_default_style .at15t_compact,.addthis_default_style .at15t_expanded{margin-right:4px}#at_share .at_item{width:123px!important;padding:4px;margin-right:2px;border:1px solid #fff}#at16p{background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABtJREFUeNpiZGBgaGAgAjAxEAlGFVJHIUCAAQDcngCUgqGMqwAAAABJRU5ErkJggg==);z-index:10000001;position:absolute;top:50%;left:50%;width:300px;padding:10px;margin:0 auto;margin-top:-185px;margin-left:-155px;font-family:arial,helvetica,tahoma,verdana,sans-serif;font-size:9pt;color:#5e5e5e}#at_share{margin:0;padding:0}#at16pt{position:relative;background:#f2f2f2;height:13px;padding:5px 10px}#at16pt a,#at16pt h4{font-weight:700}#at16pt h4{display:inline;margin:0;padding:0;font-size:9pt;color:#4c4c4c;cursor:default}#at16pt a{position:absolute;top:5px;right:10px;color:#4c4c4c;text-decoration:none;padding:2px}#at15sptx:focus,#at16pt a:focus{outline:thin dotted}#at15s #at16pf a{top:1px}#_atssh{width:1px!important;height:1px!important;border:0!important}.atm{width:10pc!important;padding:0;margin:0;line-height:9pt;letter-spacing:normal;font-family:arial,helvetica,tahoma,verdana,sans-serif;font-size:9pt;color:#444;background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABtJREFUeNpiZGBgaGAgAjAxEAlGFVJHIUCAAQDcngCUgqGMqwAAAABJRU5ErkJggg==);padding:4px}.atm-f{text-align:right;border-top:1px solid #ddd;padding:5px 8px}.atm-i{background:#fff;border:1px solid #d5d6d6;padding:0;margin:0;box-shadow:1px 1px 5px rgba(0,0,0,.15)}.atm-s{margin:0!important;padding:0!important}.atm-s a:focus{border:transparent;outline:0;transition:none}#at_hover.atm-s a,.atm-s a{display:block;text-decoration:none;padding:4px 10px;color:#235dab!important;font-weight:400;font-style:normal;transition:none}#at_hover.atm-s .at_bold{color:#235dab!important}#at_hover.atm-s a:hover,.atm-s a:hover{background:#2095f0;text-decoration:none;color:#fff!important}#at_hover.atm-s .at_bold{font-weight:700}#at_hover.atm-s a:hover .at_bold{color:#fff!important}.atm-s a .at-label{vertical-align:middle;margin-left:5px;direction:ltr}.at_PinItButton{display:block;width:40px;height:20px;padding:0;margin:0;background-image:url(//s7.addthis.com/static/t00/pinit00.png);background-repeat:no-repeat}.at_PinItButton:hover{background-position:0 -20px}.addthis_toolbox .addthis_button_pinterest_pinit{position:relative}.at-share-tbx-element .fb_iframe_widget span{vertical-align:baseline!important}#at16pf{height:auto;text-align:right;padding:4px 8px}.at-privacy-info{position:absolute;left:7px;bottom:7px;cursor:pointer;text-decoration:none;font-family:helvetica,arial,sans-serif;font-size:10px;line-height:9pt;letter-spacing:.2px;color:#666}.at-privacy-info:hover{color:#000}.body .wsb-social-share .wsb-social-share-button-vert{padding-top:0;padding-bottom:0}.body .wsb-social-share.addthis_counter_style .addthis_button_tweet.wsb-social-share-button{padding-top:40px}.body .wsb-social-share.addthis_counter_style .addthis_button_google_plusone.wsb-social-share-button{padding-top:0}.body .wsb-social-share.addthis_counter_style .addthis_button_facebook_like.wsb-social-share-button{padding-top:21px}@media print{#at4-follow,#at4-share,#at4-thankyou,#at4-whatsnext,#at4m-mobile,#at15s,.at4,.at4-recommended{display:none!important}}@media screen and (max-width:400px){.at4win{width:100%}}@media screen and (max-height:700px) and (max-width:400px){.at4-thankyou-inner .at4-recommended-container{height:122px;overflow:hidden}.at4-thankyou-inner .at4-recommended .at4-recommended-item:first-child{border-bottom:1px solid #c5c5c5}}
      </style>
      <style type="text/css">
       .at-branding-logo{font-family:helvetica,arial,sans-serif;text-decoration:none;font-size:10px;display:inline-block;margin:2px 0;letter-spacing:.2px}.at-branding-logo .at-branding-icon{background-image:url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAMAAAC67D+PAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAAZQTFRF////+GlNUkcc1QAAAB1JREFUeNpiYIQDBjQmAwMmkwEM0JnY1WIxFyDAABGeAFEudiZsAAAAAElFTkSuQmCC")}.at-branding-logo .at-branding-icon,.at-branding-logo .at-privacy-icon{display:inline-block;height:10px;width:10px;margin-left:4px;margin-right:3px;margin-bottom:-1px;background-repeat:no-repeat}.at-branding-logo .at-privacy-icon{background-image:url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAKCAMAAABR24SMAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABhQTFRF8fr9ot/xXcfn2/P5AKva////////AKTWodjhjAAAAAd0Uk5T////////ABpLA0YAAAA6SURBVHjaJMzBDQAwCAJAQaj7b9xifV0kUKJ9ciWxlzWEWI5gMF65KUTv0VKkjVeTerqE/x7+9BVgAEXbAWI8QDcfAAAAAElFTkSuQmCC")}.at-branding-logo span{text-decoration:none}.at-branding-logo .at-branding-addthis,.at-branding-logo .at-branding-powered-by{color:#666}.at-branding-logo .at-branding-addthis:hover{color:#333}.at-cv-with-image .at-branding-addthis,.at-cv-with-image .at-branding-addthis:hover{color:#fff}a.at-branding-logo:visited{color:initial}.at-branding-info{display:inline-block;padding:0 5px;color:#666;border:1px solid #666;border-radius:50%;font-size:10px;line-height:9pt;opacity:.7;transition:all .3s ease;text-decoration:none}.at-branding-info span{border:0;clip:rect(0 0 0 0);height:1px;margin:-1px;overflow:hidden;padding:0;position:absolute;width:1px}.at-branding-info:before{content:'i';font-family:Times New Roman}.at-branding-info:hover{color:#0780df;border-color:#0780df}
      </style>
      <script async="" charset="utf-8" src="//s7.addthis.com/static/layers.1457328982467cc82fb7.js" type="text/javascript">
      </script>
      <style type="text/css">
       .at-share-dock.atss{top:auto;left:0;right:0;bottom:0;width:100%;max-width:100%;z-index:1000200;box-shadow:0 0 1px 1px #e2dfe2}.at-share-dock.at-share-dock-zindex-hide{z-index:-1!important}.at-share-dock.atss-top{bottom:auto;top:0}.at-share-dock a{width:auto;transition:none;color:#fff;text-decoration:none;box-sizing:content-box;-webkit-box-sizing:content-box;-moz-box-sizing:content-box}.at-share-dock a:hover{width:auto}.at-share-dock .at4-count{height:43px;padding:5px 0 0;line-height:20px;background:#fff;font-family:Helvetica neue,arial}.at-share-dock .at4-count span{width:100%}.at-share-dock .at4-count .at4-share-label{color:#848484;font-size:10px;letter-spacing:1px}.at-share-dock .at4-count .at4-counter{top:2px;position:relative;display:block;color:#222;font-size:22px}.at-share-dock.at-shfs-medium .at4-count{height:36px;line-height:1pc;padding-top:4px}.at-share-dock.at-shfs-medium .at4-count .at4-counter{font-size:18px}.at-share-dock.at-shfs-medium .at-share-btn .at-icon-wrapper,.at-share-dock.at-shfs-medium a .at-icon-wrapper{padding:6px 0}.at-share-dock.at-shfs-small .at4-count{height:26px;line-height:1;padding-top:3px}.at-share-dock.at-shfs-small .at4-count .at4-share-label{font-size:8px}.at-share-dock.at-shfs-small .at4-count .at4-counter{font-size:14px}.at-share-dock.at-shfs-small .at-share-btn .at-icon-wrapper,.at-share-dock.at-shfs-small a .at-icon-wrapper{padding:4px 0}
      </style>
      <style type="text/css">
       div.at-share-close-control.ats-dark,div.at-share-open-control-left.ats-dark,div.at-share-open-control-right.ats-dark{background:#262b30}div.at-share-close-control.ats-light,div.at-share-open-control-left.ats-light,div.at-share-open-control-right.ats-light{background:#fff}div.at-share-close-control.ats-gray,div.at-share-open-control-left.ats-gray,div.at-share-open-control-right.ats-gray{background:#f2f2f2}.atss{position:fixed;top:20%;width:3pc;z-index:100020;background:none}.at-share-close-control{position:relative;width:3pc;overflow:auto}.at-share-open-control-left{position:fixed;top:20%;z-index:100020;left:0;width:22px}.at-share-close-control .at4-arrow.at-left{float:right}.atss-left{left:0;float:left;right:auto}.atss-right{left:auto;float:right;right:0}.atss-right.at-share-close-control .at4-arrow.at-right{position:relative;right:0;overflow:auto}.atss-right.at-share-close-control .at4-arrow{float:left}.at-share-open-control-right{position:fixed;top:20%;z-index:100020;right:0;width:22px;float:right}.atss-right .at-share-close-control .at4-arrow{float:left}.atss.atss-right a{float:right}.atss.atss-right .at4-share-title{float:right;overflow:hidden}.atss .at-share-btn,.atss a{position:relative;display:block;width:3pc;margin:0;outline-offset:-1px;text-align:center;float:left;transition:width .15s ease-in-out;overflow:hidden;background:#e8e8e8;z-index:100030;cursor:pointer}.at-share-btn::-moz-focus-inner{border:0;padding:0}.atss-right .at-share-btn{float:right}.atss .at-share-btn{border:0;padding:0}.atss .at-share-btn:focus,.atss .at-share-btn:hover,.atss a:focus,.atss a:hover{width:4pc}.atss .at-share-btn .at-icon-wrapper,.atss a .at-icon-wrapper{display:block;padding:8px 0}.atss .at-share-btn:last-child,.atss a:last-child{border:none}.atss .at-share-btn span .at-icon,.atss a span .at-icon{position:relative;top:0;left:0;display:block;background-repeat:no-repeat;background-position:50% 50%;width:2pc;height:2pc;line-height:2pc;border:none;padding:0;margin:0 auto;overflow:hidden;cursor:pointer;cursor:hand}.at4-share .at-custom-sidebar-counter{font-family:Helvetica neue,arial;vertical-align:top;margin-right:4px;display:inline-block;text-align:center}.at4-share .at-custom-sidebar-count{font-size:17px;line-height:1.25em;color:#222}.at4-share .at-custom-sidebar-text{font-size:9px;line-height:1.25em;color:#888;letter-spacing:1px}.at4-share .at4-share-count-container{position:absolute;left:0;right:auto;top:auto;bottom:0;width:100%;color:#fff;background:inherit}.at4-share .at4-share-count,.at4-share .at4-share-count-container{line-height:1pc;font-size:10px}.at4-share .at4-share-count{text-indent:0;font-family:Arial,Helvetica Neue,Helvetica,sans-serif;font-weight:200;width:100%;height:1pc}.at4-share .at4-share-count-anchor{padding-bottom:8px;text-decoration:none;transition:padding .15s ease-in-out .15s,width .15s ease-in-out}
      </style>
      <style type="text/css">
       #at4-drawer-outer-container{top:0;width:20pc;position:fixed}#at4-drawer-outer-container.at4-drawer-inline{position:relative}#at4-drawer-outer-container.at4-drawer-inline.at4-drawer-right{float:right;right:0;left:auto}#at4-drawer-outer-container.at4-drawer-inline.at4-drawer-left{float:left;left:0;right:auto}#at4-drawer-outer-container.at4-drawer-shown,#at4-drawer-outer-container.at4-drawer-shown *{z-index:999999}#at4-drawer-outer-container,#at4-drawer-outer-container .at4-drawer-outer,#at-drawer{height:100%;overflow-y:auto;overflow-x:hidden}.at4-drawer-push-content-right-back{position:relative;right:0}.at4-drawer-push-content-right{position:relative;left:20pc!important}.at4-drawer-push-content-left-back{position:relative;left:0}.at4-drawer-push-content-left{position:relative;right:20pc!important}#at4-drawer-outer-container.at4-drawer-right{left:auto;right:-20pc}#at4-drawer-outer-container.at4-drawer-left{right:auto;left:-20pc}#at4-drawer-outer-container.at4-drawer-shown.at4-drawer-right{left:auto;right:0}#at4-drawer-outer-container.at4-drawer-shown.at4-drawer-left{right:auto;left:0}#at-drawer{top:0;z-index:9999999;height:100%;animation-duration:.4s}#at-drawer.drawer-push.at-right{right:-20pc}#at-drawer.drawer-push.at-left{left:-20pc}#at-drawer .at-recommended-label{padding:0 0 0 20px;color:#999;line-height:3pc;font-size:18px;font-weight:300;cursor:default}#at-drawer-arrow{width:30px;height:5pc}#at-drawer-arrow.ats-dark{background:#262b30}#at-drawer-arrow.ats-gray{background:#f2f2f2}#at-drawer-open-arrow{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA0AAABcCAYAAAC1OT8uAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChNYWNpbnRvc2gpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjk3ODNCQjdERUQ3QjExRTM5NjFGRUZBODc3MTIwMTNCIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjk3ODNCQjdFRUQ3QjExRTM5NjFGRUZBODc3MTIwMTNCIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6OTc4M0JCN0JFRDdCMTFFMzk2MUZFRkE4NzcxMjAxM0IiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6OTc4M0JCN0NFRDdCMTFFMzk2MUZFRkE4NzcxMjAxM0IiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7kstzCAAAB4ElEQVR42uyWv0oDQRDGb9dYimgVjliID2Ca9AGfwtZob2Grja1PIFj7EhGCYK99VPBPOkVMp8X5rc6FeN7dfjOksMjAxwXZ3667OzvfLKRr682l5ZV9aDh+fxsnRHhoDzqGLjFBi4XOoFtoAxowoB893o/w7WpAl/+QgQMBwwRdTPhUC2lAV/wDA7qy5WOgq9psHejqTqkKdLE7KYCv0JZjMgBgB58raBG6mP1K6j2pT099T+qMUOeeOss1wDcEIA1PnQXy576rAUI0oFMoC7VCnn40Gs8Pd4lAiXNUKmJ0lh1mPzGEWiyUCqAGW3Pwv4IvUJsFO9CHgP3Zr6Te0xwgAf3LxaAjS241pbikCRkOg+nSJdV4p8HOPl3vvRYI5dtrgVDvvcWovcWovcWovcWovcWovcWovQChWNywNpqvdAKtQp/QNmPUIQ6kwwqt2Xmsxf6GMPM1Pptsbz45CPmXqKb+15Gz4J/LZcDSNIqBlQlbB0afe1mmUDWiCNKFZRq0VKMeXY1CTDq2sJLWsCmoaBBRqNRR6qBKC6qCaj2rDIqaXBGiXHEaom00h1S+K3fVlr6HNuqgvgCh0+owt21bybQn8+mZ78mcEebcM2e5+T2ZX24ZqCph0qn1vgQYAJ/KDpLQr2tPAAAAAElFTkSuQmCC);background-repeat:no-repeat;width:13px;height:23px;margin:28px 0 0 8px}.at-left #at-drawer-open-arrow{background-position:0 -46px}.ats-dark #at-drawer-open-arrow{background-position:0 -23px}.ats-dark.at-left #at-drawer-open-arrow{background-position:0 -69px}#at-drawer-arrow.at4-drawer-modern-browsers{position:fixed;top:40%;background-repeat:no-repeat;background-position:0 0!important;z-index:9999999}.at4-drawer-inline #at-drawer-arrow{position:absolute}#at-drawer-arrow.at4-drawer-modern-browsers.at-right{right:0}#at-drawer-arrow.at4-drawer-modern-browsers.at-left{left:0}.at4-drawer-push-animation-left{transition:left .4s ease-in-out .15s}.at4-drawer-push-animation-right{transition:right .4s ease-in-out .15s}#at-drawer.drawer-push.at4-drawer-push-animation-right{right:0}#at-drawer.drawer-push.at4-drawer-push-animation-right-back{right:-20pc!important}#at-drawer.drawer-push.at4-drawer-push-animation-left{left:0}#at-drawer.drawer-push.at4-drawer-push-animation-left-back{left:-20pc!important}#at-drawer .at4-closebutton.drawer-close{content:'X';color:#999;display:block;position:absolute;margin:0;top:0;right:0;width:3pc;height:45px;line-height:45px;overflow:hidden;opacity:.5}#at-drawer.ats-dark .at4-closebutton.drawer-close{color:#fff}#at-drawer .at4-closebutton.drawer-close:hover{opacity:1}#at-drawer.ats-dark.at4-recommended .at4-logo-container a{color:#666}#at-drawer.at4-recommended .at4-recommended-vertical{padding:0}#at-drawer.at4-recommended .at4-recommended-item .sponsored-label{margin:2px 0 0 21px;color:#ddd}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item{position:relative;padding:0;width:20pc;height:180px;margin:0}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img a:after{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,.65);z-index:1000000;transition:all .2s ease-in-out}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item.at-hover .at4-recommended-item-img a:after{background:rgba(0,0,0,.8)}#at-drawer .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img,#at-drawer .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img a,#at-drawer .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img img{width:20pc;height:180px;float:none}#at-drawer .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption{width:100%;position:absolute;bottom:0;left:0;height:70px}#at-drawer .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption .at-h4{color:#fff;position:absolute;height:52px;top:0;left:20px;right:20px;margin:0;padding:0;line-height:25px;font-size:20px;font-weight:600;z-index:1000001;text-decoration:none;text-transform:none}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption .at-h4 a:hover{text-decoration:none}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption .at-h4 a:link{color:#fff}#at-drawer.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption small{position:absolute;top:auto;bottom:10px;left:20px;width:auto;color:#ccc}#at-drawer.at4-recommended .at4-logo-container{margin-left:20px}#at-drawer.ats-dark.at4-recommended .at4-logo-container a:hover{color:#fff}#at-drawer.at4-recommended .at-logo{margin:0}
      </style>
      <style type="text/css">
       .at4-follow.at-mobile{display:none!important}.at4-follow{position:fixed;top:0;right:0;font-weight:400;color:#666;cursor:default;z-index:10001}.at4-follow .at4-follow-inner{position:relative;padding:10px 24px 10px 15px}.at4-follow-inner,.at-follow-open-control{border:0 solid #c5c5c5;border-width:1px 0 1px 1px;margin-top:-1px}.at4-follow .at4-follow-container{margin-left:9pt}.at4-follow.at4-follow-24 .at4-follow-container{height:24px;line-height:23px;font-size:13px}.at4-follow.at4-follow-32 .at4-follow-container{width:15pc;height:2pc;line-height:2pc;font-size:14px}.at4-follow .at4-follow-container .at-follow-label{display:inline-block;height:24px;line-height:24px;margin-right:10px;padding:0;cursor:default;float:left}.at4-follow .at4-follow-container .at-icon-wrapper{height:24px;width:24px}.at4-follow.ats-transparent .at4-follow-inner,.at-follow-open-control.ats-transparent{border-color:transparent}.at4-follow.ats-dark .at4-follow-inner,.at-follow-open-control.ats-dark{background:#262b30;border-color:#000;color:#fff}.at4-follow.ats-dark .at-follow-close-control{background-color:#262b30}.at4-follow.ats-light .at4-follow-inner{background:#fff;border-color:#c5c5c5}.at4-follow.ats-gray .at4-follow-inner,.at-follow-open-control.ats-gray{background:#f2f2f2;border-color:#c5c5c5}.at4-follow.ats-light .at4-follow-close-control,.at-follow-open-control.ats-light{background:#e5e5e5}.at4-follow .at4-follow-inner .at4-follow-close-control{position:absolute;top:0;bottom:0;left:0;width:20px;cursor:pointer;display:none}.at4-follow .at4-follow-inner .at4-follow-close-control div{display:block;line-height:20px;text-indent:-9999em;margin-top:calc(50% + 1px);overflow:hidden}.at-follow-open-control div.at4-arrow.at-left{background-position:0 -2px}.at-follow-open-control{position:fixed;height:35px;top:0;right:0;padding-top:10px;z-index:10002}.at-follow-btn{margin:0 5px 5px 0;padding:0;outline-offset:-1px;display:inline-block;box-sizing:content-box;transition:all .2s ease-in-out}.at-follow-btn:focus,.at-follow-btn:hover{transform:translateY(-4px)}.at4-follow-24 .at-follow-btn{height:25px;line-height:0;width:25px}
      </style>
      <style type="text/css">
       .at-follow-tbx-element .at300b,.at-follow-tbx-element .at300m{display:inline-block;width:auto;padding:0;margin:0 2px 5px;outline-offset:-1px;transition:all .2s ease-in-out}.at-follow-tbx-element .at300b:focus,.at-follow-tbx-element .at300b:hover,.at-follow-tbx-element .at300m:focus,.at-follow-tbx-element .at300m:hover{transform:translateY(-4px)}.at-follow-tbx-element .addthis_vertical_style .at300b,.at-follow-tbx-element .addthis_vertical_style .at300m{display:block}.at-follow-tbx-element .addthis_vertical_style .at300b .addthis_follow_label,.at-follow-tbx-element .addthis_vertical_style .at300b .at-icon-wrapper,.at-follow-tbx-element .addthis_vertical_style .at300m .addthis_follow_label,.at-follow-tbx-element .addthis_vertical_style .at300m .at-icon-wrapper{display:inline-block;vertical-align:middle;margin-right:5px}.at-follow-tbx-element .addthis_vertical_style .at300b:focus,.at-follow-tbx-element .addthis_vertical_style .at300b:hover,.at-follow-tbx-element .addthis_vertical_style .at300m:focus,.at-follow-tbx-element .addthis_vertical_style .at300m:hover{transform:none}
      </style>
      <style type="text/css">
       .at4-jumboshare .at-share-btn{display:inline-block;margin-right:13px;margin-top:13px}.at4-jumboshare .at-share-btn .at-icon{float:left}.at4-jumboshare .at-share-btn .at300bs{display:inline-block;float:left;cursor:pointer}.at4-jumboshare .at4-mobile .at-share-btn .at-icon,.at4-jumboshare .at4-mobile .at-share-btn .at-icon-wrapper{margin:0;padding:0}.at4-jumboshare .at4-mobile .at-share-btn{padding:0}.at4-jumboshare .at4-mobile .at-share-btn .at-label{display:none}.at4-jumboshare .at4-count{font-size:60px;line-height:60px;font-family:Helvetica neue,arial;font-weight:700}.at4-jumboshare .at4-count-container{display:table-cell;text-align:center;min-width:200px;vertical-align:middle;border-right:1px solid #ccc;padding-right:20px}.at4-jumboshare .at4-share-container{display:table-cell;vertical-align:middle;padding-left:20px}.at4-jumboshare .at4-share-container.at-share-tbx-element{padding-top:0}.at4-jumboshare .at4-title{position:relative;font-size:18px;line-height:18px;bottom:2px}.at4-jumboshare .at4-spacer{height:1px;display:block;visibility:hidden;opacity:0}.at4-jumboshare .at-share-btn{display:inline-block;margin:0 2px;line-height:0;padding:0;overflow:hidden;text-decoration:none;text-transform:none;color:#fff;cursor:pointer;transition:all .2s ease-in-out;border:0;background-color:transparent}.at4-jumboshare .at-share-btn:focus,.at4-jumboshare .at-share-btn:hover{transform:translateY(-4px);color:#fff;text-decoration:none}.at4-jumboshare .at-label{font-family:helvetica neue,helvetica,arial,sans-serif;font-size:9pt;padding:0 15px 0 0;margin:0;height:2pc;line-height:2pc;background:none}.at4-jumboshare .at-share-btn:hover,.at4-jumboshare .at-share-btn:link{text-decoration:none}.at4-jumboshare .at-share-btn::-moz-focus-inner{border:0;padding:0}.at4-jumboshare.at-mobile .at-label{display:none}
      </style>
      <style type="text/css">
       .at4-recommendedbox-outer-container{display:inline}.at4-recommended-outer{position:static}.at4-recommended{top:20%;margin:0;text-align:center;font-weight:400;font-size:13px;line-height:17px;color:#666}.at4-recommended.at-inline .at4-recommended-horizontal{text-align:left}.at4-recommended-recommendedbox{padding:0;z-index:inherit}.at4-recommended-recommended{padding:40px 0}.at4-recommended-horizontal{max-height:340px}.at4-recommended.at-medium .at4-recommended-horizontal{max-height:15pc}.at4-recommended.at4-minimal.at-medium .at4-recommended-horizontal{padding-top:10px;max-height:230px}.at4-recommended-text-only .at4-recommended-horizontal{max-height:130px}.at4-recommended-horizontal{padding-top:5px;overflow-y:hidden}.at4-minimal{background:none;color:#000;border:none!important;box-shadow:none!important}@media screen and (max-width:900px){.at4-recommended-horizontal .at4-recommended-item,.at4-recommended-horizontal .at4-recommended-item .at4-recommended-item-img{width:15pc}}.at4-recommended.at4-minimal .at4-recommended-horizontal .at4-recommended-item .at4-recommended-item-caption{padding:0 0 10px}.at4-recommended.at4-minimal .at4-recommended-horizontal .at4-recommended-item-caption{padding:20px 0 0!important}.addthis-smartlayers .at4-recommended .at-h3.at-recommended-label{margin:0;padding:0;font-weight:300;font-size:18px;line-height:24px;color:#464646;width:100%;display:inline-block;zoom:1}.addthis-smartlayers .at4-recommended.at-inline .at-h3.at-recommended-label{text-align:left}#at4-thankyou .addthis-smartlayers .at4-recommended.at-inline .at-h3.at-recommended-label{text-align:center}.at4-recommended .at4-recommended-item{display:inline-block;zoom:1;position:relative;background:#fff;border:1px solid #c5c5c5;width:200px;margin:10px}.addthis_recommended_horizontal .at4-recommended-item{border:none}.at4-recommended .at4-recommended-item .sponsored-label{color:#666;font-size:9px;position:absolute;top:-20px}.at4-recommended .at4-recommended-item-img .at-tli,.at4-recommended .at4-recommended-item-img a{position:absolute;left:0}.at4-recommended.at-inline .at4-recommended-horizontal .at4-recommended-item{margin:10px 20px 0 0}.at4-recommended.at-medium .at4-recommended-horizontal .at4-recommended-item{margin:10px 10px 0 0}.at4-recommended.at-medium .at4-recommended-item{width:140px;overflow:hidden}.at4-recommended .at4-recommended-item .at4-recommended-item-img{position:relative;text-align:center;width:100%;height:200px;line-height:0;overflow:hidden}.at4-recommended .at4-recommended-item .at4-recommended-item-img a{display:block;width:100%;height:200px}.at4-recommended.at-medium .at4-recommended-item .at4-recommended-item-img,.at4-recommended.at-medium .at4-recommended-item .at4-recommended-item-img a{height:140px}.at4-recommended .at4-recommended-item .at4-recommended-item-img img{position:absolute;top:0;left:0;min-height:0;min-width:0;max-height:none;max-width:none;margin:0;padding:0}.at4-recommended .at4-recommended-item .at4-recommended-item-caption{height:74px;overflow:hidden;padding:20px;text-align:left;-ms-box-sizing:content-box;-o-box-sizing:content-box;box-sizing:content-box}.at4-recommended.at-medium .at4-recommended-item .at4-recommended-item-caption{height:50px;padding:15px}.at4-recommended .at4-recommended-item .at4-recommended-item-caption .at-h4{height:54px;margin:0 0 5px;padding:0;overflow:hidden;word-wrap:break-word;font-size:14px;font-weight:400;line-height:18px;text-align:left}.at4-recommended.at-medium .at4-recommended-item .at4-recommended-item-caption .at-h4{font-size:9pt;line-height:1pc;height:33px}.at4-recommended .at4-recommended-item:hover .at4-recommended-item-caption .at-h4{text-decoration:underline}.at4-recommended a:link,.at4-recommended a:visited{text-decoration:none;color:#464646}.at4-recommended .at4-recommended-item .at4-recommended-item-caption .at-h4 a:hover{text-decoration:underline;color:#000}.at4-recommended .at4-recommended-item .at4-recommended-item-caption small{display:block;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;font-size:11px;color:#666}.at4-recommended.at-medium .at4-recommended-item .at4-recommended-item-caption small{font-size:9px}.at4-recommended .at4-recommended-vertical{padding:15px 0 0}.at4-recommended .at4-recommended-vertical .at4-recommended-item{display:block;width:auto;max-width:100%;height:60px;border:none;margin:0 0 15px;box-shadow:none;background:none}.at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img,.at4-recommended-vertical .at4-recommended-item .at4-recommended-item-img img{width:60px;height:60px;float:left}.at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption{border-top:none;margin:0;height:60px;padding:3px 5px}.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption .at-h4{height:38px;margin:0}.at4-recommended .at4-recommended-vertical .at4-recommended-item .at4-recommended-item-caption small{position:absolute;bottom:0}.at4-recommended .at-recommended-label.at-vertical{text-align:left}.at4-no-image-light-recommended,.at4-no-image-minimal-recommended{background-color:#f2f2f2!important}.at4-no-image-gray-recommended{background-color:#e6e6e5!important}.at4-no-image-dark-recommended{background-color:#4e555e!important}.at4-recommended .at4-recommended-item-placeholder-img{background-repeat:no-repeat!important;background-position:center!important;width:100%!important;height:100%!important}.at4-recommended-horizontal .at4-no-image-dark-recommended .at4-recommended-item-placeholder-img{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACIAAAAfCAYAAACCox+xAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChNYWNpbnRvc2gpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjlFNUUyQTg3MTI0RDExRTM4NzAwREJDRjlCQzAyMUVFIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjlFNUUyQTg4MTI0RDExRTM4NzAwREJDRjlCQzAyMUVFIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6OUU1RTJBODUxMjREMTFFMzg3MDBEQkNGOUJDMDIxRUUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6OUU1RTJBODYxMjREMTFFMzg3MDBEQkNGOUJDMDIxRUUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6oCfPiAAABfUlEQVR42uyWTU/DMAyGm3bdBxp062hHe+PC//9HCIkDYpNAO7CPAuWN5Eohyhpno2GHWqq8pO78xHHsiLquH4L/l6cwuBAZaOPKs//YBFIJIR59UiAt7huYi90aE/UQakTDLaL26RUEAAJqiefm93T9Bpj1X4O0bY0OIUXCpYBJvYDAUWyAUCWliHGTcnpqRMaM72ImRAJVknYG+eb4YEDIBeU0zGnsBLK1ODogYSsLhDwIJeVVk18lzfNA4ERGZNXi59UCIQhiYDilpSm/jp4awLxDvWhlf4/nGe8+LLuSt+SZul28ggaHG6gNVhDR+IuRFzOoxGKWwG7vVFm5AAQxgcqYpzrjFjR9zwPH5LSuT7XlNr2MQm5LzqjLpncNNaM+s8M27Y60g3FwhoSMzrtUQllgLtRs5pZ2cB4IhbvQbGRZv1NsrhyS8+SI5Mo9RJWpjAI1xqKL+0iEP180vy214JbeR12AyOgsHI7e0NfFyKv0ID1ID+IqPwIMAOeljGQOryBmAAAAAElFTkSuQmCC)!important}.at4-recommended-vertical .at4-no-image-dark-recommended .at4-recommended-item-placeholder-img{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAOCAYAAADwikbvAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChNYWNpbnRvc2gpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjAzREMyNTM2MTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjAzREMyNTM3MTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MDNEQzI1MzQxMjRGMTFFMzg3MDBEQkNGOUJDMDIxRUUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MDNEQzI1MzUxMjRGMTFFMzg3MDBEQkNGOUJDMDIxRUUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz5GfbtkAAAAxklEQVR42qRSTQvCMAxduk53mEOHKFPP/v8/5cGTiIibivVFUomlG7gFHvloXpKmJefcPhkmNyvGEWj+IOZA6ckPImoxxVwOLvCvXUzkpayNCpRQK64IbOBnAYGAXMeMslNlU+CzrIEdCkxi5DPAoz6BE8ZuVNdKJuL8rS9sv62IXlCHyP0KqKUKZXK9uwkSLVArfwpVR3b225kXwovibcP+jC4jUtfWPZmfqJJnYlkAM128j1z0nHWKSUbIKDL/msHktwADAPptQo+vkZNLAAAAAElFTkSuQmCC)!important}.at4-recommended-horizontal .at4-no-image-gray-recommended .at4-recommended-item-placeholder-img,.at4-recommended-horizontal .at4-no-image-light-recommended .at4-recommended-item-placeholder-img,.at4-recommended-horizontal .at4-no-image-minimal-recommended .at4-recommended-item-placeholder-img{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACIAAAAfCAYAAACCox+xAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChNYWNpbnRvc2gpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjAzREMyNTMyMTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjAzREMyNTMzMTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6OUU1RTJBODkxMjREMTFFMzg3MDBEQkNGOUJDMDIxRUUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6OUU1RTJBOEExMjREMTFFMzg3MDBEQkNGOUJDMDIxRUUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz6dfDQvAAABg0lEQVR42uyWS0vDQBDH82jaKNW0qUltbl68e/Di98eLBz+CCB5EBaWIpUat/4UJLMuame1j7SEDYbqbKfPLvHbDi8ur8+D/5T4K9kR6xrr27D+xgdS3N9d3PilQFmcNzN6mxkbdhxrQcoGofXkFAUAINcVzrG2vsP8KmJdtg7SlxoRQouBywOReQOAosUDoklPEpEU5XDciqeB/iRAig6pIO4P8CHysBBDqg0palrR2Alkwjj5RsDUDoRqhorpq6quifRkInKiIPLf4eWIgQoLoWbq0stXXn10DmDeoR2PsL/E84N0Hk5Wypc70dMkGGhzOoeb4gpjW34K6GEFljFkGu6XTZJUCEMQBVCHs6kI60MycB47FyUmo20oPvYJCzhVnvIsR3zg5ghoRTNpyHKTBBhIJTt6pFsoZ9iLDZswcB5uBULhnho0a66eazaFDca59Hym1e4guQ4rCO4Fu/T4Sw8Gk+c3MghN6H+8CRKVg4tB6fV8XI6/SgXQgHYir/AowAMU5TskhKVUNAAAAAElFTkSuQmCC)!important}.at4-recommended-vertical .at4-no-image-gray-recommended .at4-recommended-item-placeholder-img,.at4-recommended-vertical .at4-no-image-light-recommended .at4-recommended-item-placeholder-img,.at4-recommended-vertical .at4-no-image-minimal-recommended .at4-recommended-item-placeholder-img{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAOCAYAAADwikbvAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDE0IDc5LjE1MTQ4MSwgMjAxMy8wMy8xMy0xMjowOToxNSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChNYWNpbnRvc2gpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOjAzREMyNTNBMTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOjAzREMyNTNCMTI0RjExRTM4NzAwREJDRjlCQzAyMUVFIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6MDNEQzI1MzgxMjRGMTFFMzg3MDBEQkNGOUJDMDIxRUUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6MDNEQzI1MzkxMjRGMTFFMzg3MDBEQkNGOUJDMDIxRUUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz65Fr9cAAAA0ElEQVR42qRRuQrCQBDd3SSaIgYNosSrtLew8f+xsfAnYmEVRMR4YHwjExjCbsBk4DHHzptjR2+2u7VqJ3efjTNQ/EEMgbgiv46H/QNTDPnhCv/mYiLPI21EIIaaUEVgBj+oETQQypgRtidsXfNJpsACBXo28gWgUd9AjrEL0TXhiSh/XhWudlZI/kCdLPtFUGMRCni9p6kl+kAq/D5UavmzX2fNd87obsCSfztnrOR0rjvTiRImkoyAQQNRyZ2jhjenGNVBOpF1WZatyV8BBgBJ+irgS/KHdAAAAABJRU5ErkJggg==)!important}#at-drawer.ats-dark,.at4-recommended.ats-dark .at4-recommended-horizontal .at4-recommended-item-caption,.at4-recommended.ats-dark .at4-recommended-vertical .at4-recommended-item-caption{background:#262b30}#at-drawer.ats-gray,.at4-recommended.ats-gray .at4-recommended-horizontal .at4-recommended-item-caption{background:#f2f2f2}#at-drawer.ats-light,.at4-recommended.ats-light .at4-recommended-horizontal .at4-recommended-item-caption{background:#fff}.at4-recommended.ats-dark .at4-recommended-vertical .at4-recommended-item{background:none}.at4-recommended.ats-dark .at4-recommended-item .at4-recommended-item-caption a:hover,.at4-recommended.ats-dark .at4-recommended-item .at4-recommended-item-caption a:link,.at4-recommended.ats-dark .at4-recommended-item .at4-recommended-item-caption a:visited,.at4-recommended.ats-dark .at4-recommended-item .at4-recommended-item-caption small,.at4-recommended.ats-dark .at4-recommended-item-caption,.at4-recommended.ats-dark .at-logo a:hover,.at4-recommended.ats-dark .at-recommended-label.at-vertical{color:#fff}.at4-recommended-vertical-logo{padding-top:0;text-align:left}.at4-recommended-vertical-logo .at4-logo-container{line-height:10px}.at4-recommended-horizontal-logo{text-align:center}.at4-recommended.at-inline .at4-recommended-horizontal-logo{text-align:left}#at4-thankyou .at4-recommended.at-inline .at4-recommended-horizontal{text-align:center}.at4-recommended .at-logo{margin:10px 0 0;padding:0;height:25px;overflow:auto;-ms-box-sizing:content-box;-o-box-sizing:content-box;box-sizing:content-box}.at4-recommended.at-inline .at4-recommended-horizontal .at-logo{text-align:left}.at4-recommended .at4-logo-container a.at-sponsored-link{color:#666}.at4-recommended-class .at4-logo-container a:hover,.at4-recommendedbox-outer-container .at4-recommended-recommendedbox .at4-logo-container a:hover{color:#000}
      </style>
      <style type="text/css">
       .at-recommendedjumbo-outer-container{margin:0;padding:0;border:0;background:none;color:#000}.at-recommendedjumbo-footer{position:relative;width:100%;height:510px;overflow:hidden;transition:all .3s ease-in-out}.at-mobile .at-recommendedjumbo-footer{height:250px}.at-recommendedjumbo-footer #bg-link:after{content:'';position:absolute;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,.75)}.at-recommendedjumbo-footer:hover #bg-link:after{background:rgba(0,0,0,.85)}.at-recommendedjumbo-footer *,.at-recommendedjumbo-footer :after,.at-recommendedjumbo-footer :before{box-sizing:border-box}.at-recommendedjumbo-footer:hover #at-recommendedjumbo-footer-bg{animation:atRecommendedJumboAnimatedBackground 1s ease-in-out 1;animation-fill-mode:forwards}.at-recommendedjumbo-footer #at-recommendedjumbo-top-holder{position:absolute;top:0;padding:0 40px;width:100%}.at-mobile .at-recommendedjumbo-footer #at-recommendedjumbo-top-holder{padding:0 20px}.at-recommendedjumbo-footer .at-recommendedjumbo-footer-inner{position:relative;text-align:center;font-family:helvetica,arial,sans-serif;z-index:2;width:100%}.at-recommendedjumbo-footer #at-recommendedjumbo-label-holder{margin:40px 0 0;max-height:30px}.at-mobile .at-recommendedjumbo-footer #at-recommendedjumbo-label-holder{margin:20px 0 0;max-height:20px}.at-recommendedjumbo-footer #at-recommendedjumbo-label{font-weight:300;font-size:24px;line-height:24px;color:#fff;margin:0}.at-mobile .at-recommendedjumbo-footer #at-recommendedjumbo-label{font-weight:150;font-size:14px;line-height:14px}.at-recommendedjumbo-footer #at-recommendedjumbo-title-holder{margin:20px 0 0;min-height:3pc;max-height:78pt}.at-mobile .at-recommendedjumbo-footer #at-recommendedjumbo-title-holder{margin:10px 0 0;min-height:24px;max-height:54px}.at-recommendedjumbo-footer #at-recommendedjumbo-content-title{font-size:3pc;line-height:52px;font-weight:700;margin:0}.at-mobile .at-recommendedjumbo-footer #at-recommendedjumbo-content-title{font-size:24px;line-height:27px}.at-recommendedjumbo-footer a{text-decoration:none;color:#fff}.at-recommendedjumbo-footer a:visited{color:#fff}.at-recommendedjumbo-footer small{margin:20px 0 0;display:inline-block;height:2pc;line-height:2pc;font-size:14px;color:#ccc;cursor:default}.at-mobile .at-recommendedjumbo-footer small{margin:10px 0 0;height:14px;line-height:14px;font-size:9pt}.at-recommendedjumbo-footer .at-logo-container{position:absolute;bottom:20px;margin:auto;left:0;right:0}.at-mobile .at-recommendedjumbo-footer .at-logo-container{bottom:10px}.at-recommendedjumbo-footer a.at-sponsored-link{color:#ccc}.at-recommendedjumbo-footer div #at-recommendedjumbo-logo-link{padding:2px 0 0 11px;text-decoration:none;line-height:20px;font-family:helvetica,arial,sans-serif;font-size:9px;color:#ccc}@keyframes atRecommendedJumboAnimatedBackground{0%{transform:scale(1,1)}to{transform:scale(1.1,1.1)}}
      </style>
      <style type="text/css">
       .at-resp-share-element{position:relative;padding:0;margin:0;font-size:0;line-height:0}.at-resp-share-element:after,.at-resp-share-element:before{content:" ";display:table}.at-resp-share-element.at-mobile .at4-share-count-container,.at-resp-share-element.at-mobile .at-label{display:none}.at-resp-share-element .at-share-btn{display:inline-block;*display:inline;*zoom:1;margin:0 2px 5px;padding:0;overflow:hidden;line-height:0;text-decoration:none;text-transform:none;color:#fff;cursor:pointer;transition:all .2s ease-in-out;border:0;font-family:helvetica neue,helvetica,arial,sans-serif;background-color:transparent}.at-resp-share-element .at-share-btn::-moz-focus-inner{border:0;padding:0}.at-resp-share-element .at-share-btn:focus,.at-resp-share-element .at-share-btn:hover{transform:translateY(-4px);color:#fff;text-decoration:none}.at-resp-share-element .at-share-btn .at-icon-wrapper{float:left}.at-resp-share-element .at-share-btn.at-share-btn.at-svc-compact:hover{transform:none}.at-resp-share-element .at-share-btn .at-label{font-family:helvetica neue,helvetica,arial,sans-serif;font-size:9pt;padding:0 15px 0 0;margin:0 0 0 5px;height:2pc;line-height:2pc;background:none}.at-resp-share-element .at-icon,.at-resp-share-element .at-label{cursor:pointer}.at-resp-share-element .at4-share-count-container{text-decoration:none;float:right;padding-right:15px;font-size:9pt}.at-mobile .at-resp-share-element .at-label{display:none}.at-resp-share-element.at-mobile .at-share-btn{margin-right:5px}.at-mobile .at-resp-share-element .at-share-btn{padding:5px;margin-right:5px}
      </style>
      <style type="text/css">
       .at-share-tbx-element{position:relative;margin:0;color:#fff;font-size:0}.at-share-tbx-element,.at-share-tbx-element .at-share-btn{font-family:helvetica neue,helvetica,arial,sans-serif;padding:0;line-height:0}.at-share-tbx-element .at-share-btn{cursor:pointer;margin:0 5px 5px 0;display:inline-block;overflow:hidden;border:0;text-decoration:none;text-transform:none;background-color:transparent;color:inherit;transition:all .2s ease-in-out}.at-share-tbx-element .at-share-btn:focus,.at-share-tbx-element .at-share-btn:hover{transform:translateY(-4px);outline-offset:-1px;color:inherit}.at-share-tbx-element .at-share-btn::-moz-focus-inner{border:0;padding:0}.at-share-tbx-element .at-share-btn.at-share-btn.at-svc-compact:hover{transform:none}.at-share-tbx-element .at-icon-wrapper{vertical-align:middle}.at-share-tbx-element .at4-share-count,.at-share-tbx-element .at-label{margin:0 7.5px 0 2.5px;text-decoration:none;vertical-align:middle;display:inline-block;background:none;height:0;font-size:inherit;line-height:inherit;color:inherit}.at-share-tbx-element.at-mobile .at4-share-count,.at-share-tbx-element.at-mobile .at-label{display:none}.at-share-tbx-element .at_native_button{vertical-align:middle}.at-share-tbx-element .addthis_counter.addthis_bubble_style{margin:0 2px;vertical-align:middle;display:inline-block}.at-share-tbx-element .fb_iframe_widget{display:block}.at-share-tbx-element.at-share-tbx-native .at300b{vertical-align:middle}.at-style-responsive .at-share-btn{padding:5px}.at-style-jumbo{display:table}.at-style-jumbo .at4-spacer{height:1px;display:block;visibility:hidden;opacity:0}.at-style-jumbo .at4-count-container{display:table-cell;text-align:center;min-width:200px;vertical-align:middle;border-right:1px solid #ccc;padding-right:20px}.at-style-jumbo .at4-count{font-size:60px;line-height:60px;font-weight:700}.at-style-jumbo .at4-count-title{position:relative;font-size:18px;line-height:18px;bottom:2px}.at-style-jumbo .at-share-btn-elements{display:table-cell;vertical-align:middle;padding-left:20px}.at_flat_counter{cursor:pointer;font-family:helvetica,arial,sans-serif;font-weight:700;text-transform:uppercase;display:inline-block;position:relative;vertical-align:top;height:auto;margin:0 5px;padding:0 6px;left:-1px;background:#ebebeb;color:#32363b;transition:all .2s ease}.at_flat_counter:after{top:30%;left:-4px;content:"";position:absolute;border-width:5px 8px 5px 0;border-style:solid;border-color:transparent #ebebeb transparent transparent;display:block;width:0;height:0;transform:translateY(360deg)}.at_flat_counter:hover{background:#e1e2e2}
      </style>
      <style type="text/css">
       .at4-thankyou-background{top:0;right:0;left:0;bottom:0;-webkit-overflow-scrolling:touch;z-index:9999999;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABtJREFUeNpizCuu/sRABGBiIBKMKqSOQoAAAwC8KgJipENhxwAAAABJRU5ErkJggg==);background:hsla(217,6%,46%,.95)}.at4-thankyou-background.at-thankyou-shown{position:fixed}.at4-thankyou-inner{position:absolute;width:100%;top:10%;left:50%;margin-left:-50%;text-align:center}.at4-thankyou-mobile .at4-thankyou-inner{top:5%}.thankyou-description{font-weight:400}.at4-thankyou-background .at4lb-inner{position:relative;width:100%;height:100%}.at4-thankyou-background .at4lb-inner .at4x{position:absolute;top:15px;right:15px;display:block;width:20px;height:20px;padding:20px;margin:0;cursor:pointer;transition:opacity .25s ease-in;opacity:.4;background:url("data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAABx0RVh0U29mdHdhcmUAQWRvYmUgRmlyZXdvcmtzIENTNui8sowAAAAWdEVYdENyZWF0aW9uIFRpbWUAMTEvMTMvMTKswDp5AAAAd0lEQVQ4jb2VQRLAIAgDE///Z3qqY1FAhalHMCsCIkVEAIAkkVgvp2lDBgYAnAyHkWotLccNrEd4A7X2TqIdqLfnWBAdaF5rJdyJfjtPH5GT37CaGhoVq3nOm/XflUuLUto2pY1d+vRKh0Pp+MrAVtDe2JkvYNQ+jVSEEFmOkggAAAAASUVORK5CYII=") no-repeat center center;overflow:hidden;text-indent:-99999em;border:1px solid transparent}.at4-thankyou-background .at4lb-inner .at4x:focus,.at4-thankyou-background .at4lb-inner .at4x:hover{border:1px solid #fff;border-radius:50%;outline:0}.at4-thankyou-background .at4lb-inner #at4-palogo{position:absolute;bottom:10px;display:inline-block;text-decoration:none;font-family:helvetica,arial,sans-serif;font-size:11px;cursor:pointer;-webkit-transition:opacity .25s ease-in;moz-transition:opacity .25s ease-in;transition:opacity .25s ease-in;opacity:.5;z-index:100020;color:#fff;padding:2px 0 0 13px}.at4-thankyou-background .at4lb-inner #at4-palogo .at-branding-addthis,.at4-thankyou-background .at4lb-inner #at4-palogo .at-branding-info{color:#fff}.at4-thankyou-background .at4lb-inner #at4-palogo:hover,.at4-thankyou-background.ats-dark .at4lb-inner a#at4-palogo:hover{text-decoration:none;color:#fff;opacity:1}.at4-thankyou-background.ats-dark{background-image:url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAoAAAAKCAYAAACNMs+9AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAABtJREFUeNpiZGBgeMZABGBiIBKMKqSOQoAAAwB+cQD6hqlbCwAAAABJRU5ErkJggg==");background:rgba(0,0,0,.85)}.at4-thankyou-background .thankyou-title{color:#fff;font-size:38.5px;margin:10px 20px;line-height:38.5px;font-family:helvetica neue,helvetica,arial,sans-serif;font-weight:300}.at4-thankyou-background.ats-dark .thankyou-description,.at4-thankyou-background.ats-dark .thankyou-title{color:#fff}.at4-thankyou-background .thankyou-description{color:#fff;font-size:18px;margin:10px 0;line-height:24px;padding:0;font-family:helvetica neue,helvetica,arial,sans-serif;font-weight:300}.at4-thankyou-background .at4-thanks-icons{padding-top:10px}.at4-thankyou-mobile *{-webkit-overflow-scrolling:touch}#at4-thankyou .at4-recommended-recommendedbox .at-logo{display:none}.at4-thankyou .at-h3{height:49px;line-height:49px;margin:0 50px 0 20px;padding:1px 0 0;font-family:helvetica neue,helvetica,arial,sans-serif;font-size:1pc;font-weight:700;color:#fff;text-shadow:0 1px #000}.at4-thanks{padding-top:50px;text-align:center}.at4-thanks label{display:block;margin:0 0 15px;font-size:1pc;line-height:1pc}.at4-thanks .at4-h2{background:none;border:none;margin:0 0 10px;padding:0;font-family:helvetica neue,helvetica,arial,sans-serif;font-size:28px;font-weight:300;color:#000}.at4-thanks .at4-thanks-icons{position:relative;height:2pc}.at4-thanks .at4-thanks-icons .at-thankyou-label{display:block;padding-bottom:10px;font-size:14px;color:#666}.at4-thankyou-layer .at-follow .at-icon-wrapper{width:2pc;height:2pc}
      </style>
      <style type="text/css">
       .at4-recommended-toaster{position:fixed;top:auto;bottom:0;right:0;z-index:100021}.at4-recommended-toaster.ats-light{border:1px solid #c5c5c5;background:#fff}.at4-recommended-toaster.ats-gray{border:1px solid #c5c5c5;background:#f2f2f2}.at4-recommended-toaster.ats-dark{background:#262b30;color:#fff}.at4-recommended-toaster .at4-recommended-container{padding-top:0;margin:0}.at4-recommended.at4-recommended-toaster div.at-recommended-label{line-height:1pc;font-size:1pc;text-align:left;padding:20px 0 0 20px}.at4-toaster-outer .at4-recommended .at4-recommended-item .at4-recommended-item-caption .at-h4{font-size:11px;line-height:11px;margin:10px 0 6px;height:30px}.at4-recommended.at4-recommended-toaster div.at-recommended-label.ats-gray,.at4-recommended.at4-recommended-toaster div.at-recommended-label.ats-light{color:#464646}.at4-recommended.at4-recommended-toaster div.at-recommended-label.ats-dark{color:#fff}.at4-toaster-close-control{position:absolute;top:0;right:0;display:block;width:20px;height:20px;line-height:20px;margin:5px 5px 0 0;padding:0;text-indent:-9999em}.at4-toaster-open-control{position:fixed;right:0;bottom:0;z-index:100020}.at4-toaster-outer .at4-recommended-item{width:90pt;border:0;margin:9px 10px 0}.at4-toaster-outer .at4-recommended-item:first-child{margin-left:20px}.at4-toaster-outer .at4-recommended-item:last-child{margin-right:20px}.at4-toaster-outer .at4-recommended-item .at4-recommended-item-img{max-height:90pt;max-width:90pt}.at4-toaster-outer .at4-recommended-item .at4-recommended-item-img img{height:90pt;width:90pt}.at4-toaster-outer .at4-recommended-item .at4-recommended-item-caption{height:30px;padding:0;margin:0;height:initial}.at4-toaster-outer .ats-dark .at4-recommended-item .at4-recommended-item-caption{background:#262b30}.at4-toaster-outer .at4-recommended .at4-recommended-item .at4-recommended-item-caption small{width:auto;line-height:14px;margin:0}.at4-toaster-outer .at4-recommended.ats-dark .at4-recommended-item .at4-recommended-item-caption small{color:#fff}.at4-recommended-toaster .at-logo{margin:0 0 3px 20px;text-align:left}.at4-recommended-toaster .at-logo .at4-logo-container.at-sponsored-logo{position:relative}.at4-toaster-outer .at4-recommended-item .sponsored-label{text-align:right;font-size:10px;color:#666;float:right;position:fixed;bottom:6px;right:20px;top:initial;z-index:99999}
      </style>
      <style type="text/css">
       .at4-whatsnext{position:fixed;bottom:0!important;right:0;background:#fff;border:1px solid #c5c5c5;margin:-1px;width:390px;height:90pt;overflow:hidden;font-size:9pt;font-weight:400;color:#000;z-index:1800000000}.at4-whatsnext a{color:#666}.at4-whatsnext .at-whatsnext-content{height:90pt;position:relative}.at4-whatsnext .at-whatsnext-content .at-branding{position:absolute;bottom:15px;right:10px;padding-left:9px;text-decoration:none;line-height:10px;font-family:helvetica,arial,sans-serif;font-size:10px;color:#666}.at4-whatsnext .at-whatsnext-content .at-whatsnext-content-inner{position:absolute;top:15px;right:20px;bottom:15px;left:140px;text-align:left;height:105px}.at4-whatsnext .at-whatsnext-content-inner a{display:inline-block}.at4-whatsnext .at-whatsnext-content-inner div.at-h6{text-align:left;margin:0;padding:0 0 3px;font-size:11px;color:#666;cursor:default}.at4-whatsnext .at-whatsnext-content .at-h3{text-align:left;margin:5px 0;padding:0;line-height:1.2em;font-weight:400;font-size:14px;height:3pc}.at4-whatsnext .at-whatsnext-content-inner a:link,.at4-whatsnext .at-whatsnext-content-inner a:visited{text-decoration:none;font-weight:400;color:#464646}.at4-whatsnext .at-whatsnext-content-inner a:hover{color:#000}.at4-whatsnext .at-whatsnext-content-inner small{position:absolute;bottom:15px;line-height:10px;font-size:11px;color:#666;cursor:default;text-align:left}.at4-whatsnext .at-whatsnext-content .at-whatsnext-content-img{position:absolute;top:0;left:0;width:90pt;height:90pt;overflow:hidden}.at4-whatsnext .at-whatsnext-content .at-whatsnext-content-img img{position:absolute;top:0;left:0;max-height:none;max-width:none}.at4-whatsnext .at-whatsnext-close-control{position:absolute;top:0;right:0;display:block;width:20px;height:20px;line-height:20px;margin:0 5px 0 0;padding:0;text-indent:-9999em}.at-whatsnext-open-control{position:fixed;right:0;bottom:0;z-index:100020}.at4-whatsnext.ats-dark{background:#262b30}.at4-whatsnext.ats-dark .at-whatsnext-content .at-h3,.at4-whatsnext.ats-dark .at-whatsnext-content a.at4-logo:hover,.at4-whatsnext.ats-dark .at-whatsnext-content-inner a:link,.at4-whatsnext.ats-dark .at-whatsnext-content-inner a:visited{color:#fff}.at4-whatsnext.ats-light{background:#fff}.at4-whatsnext.ats-gray{background:#f2f2f2}.at4-whatsnext.at-whatsnext-nophoto{width:270px}.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content-img{display:none}.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content .at-whatsnext-content-inner{top:15px;right:0;left:20px}.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content .at-whatsnext-content-inner.addthis_32x32_style{top:0;right:0;left:0;padding:45px 20px 0;font-size:20px}.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content .at-whatsnext-content-inner .at4-icon,.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content .at-whatsnext-content-inner .at4-icon-fw,.at4-whatsnext.at-whatsnext-nophoto .at-whatsnext-content .at-whatsnext-content-inner .whatsnext-msg{vertical-align:middle}.at-whatsnext-img,.at-whatsnext-img-lnk{position:absolute;left:0}
      </style>
      <style type="text/css">
       .at4-whatsnextmobile{position:fixed;bottom:0;right:0;left:0;background:#fff;z-index:9999998;height:170px;font-size:28px}.at4-whatsnextmobile .col-2{height:100%;font-size:1em}.at4-whatsnextmobile .col-2:first-child{max-width:200px;display:inline-block;float:left}.at4-whatsnextmobile .col-2:last-child{position:absolute;left:200px;right:50px;top:0;bottom:0;display:inline-block}.at4-whatsnextmobile .at-whatsnext-content-inner{font-size:1em}.at4-whatsnextmobile .at-whatsnext-content-img img{height:100%;width:100%}.at4-whatsnextmobile .at-close-control{font-size:1em;position:absolute;top:0;right:0;width:50px;height:50px}.at4-whatsnextmobile .at-close-control button{width:100%;height:100%;font-size:1em;font-weight:400;text-decoration:none;opacity:.5;padding:0;cursor:pointer;background:0 0;border:0;-webkit-appearance:none}.at4-whatsnextmobile .at-h3,.at4-whatsnextmobile .at-h6{font-size:1em;margin:0;color:#a1a1a1;margin-left:2.5%;margin-top:25px}.at4-whatsnextmobile .at-h3{font-size:1em;line-height:1em;font-weight:500;height:50%}.at4-whatsnextmobile .at-h3 a{font-size:1em;text-decoration:none}.at4-whatsnextmobile .at-h6{font-size:.8em;line-height:.8em;font-weight:500}.at4-whatsnextmobile .footer{position:absolute;bottom:2px;left:200px;right:0;padding-left:2.5%;font-size:1em;line-height:.6em}.at4-whatsnextmobile .footer small{font-size:.6em;color:#a1a1a1}.at4-whatsnextmobile .footer small:first-child{margin-right:5%;float:left}.at4-whatsnextmobile .footer small:last-child{margin-right:2.5%;float:right}.at4-whatsnextmobile .at-whatsnext-content{height:100%}.at4-whatsnextmobile.ats-dark{background:#262b30;color:#fff}.at4-whatsnextmobile .at-close-control button{color:#bfbfbf}.at4-whatsnextmobile.ats-dark a:link,.at4-whatsnextmobile.ats-dark a:visited{color:#fff}.at4-whatsnextmobile.ats-gray{background:#f2f2f2;color:#262b30}.at4-whatsnextmobile.ats-light{background:#fff;color:#262b30}.at4-whatsnextmobile.ats-dark .footer a:link,.at4-whatsnextmobile.ats-dark .footer a:visited,.at4-whatsnextmobile.ats-gray .footer a:link,.at4-whatsnextmobile.ats-gray .footer a:visited,.at4-whatsnextmobile.ats-light .footer a:link,.at4-whatsnextmobile.ats-light .footer a:visited{color:#a1a1a1}.at4-whatsnextmobile.ats-gray a:link,.at4-whatsnextmobile.ats-gray a:visited,.at4-whatsnextmobile.ats-light a:link,.at4-whatsnextmobile.ats-light a:visited{color:#262b30}@media only screen and (min-device-width:320px) and (max-device-width:480px){.at4-whatsnextmobile{height:85px;font-size:14px}.at4-whatsnextmobile .col-2:first-child{width:75pt}.at4-whatsnextmobile .col-2:last-child{right:25px;left:75pt}.at4-whatsnextmobile .footer{left:75pt}.at4-whatsnextmobile .at-close-control{width:25px;height:25px}.at4-whatsnextmobile .at-h3,.at4-whatsnextmobile .at-h6{margin-top:12.5px}}
      </style>
      <style type="text/css">
       .at-custom-mobile-bar{left:0;right:0;width:100%;height:56px;position:fixed;text-align:center;z-index:100020;background:#fff;overflow:hidden;box-shadow:0 0 10px 0 rgba(0,0,0,.2);font:initial;line-height:normal;top:auto;bottom:0}.at-custom-mobile-bar.at-custom-mobile-bar-zindex-hide{z-index:-1!important}.at-custom-mobile-bar.atss-top{top:0;bottom:auto}.at-custom-mobile-bar.atss-bottom{top:auto;bottom:0}.at-custom-mobile-bar .at-custom-mobile-bar-btns{display:inline-block;text-align:center}.at-custom-mobile-bar .at-custom-mobile-bar-counter,.at-custom-mobile-bar .at-share-btn{margin-top:4px}.at-custom-mobile-bar .at-share-btn{display:inline-block;text-decoration:none;transition:none;box-sizing:content-box}.at-custom-mobile-bar .at-custom-mobile-bar-counter{font-family:Helvetica neue,arial;vertical-align:top;margin-left:4px;margin-right:4px;display:inline-block}.at-custom-mobile-bar .at-custom-mobile-bar-count{font-size:26px;line-height:1.25em;color:#222}.at-custom-mobile-bar .at-custom-mobile-bar-text{font-size:9pt;line-height:1.25em;color:#888;letter-spacing:1px}.at-custom-mobile-bar .at-icon-wrapper{text-align:center;height:3pc;width:3pc;margin:0 4px}.at-custom-mobile-bar .at-icon{vertical-align:top;margin:8px;width:2pc;height:2pc}.at-custom-mobile-bar.at-shfs-medium{height:3pc}.at-custom-mobile-bar.at-shfs-medium .at-custom-mobile-bar-counter{margin-top:6px}.at-custom-mobile-bar.at-shfs-medium .at-custom-mobile-bar-count{font-size:18px}.at-custom-mobile-bar.at-shfs-medium .at-custom-mobile-bar-text{font-size:10px}.at-custom-mobile-bar.at-shfs-medium .at-icon-wrapper{height:40px;width:40px}.at-custom-mobile-bar.at-shfs-medium .at-icon{margin:6px;width:28px;height:28px}.at-custom-mobile-bar.at-shfs-small{height:40px}.at-custom-mobile-bar.at-shfs-small .at-custom-mobile-bar-counter{margin-top:3px}.at-custom-mobile-bar.at-shfs-small .at-custom-mobile-bar-count{font-size:1pc}.at-custom-mobile-bar.at-shfs-small .at-custom-mobile-bar-text{font-size:10px}.at-custom-mobile-bar.at-shfs-small .at-icon-wrapper{height:2pc;width:2pc}.at-custom-mobile-bar.at-shfs-small .at-icon{margin:4px;width:24px;height:24px}
      </style>
      <style type="text/css">
       .at-custom-sidebar{top:20%;width:58px;position:fixed;text-align:center;z-index:100020;background:#fff;overflow:hidden;box-shadow:0 0 10px 0 rgba(0,0,0,.2);font:initial;line-height:normal;top:auto;bottom:0}.at-custom-sidebar.at-custom-sidebar-zindex-hide{z-index:-1!important}.at-custom-sidebar.atss-left{left:0;right:auto;float:left;border-radius:0 4px 4px 0}.at-custom-sidebar.atss-right{left:auto;right:0;float:right;border-radius:4px 0 0 4px}.at-custom-sidebar .at-custom-sidebar-btns{display:inline-block;text-align:center;padding-top:4px}.at-custom-sidebar .at-custom-sidebar-counter{margin-bottom:8px}.at-custom-sidebar .at-share-btn{display:inline-block;text-decoration:none;transition:none;box-sizing:content-box}.at-custom-sidebar .at-custom-sidebar-counter{font-family:Helvetica neue,arial;vertical-align:top;margin-left:4px;margin-right:4px;display:inline-block}.at-custom-sidebar .at-custom-sidebar-count{font-size:21px;line-height:1.25em;color:#222}.at-custom-sidebar .at-custom-sidebar-text{font-size:10px;line-height:1.25em;color:#888;letter-spacing:1px}.at-custom-sidebar .at-icon-wrapper{text-align:center;margin:0 4px}.at-custom-sidebar .at-icon{vertical-align:top;margin:9px;width:2pc;height:2pc}.at-custom-sidebar .at-icon-wrapper{position:relative}.at-custom-sidebar .at4-share-count,.at-custom-sidebar .at4-share-count-container{line-height:1pc;font-size:10px}.at-custom-sidebar .at4-share-count{text-indent:0;font-family:Arial,Helvetica Neue,Helvetica,sans-serif;font-weight:200;width:100%;height:1pc}.at-custom-sidebar .at4-share-count-anchor .at-icon{margin-top:3px}.at-custom-sidebar .at4-share-count-container{position:absolute;left:0;right:auto;top:auto;bottom:0;width:100%;color:#fff;background:inherit}
      </style>
      <style type="text/css">
       .at-image-sharing-mobile-icon{position:absolute;background:#000 url(//s7.addthis.com/static/44a36d35bafef33aa9455b7d3039a771.png) no-repeat top center;background-color:rgba(0,0,0,.9);background-image:url(//s7.addthis.com/static/10db525181ee0bbe1a515001be1c7818.svg),none;border-radius:3px;width:50px;height:40px;top:-9999px;left:-9999px}.at-image-sharing-tool{display:block;position:absolute;text-align:center;z-index:9001;background:none;overflow:hidden;top:-9999px;left:-9999px;font:initial;line-height:0}.at-image-sharing-tool.addthis-animated{animation-duration:.15s}.at-image-sharing-tool.at-orientation-vertical .at-share-btn{display:block}.at-image-sharing-tool.at-orientation-horizontal .at-share-btn{display:inline-block}.at-image-sharing-tool.at-image-sharing-tool-size-big .at-icon{width:43px;height:43px}.at-image-sharing-tool.at-image-sharing-tool-size-mobile .at-share-btn{margin:0!important}.at-image-sharing-tool.at-image-sharing-tool-size-mobile .at-icon-wrapper{height:60px;width:100%;border-radius:0!important}.at-image-sharing-tool.at-image-sharing-tool-size-mobile .at-icon{max-width:100%;height:54px!important;width:54px!important}.at-image-sharing-tool .at-custom-shape.at-image-sharing-tool-btns{margin-right:8px;margin-bottom:8px}.at-image-sharing-tool .at-custom-shape .at-share-btn{margin-top:8px;margin-left:8px}.at-image-sharing-tool .at-share-btn{line-height:0;text-decoration:none;transition:none;box-sizing:content-box}.at-image-sharing-tool .at-icon-wrapper{text-align:center;height:100%;width:100%}.at-image-sharing-tool .at-icon{vertical-align:top;width:2pc;height:2pc;margin:3px}
      </style>
      <style type="text/css">
       .at-expanding-share-button{box-sizing:border-box;position:fixed;z-index:9999}.at-expanding-share-button[data-position=bottom-right]{bottom:10px;right:10px}.at-expanding-share-button[data-position=bottom-right] .at-expanding-share-button-toggle-bg,.at-expanding-share-button[data-position=bottom-right] .at-expanding-share-button-toggle-btn[data-name]:after,.at-expanding-share-button[data-position=bottom-right] .at-icon-wrapper,.at-expanding-share-button[data-position=bottom-right] [data-name]:after{float:right}.at-expanding-share-button[data-position=bottom-right] [data-name]:after{margin-right:10px}.at-expanding-share-button[data-position=bottom-right] .at-expanding-share-button-toggle-btn[data-name]:after{margin-right:5px}.at-expanding-share-button[data-position=bottom-right] .at-icon-wrapper{margin-right:-3px}.at-expanding-share-button[data-position=bottom-left]{bottom:10px;left:10px}.at-expanding-share-button[data-position=bottom-left] .at-expanding-share-button-toggle-bg,.at-expanding-share-button[data-position=bottom-left] .at-expanding-share-button-toggle-btn[data-name]:after,.at-expanding-share-button[data-position=bottom-left] .at-icon-wrapper,.at-expanding-share-button[data-position=bottom-left] [data-name]:after{float:left}.at-expanding-share-button[data-position=bottom-left] [data-name]:after{margin-left:10px}.at-expanding-share-button[data-position=bottom-left] .at-expanding-share-button-toggle-btn[data-name]:after{margin-left:5px}.at-expanding-share-button *,.at-expanding-share-button :after,.at-expanding-share-button :before{box-sizing:border-box}.at-expanding-share-button .at-expanding-share-button-services-list{display:none;list-style:none;margin:0 5px;overflow:visible;padding:0}.at-expanding-share-button .at-expanding-share-button-services-list&gt;li{display:block;height:45px;position:relative;overflow:visible}.at-expanding-share-button .at-expanding-share-button-toggle-btn,.at-expanding-share-button .at-share-btn{transition:.1s;text-decoration:none}.at-expanding-share-button .at-share-btn{display:block;height:40px;padding:0 3px 0 0}.at-expanding-share-button .at-expanding-share-button-toggle-btn{position:relative;overflow:auto}.at-expanding-share-button .at-expanding-share-button-toggle-btn.at-expanding-share-button-hidden[data-name]:after{display:none}.at-expanding-share-button .at-expanding-share-button-toggle-bg{box-shadow:0 2px 4px 0 rgba(0,0,0,.3);border-radius:50%;position:relative}.at-expanding-share-button .at-expanding-share-button-toggle-bg&gt;span{background-image:url("data:image/svg+xml,%3Csvg%20width%3D%2232px%22%20height%3D%2232px%22%20viewBox%3D%220%200%2032%2032%22%20version%3D%221.1%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Ctitle%3Eshare%3C%2Ftitle%3E%3Cg%20stroke%3D%22none%22%20stroke-width%3D%221%22%20fill%3D%22none%22%20fill-rule%3D%22evenodd%22%3E%3Cg%20fill%3D%22%23FFFFFF%22%3E%3Cpath%20d%3D%22M26%2C13.4285714%20C26%2C13.6220248%2025.9293162%2C13.7894338%2025.7879464%2C13.9308036%20L20.0736607%2C19.6450893%20C19.932291%2C19.786459%2019.7648819%2C19.8571429%2019.5714286%2C19.8571429%20C19.3779752%2C19.8571429%2019.2105662%2C19.786459%2019.0691964%2C19.6450893%20C18.9278267%2C19.5037195%2018.8571429%2C19.3363105%2018.8571429%2C19.1428571%20L18.8571429%2C16.2857143%20L16.3571429%2C16.2857143%20C15.6279725%2C16.2857143%2014.9750773%2C16.3080355%2014.3984375%2C16.3526786%20C13.8217977%2C16.3973217%2013.2488868%2C16.477306%2012.6796875%2C16.5926339%20C12.1104882%2C16.7079619%2011.6157015%2C16.8660704%2011.1953125%2C17.0669643%20C10.7749235%2C17.2678581%2010.3824423%2C17.5264121%2010.0178571%2C17.8426339%20C9.65327199%2C18.1588557%209.35565592%2C18.534596%209.125%2C18.9698661%20C8.89434408%2C19.4051361%208.71391434%2C19.9203839%208.58370536%2C20.515625%20C8.45349637%2C21.1108661%208.38839286%2C21.7842224%208.38839286%2C22.5357143%20C8.38839286%2C22.9449425%208.40699386%2C23.4025272%208.44419643%2C23.9084821%20C8.44419643%2C23.9531252%208.45349693%2C24.0405499%208.47209821%2C24.1707589%20C8.4906995%2C24.3009679%208.5%2C24.3995532%208.5%2C24.4665179%20C8.5%2C24.5781256%208.46837829%2C24.6711306%208.40513393%2C24.7455357%20C8.34188956%2C24.8199408%208.25446484%2C24.8571429%208.14285714%2C24.8571429%20C8.02380893%2C24.8571429%207.9196433%2C24.7938994%207.83035714%2C24.6674107%20C7.77827355%2C24.6004461%207.72991094%2C24.5186017%207.68526786%2C24.421875%20C7.64062478%2C24.3251483%207.59040206%2C24.2135423%207.53459821%2C24.0870536%20C7.47879436%2C23.9605648%207.43973225%2C23.87128%207.41741071%2C23.8191964%20C6.47246551%2C21.6986501%206%2C20.0208395%206%2C18.7857143%20C6%2C17.3050521%206.19717065%2C16.0662252%206.59151786%2C15.0691964%20C7.79688103%2C12.0706695%2011.0520568%2C10.5714286%2016.3571429%2C10.5714286%20L18.8571429%2C10.5714286%20L18.8571429%2C7.71428571%20C18.8571429%2C7.52083237%2018.9278267%2C7.35342333%2019.0691964%2C7.21205357%20C19.2105662%2C7.07068382%2019.3779752%2C7%2019.5714286%2C7%20C19.7648819%2C7%2019.932291%2C7.07068382%2020.0736607%2C7.21205357%20L25.7879464%2C12.9263393%20C25.9293162%2C13.067709%2026%2C13.2351181%2026%2C13.4285714%20L26%2C13.4285714%20Z%22%3E%3C%2Fpath%3E%3C%2Fg%3E%3C%2Fg%3E%3C%2Fsvg%3E");background-position:center center;background-repeat:no-repeat;transition:transform .4s ease;border-radius:50%;display:block}.at-expanding-share-button .at-icon-wrapper{box-shadow:0 2px 4px 0 rgba(0,0,0,.3);border-radius:50%;display:inline-block;height:40px;line-height:40px;text-align:center;width:40px}.at-expanding-share-button .at-icon{display:inline-block;height:34px;margin:3px 0;vertical-align:top;width:34px}.at-expanding-share-button [data-name]:after{box-shadow:0 2px 4px 0 rgba(0,0,0,.3);transform:translate(0, -50%);transition:.4s;background-color:#fff;border-radius:3px;color:#666;content:attr(data-name);font-family:Helvetica Neue,Helvetica,Arial,sans-serif;font-size:9pt;line-height:9pt;font-weight:500;opacity:0;padding:3px 5px;position:relative;top:20px;white-space:nowrap}.at-expanding-share-button.at-expanding-share-button-show-icons .at-expanding-share-button-services-list{display:block}.at-expanding-share-button.at-expanding-share-button-animate-in .at-expanding-share-button-toggle-bg&gt;span{transform:rotate(270deg);background-image:url("data:image/svg+xml,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20viewBox%3D%220%200%2032%2032%22%3E%3Cg%3E%3Cpath%20d%3D%22M18%2014V8h-4v6H8v4h6v6h4v-6h6v-4h-6z%22%20fill-rule%3D%22evenodd%22%20fill%3D%22white%22%3E%3C%2Fpath%3E%3C%2Fg%3E%3C%2Fsvg%3E");background-position:center center;background-repeat:no-repeat}.at-expanding-share-button.at-expanding-share-button-animate-in [data-name]:after{opacity:1}.at-expanding-share-button.at-hide-label [data-name]:after{display:none}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle{height:50px}.at-expanding-share-button.at-expanding-share-button-desktop .at-icon-wrapper:hover{box-shadow:0 2px 5px 0 rgba(0,0,0,.5)}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle-bg{height:50px;line-height:50px;width:50px}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle-bg&gt;span{height:50px;width:50px}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle-bg:after{box-shadow:0 2px 5px 0 rgba(0,0,0,.2);transition:opacity .2s ease;border-radius:50%;content:'';height:100%;opacity:0;position:absolute;top:0;left:0;width:100%}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle-bg:hover:after{opacity:1}.at-expanding-share-button.at-expanding-share-button-desktop .at-expanding-share-button-toggle-btn[data-name]:after{top:25px}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-services-list{margin:0}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-toggle-btn,.at-expanding-share-button.at-expanding-share-button-mobile .at-share-btn{outline:0}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-toggle{height:40px;-webkit-tap-highlight-color:transparent}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-toggle-bg,.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-toggle-bg span{height:40px;line-height:40px;width:40px}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-click-flash{transform:scale(0);transition:transform ease,opacity ease-in;background-color:hsla(0,0%,100%,.3);border-radius:50%;height:40px;opacity:1;position:absolute;width:40px;z-index:10000}.at-expanding-share-button.at-expanding-share-button-mobile .at-expanding-share-button-click-flash.at-expanding-share-button-click-flash-animate{transform:scale(1);opacity:0}.at-expanding-share-button.at-expanding-share-button-mobile+.at-expanding-share-button-mobile-overlay{transition:opacity ease;bottom:0;background-color:hsla(0,0%,87%,.7);display:block;height:auto;left:0;opacity:0;position:fixed;right:0;top:0;width:auto;z-index:9998}.at-expanding-share-button.at-expanding-share-button-mobile+.at-expanding-share-button-mobile-overlay.at-expanding-share-button-hidden{height:0;width:0;z-index:-10000}.at-expanding-share-button.at-expanding-share-button-mobile.at-expanding-share-button-animate-in+.at-expanding-share-button-mobile-overlay{transition:opacity ease;opacity:1}
      </style>
      <style type="text/css">
       .at-tjin-element .at300b,.at-tjin-element .at300m{display:inline-block;width:auto;padding:0;margin:0 2px 5px;outline-offset:-1px;transition:all .2s ease-in-out}.at-tjin-element .at300b:focus,.at-tjin-element .at300b:hover,.at-tjin-element .at300m:focus,.at-tjin-element .at300m:hover{transform:translateY(-4px)}.at-tjin-element .addthis_tjin_label{display:none}.at-tjin-element .addthis_vertical_style .at300b,.at-tjin-element .addthis_vertical_style .at300m{display:block}.at-tjin-element .addthis_vertical_style .at300b .addthis_tjin_label,.at-tjin-element .addthis_vertical_style .at300b .at-icon-wrapper,.at-tjin-element .addthis_vertical_style .at300m .addthis_tjin_label,.at-tjin-element .addthis_vertical_style .at300m .at-icon-wrapper{display:inline-block;vertical-align:middle;margin-right:5px}.at-tjin-element .addthis_vertical_style .at300b:focus,.at-tjin-element .addthis_vertical_style .at300b:hover,.at-tjin-element .addthis_vertical_style .at300m:focus,.at-tjin-element .addthis_vertical_style .at300m:hover{transform:none}.at-tjin-element .at-tjin-btn{margin:0 5px 5px 0;padding:0;outline-offset:-1px;display:inline-block;box-sizing:content-box;transition:all .2s ease-in-out}.at-tjin-element .at-tjin-btn:focus,.at-tjin-element .at-tjin-btn:hover{transform:translateY(-4px)}.at-tjin-element .at-tjin-title{margin:0 0 15px}
      </style>
      <style type="text/css">
       #addthissmartlayerscssready{color:#bada55!important}.addthis-smartlayers,div#at4-follow,div#at4-share,div#at4-thankyou,div#at4-whatsnext{padding:0;margin:0}#at4-follow-label,#at4-share-label,#at4-whatsnext-label,.at4-recommended-label.hidden{padding:0;border:none;background:none;position:absolute;top:0;left:0;height:0;width:0;overflow:hidden;text-indent:-9999em}.addthis-smartlayers .at4-arrow:hover{cursor:pointer}.addthis-smartlayers .at4-arrow:after,.addthis-smartlayers .at4-arrow:before{content:none}a.at4-logo{background:url(data:image/gif;base64,R0lGODlhBwAHAJEAAP9uQf///wAAAAAAACH5BAkKAAIALAAAAAAHAAcAAAILFH6Ge8EBH2MKiQIAOw==) no-repeat left center}.at4-minimal a.at4-logo{background:url(data:image/gif;base64,R0lGODlhBwAHAJEAAP9uQf///wAAAAAAACH5BAkKAAIALAAAAAAHAAcAAAILFH6Ge8EBH2MKiQIAOw==) no-repeat left center!important}button.at4-closebutton{position:absolute;top:0;right:0;padding:0;margin-right:10px;cursor:pointer;background:transparent;border:0;-webkit-appearance:none;font-size:19px;line-height:1;color:#000;text-shadow:0 1px 0 #fff;opacity:.2}button.at4-closebutton:hover{color:#000;text-decoration:none;cursor:pointer;opacity:.5}div.at4-arrow{background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAFAAAAAoCAYAAABpYH0BAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAV1JREFUeNrsmesOgyAMhQfxwfrofTM3E10ME2i5Oeppwr9a5OMUCrh1XV+wcvNAAIAA+BiAzrmtUWln27dbjEcC3AdODfo0BdEPhmcO4nIDvDNELi2jggk4/k8dT7skfeKzWIEd4VUpMQKvNB7X+OZSmAZkATWC1xvipbpnLmOosbJZC08CkAeA4E6qFUEMwLAGnlSBPCE8lW8CYnZTcimH2HoT7kSFOx5HBmCnDhTIu1p5s98G+QZrxGPhZVMY1vgyAQaAAAiAAAgDQACcBOD+BvJtBWfRy7NpJK5tBe4FNzXokywV734wPHMQlxvgnSGyNoUP/2ACjv/7iSeYKO3YWKzAjvCqlBiBVxqPa3ynexNJwOsN8TJbzL6JNIYYXWpMv4lIIAZgWANPqkCeEJ7KNwExu8lpLlSpAVQarO77TyKdBsyRPuwV0h0gmoGnTWFYzVkYBoAA+I/2FmAAt6+b5XM9mFkAAAAASUVORK5CYII=);background-repeat:no-repeat;width:20px;height:20px;margin:0;padding:0;overflow:hidden;text-indent:-9999em;text-align:left;cursor:pointer}#at4-recommendedpanel-outer-container .at4-arrow.at-right,div.at4-arrow.at-right{background-position:-20px 0}#at4-recommendedpanel-outer-container .at4-arrow.at-left,div.at4-arrow.at-left{background-position:0 0}div.at4-arrow.at-down{background-position:-60px 0}div.at4-arrow.at-up{background-position:-40px 0}.ats-dark div.at4-arrow.at-right{background-position:-20px -20px}.ats-dark div.at4-arrow.at-left{background-position:0 -20px}.ats-dark div.at4-arrow.at-down{background-position:-60px -20px}.ats-dark div.at4-arrow.at-up{background-position:-40px -20}.at4-opacity-hidden{opacity:0!important}.at4-opacity-visible{opacity:1!important}.at4-visually-hidden{position:absolute;clip:rect(1px,1px,1px,1px);padding:0;border:0;overflow:hidden}.at4-hidden-off-screen,.at4-hidden-off-screen *{position:absolute!important;top:-9999px!important;left:-9999px!important}.at4-show{display:block!important;opacity:1!important}.at4-show-content{opacity:1!important;visibility:visible}.at4-hide{display:none!important;opacity:0!important}.at4-hide-content{opacity:0!important;visibility:hidden}.at4-visible{display:block!important;opacity:0!important}.at-wordpress-hide{display:none!important;opacity:0!important}.addthis-animated{animation-fill-mode:both;animation-timing-function:ease-out;animation-duration:.3s}.slideInDown.addthis-animated,.slideInLeft.addthis-animated,.slideInRight.addthis-animated,.slideInUp.addthis-animated,.slideOutDown.addthis-animated,.slideOutLeft.addthis-animated,.slideOutRight.addthis-animated,.slideOutUp.addthis-animated{animation-duration:.4s}@keyframes fadeIn{0%{opacity:0}to{opacity:1}}.fadeIn{animation-name:fadeIn}@keyframes fadeInUp{0%{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}.fadeInUp{animation-name:fadeInUp}@keyframes fadeInDown{0%{opacity:0;transform:translateY(-20px)}to{opacity:1;transform:translateY(0)}}.fadeInDown{animation-name:fadeInDown}@keyframes fadeInLeft{0%{opacity:0;transform:translateX(-20px)}to{opacity:1;transform:translateX(0)}}.fadeInLeft{animation-name:fadeInLeft}@keyframes fadeInRight{0%{opacity:0;transform:translateX(20px)}to{opacity:1;transform:translateX(0)}}.fadeInRight{animation-name:fadeInRight}@keyframes fadeOut{0%{opacity:1}to{opacity:0}}.fadeOut{animation-name:fadeOut}@keyframes fadeOutUp{0%{opacity:1;transform:translateY(0)}to{opacity:0;transform:translateY(-20px)}}.fadeOutUp{animation-name:fadeOutUp}@keyframes fadeOutDown{0%{opacity:1;transform:translateY(0)}to{opacity:0;transform:translateY(20px)}}.fadeOutDown{animation-name:fadeOutDown}@keyframes fadeOutLeft{0%{opacity:1;transform:translateX(0)}to{opacity:0;transform:translateX(-20px)}}.fadeOutLeft{animation-name:fadeOutLeft}@keyframes fadeOutRight{0%{opacity:1;transform:translateX(0)}to{opacity:0;transform:translateX(20px)}}.fadeOutRight{animation-name:fadeOutRight}@keyframes slideInUp{0%{transform:translateY(1500px)}0%,to{opacity:1}to{transform:translateY(0)}}.slideInUp{animation-name:slideInUp}.slideInUp.addthis-animated{animation-duration:.4s}@keyframes slideInDown{0%{transform:translateY(-850px)}0%,to{opacity:1}to{transform:translateY(0)}}.slideInDown{animation-name:slideInDown}@keyframes slideOutUp{0%{transform:translateY(0)}0%,to{opacity:1}to{transform:translateY(-250px)}}.slideOutUp{animation-name:slideOutUp}@keyframes slideOutUpFast{0%{transform:translateY(0)}0%,to{opacity:1}to{transform:translateY(-1250px)}}#at4m-menu.slideOutUp{animation-name:slideOutUpFast}@keyframes slideOutDown{0%{transform:translateY(0)}0%,to{opacity:1}to{transform:translateY(350px)}}.slideOutDown{animation-name:slideOutDown}@keyframes slideOutDownFast{0%{transform:translateY(0)}0%,to{opacity:1}to{transform:translateY(1250px)}}#at4m-menu.slideOutDown{animation-name:slideOutDownFast}@keyframes slideInLeft{0%{opacity:0;transform:translateX(-850px)}to{transform:translateX(0)}}.slideInLeft{animation-name:slideInLeft}@keyframes slideInRight{0%{opacity:0;transform:translateX(1250px)}to{transform:translateX(0)}}.slideInRight{animation-name:slideInRight}@keyframes slideOutLeft{0%{transform:translateX(0)}to{opacity:0;transform:translateX(-350px)}}.slideOutLeft{animation-name:slideOutLeft}@keyframes slideOutRight{0%{transform:translateX(0)}to{opacity:0;transform:translateX(350px)}}.slideOutRight{animation-name:slideOutRight}.at4win{margin:0 auto;background:#fff;border:1px solid #ebeced;width:25pc;box-shadow:0 0 10px rgba(0,0,0,.3);border-radius:8px;font-family:helvetica neue,helvetica,arial,sans-serif;text-align:left;z-index:9999}.at4win .at4win-header{position:relative;border-bottom:1px solid #f2f2f2;background:#fff;height:49px;-webkit-border-top-left-radius:8px;-webkit-border-top-right-radius:8px;-moz-border-radius-topleft:8px;-moz-border-radius-topright:8px;border-top-left-radius:8px;border-top-right-radius:8px;cursor:default}.at4win .at4win-header .at-h3,.at4win .at4win-header h3{height:49px;line-height:49px;margin:0 50px 0 0;padding:1px 0 0;margin-left:20px;font-family:helvetica neue,helvetica,arial,sans-serif;font-size:1pc;font-weight:700;text-shadow:0 1px #fff;color:#333}.at4win .at4win-header .at-h3 img,.at4win .at4win-header h3 img{display:inline-block;margin-right:4px}.at4win .at4win-header .at4-close{display:block;position:absolute;top:0;right:0;background:url("data:image/gif;base64,R0lGODlhFAAUAIABAAAAAP///yH5BAEAAAEALAAAAAAUABQAAAIzBIKpG+YMm5Enpodw1HlCfnkKOIqU1VXk55goVb2hi7Y0q95lfG70uurNaqLgTviyyUoFADs=") no-repeat center center;background-repeat:no-repeat;background-position:center center;border-left:1px solid #d2d2d1;width:49px;height:49px;line-height:49px;overflow:hidden;text-indent:-9999px;text-shadow:none;cursor:pointer;opacity:.5;border:0;transition:opacity .15s ease-in}.at4win .at4win-header .at4-close::-moz-focus-inner{border:0;padding:0}.at4win .at4win-header .at4-close:hover{opacity:1;background-color:#ebeced;border-top-right-radius:7px}.at4win .at4win-content{position:relative;background:#fff;min-height:220px}#at4win-footer{position:relative;background:#fff;border-top:1px solid #d2d2d1;-webkit-border-bottom-right-radius:8px;-webkit-border-bottom-left-radius:8px;-moz-border-radius-bottomright:8px;-moz-border-radius-bottomleft:8px;border-bottom-right-radius:8px;border-bottom-left-radius:8px;height:11px;line-height:11px;padding:5px 20px;font-size:11px;color:#666;-ms-box-sizing:content-box;-o-box-sizing:content-box;box-sizing:content-box}#at4win-footer a{margin-right:10px;text-decoration:none;color:#666}#at4win-footer a:hover{text-decoration:none;color:#000}#at4win-footer a.at4-logo{top:5px;padding-left:10px}#at4win-footer a.at4-privacy{position:absolute;top:5px;right:10px;padding-right:14px}.at4win.ats-dark{border-color:#555;box-shadow:none}.at4win.ats-dark .at4win-header{background:#1b1b1b;-webkit-border-top-left-radius:6px;-webkit-border-top-right-radius:6px;-moz-border-radius-topleft:6px;-moz-border-radius-topright:6px;border-top-left-radius:6px;border-top-right-radius:6px}.at4win.ats-dark .at4win-header .at4-close{background:url("data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0NAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAALEgAACxIB0t1+/AAAABx0RVh0U29mdHdhcmUAQWRvYmUgRmlyZXdvcmtzIENTNui8sowAAAAWdEVYdENyZWF0aW9uIFRpbWUAMTEvMTMvMTKswDp5AAAAd0lEQVQ4jb2VQRLAIAgDE///Z3qqY1FAhalHMCsCIkVEAIAkkVgvp2lDBgYAnAyHkWotLccNrEd4A7X2TqIdqLfnWBAdaF5rJdyJfjtPH5GT37CaGhoVq3nOm/XflUuLUto2pY1d+vRKh0Pp+MrAVtDe2JkvYNQ+jVSEEFmOkggAAAAASUVORK5CYII=") no-repeat center center;background-image:url(//s7.addthis.com/static/fb08f6d50887bd0caacc86a62bcdcf68.svg),none;border-color:#333}.at4win.ats-dark .at4win-header .at4-close:hover{background-color:#000}.at4win.ats-dark .at4win-header .at-h3,.at4win.ats-dark .at4win-header h3{color:#fff;text-shadow:0 1px #000}.at4win.ats-gray .at4win-header{background:#fff;border-color:#d2d2d1;-webkit-border-top-left-radius:6px;-webkit-border-top-right-radius:6px;-moz-border-radius-topleft:6px;-moz-border-radius-topright:6px;border-top-left-radius:6px;border-top-right-radius:6px}.at4win.ats-gray .at4win-header a.at4-close{border-color:#d2d2d1}.at4win.ats-gray .at4win-header a.at4-close:hover{background-color:#ebeced}.at4win.ats-gray #at4win-footer{border-color:#ebeced}.at4win .clear{clear:both}.at4win ::selection{background:#fe6d4c;color:#fff}.at4win ::-moz-selection{background:#fe6d4c;color:#fff}.at4-icon-fw{display:inline-block;background-repeat:no-repeat;background-position:0 0;margin:0 5px 0 0;overflow:hidden;text-indent:-9999em;cursor:pointer;padding:0;border-radius:50%;-moz-border-radius:50%;-webkit-border-radius:50%}.at44-follow-container a.aticon{height:2pc;margin:0 5px 5px 0}.at44-follow-container .at4-icon-fw{margin:0}
      </style>
      <style id="at4-share-offset" media="screen" type="text/css">
       #at4-share,#at4-soc {bottom:25px !important;top:auto;}
      </style>
     </head>
     <body id="news" style="">
      <div data-react-class="BrowseHappier" data-react-props='{"gt":1,"lt":11}'>
       <!-- react-empty: 1 -->
      </div>
      <div id="main_container">
       <div id="site_body">
        <div class="site_header_area">
         <header class="site_header">
          <div class="brand_area">
           <div class="brand1">
            <a class="nasa_logo" href="http://www.nasa.gov" target="_blank" title="visit nasa.gov">
             NASA
            </a>
           </div>
           <div class="brand2">
            <a class="top_logo" href="https://science.nasa.gov/" target="_blank" title="Explore NASA Science">
             NASA Science
            </a>
            <a class="sub_logo" href="/mars-exploration/#" title="Mars">
             Mars Exploration Program
            </a>
           </div>
           <img alt="" class="print_only print_logo" src="/assets/logo_nasa_trio_black@2x.png"/>
          </div>
          <a class="visuallyhidden focusable" href="#page">
           Skip Navigation
          </a>
          <div class="right_header_container">
           <a class="menu_button" href="javascript:void(0);" id="menu_button">
            <span class="menu_icon">
             menu
            </span>
           </a>
           <a class="modal_close" id="modal_close">
            <span class="modal_close_icon">
            </span>
           </a>
          </div>
          <div class="nav_area">
           <div id="site_nav_container">
            <nav class="site_nav">
             <ul class="nav">
              <li>
               <div class="arrow_box">
                <span class="arrow_down">
                </span>
               </div>
               <div class="nav_title">
                <a class="main_nav_item" href="/#red_planet" target="_self">
                 The Red Planet
                </a>
               </div>
               <div class="global_subnav_container">
                <ul class="subnav">
                 <li>
                  <a href="/#red_planet/0" target="_self">
                   Dashboard
                  </a>
                 </li>
                 <li>
                  <a href="/#red_planet/1" target="_self">
                   Science Goals
                  </a>
                 </li>
                 <li>
                  <a href="/#red_planet/2" target="_self">
                   The Planet
                  </a>
                 </li>
                 <li>
                  <a href="/#red_planet/3" target="_self">
                   Atmosphere
                  </a>
                 </li>
                 <li>
                  <a href="/#red_planet/4" target="_self">
                   Astrobiology
                  </a>
                 </li>
                 <li>
                  <a href="/#red_planet/5" target="_self">
                   Past, Present, Future, Timeline
                  </a>
                 </li>
                </ul>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="arrow_box">
                <span class="arrow_down">
                </span>
               </div>
               <div class="nav_title">
                <a class="main_nav_item" href="/#mars_exploration_program" target="_self">
                 The Program
                </a>
               </div>
               <div class="global_subnav_container">
                <ul class="subnav">
                 <li>
                  <a href="/#mars_exploration_program/0" target="_self">
                   Mission Statement
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/1" target="_self">
                   About the Program
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/2" target="_self">
                   Organization
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/3" target="_self">
                   Why Mars?
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/4" target="_self">
                   Research Programs
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/5" target="_self">
                   Planetary Resources
                  </a>
                 </li>
                 <li>
                  <a href="/#mars_exploration_program/6" target="_self">
                   Technologies
                  </a>
                 </li>
                </ul>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="arrow_box">
                <span class="arrow_down">
                </span>
               </div>
               <div class="nav_title">
                <a class="main_nav_item" href="/#news_and_events" target="_self">
                 News &amp; Events
                </a>
               </div>
               <div class="global_subnav_container">
                <ul class="subnav">
                 <li class="current">
                  <a href="/news" target="_self">
                   News
                  </a>
                 </li>
                 <li>
                  <a href="/events" target="_self">
                   Events
                  </a>
                 </li>
                </ul>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="arrow_box">
                <span class="arrow_down">
                </span>
               </div>
               <div class="nav_title">
                <a class="main_nav_item" href="/#multimedia" target="_self">
                 Multimedia
                </a>
               </div>
               <div class="global_subnav_container">
                <ul class="subnav">
                 <li>
                  <a href="/multimedia/images/" target="_self">
                   Images
                  </a>
                 </li>
                 <li>
                  <a href="/multimedia/videos/" target="_self">
                   Videos
                  </a>
                 </li>
                </ul>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="arrow_box">
                <span class="arrow_down">
                </span>
               </div>
               <div class="nav_title">
                <a class="main_nav_item" href="/#missions" target="_self">
                 Missions
                </a>
               </div>
               <div class="global_subnav_container">
                <ul class="subnav">
                 <li>
                  <a href="/mars-exploration/missions/?category=167" target="_self">
                   Past
                  </a>
                 </li>
                 <li>
                  <a href="/mars-exploration/missions/?category=170" target="_self">
                   Present
                  </a>
                 </li>
                 <li>
                  <a href="/mars-exploration/missions/?category=171" target="_self">
                   Future
                  </a>
                 </li>
                 <li>
                  <a href="/mars-exploration/partners" target="_self">
                   International Partners
                  </a>
                 </li>
                </ul>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="nav_title">
                <a class="main_nav_item" href="/#more" target="_self">
                 More
                </a>
               </div>
               <div class="gradient_line">
               </div>
              </li>
              <li>
               <div class="nav_title">
                <a class="main_nav_item" href="/legacy" target="_self">
                 Legacy Site
                </a>
               </div>
               <div class="gradient_line">
               </div>
              </li>
             </ul>
             <form action="https://mars.nasa.gov/search/" class="overlay_search nav_search">
              <label class="search_label">
               Search
              </label>
              <input class="search_field" name="q" type="text" value=""/>
              <div class="search_submit">
              </div>
             </form>
            </nav>
           </div>
          </div>
         </header>
        </div>
        <div id="sticky_nav_spacer">
        </div>
        <div id="page">
         <!-- title to go in the page_header -->
         <div class="header_mask">
         </div>
         <div class="react_grid_list" data-react-class="GridListPage" data-react-props='{"left_column":false,"class_name":"","default_view":"list_view","model":"news_items","view_toggle":false,"search":"true","list_item":"News","title":"News","categories":["19,165,184,204"],"order":"publish_date desc,created_at desc","no_items_text":"There are no items matching these criteria.","site_title":"NASA’s Mars Exploration Program ","short_title":"Mars","site_share_image":"/system/site_config_values/meta_share_images/1_142497main_PIA03154-200.jpg","per_page":null,"filters":"[ [ \"date\", [ [ \"2018\", \"2018\" ], [ \"2017\", \"2017\" ], [ \"2016\", \"2016\" ], [ \"2015\", \"2015\" ], [ \"2014\", \"2014\" ], [ \"2013\", \"2013\" ], [ \"2012\", \"2012\" ], [ \"2011\", \"2011\" ], [ \"2010\", \"2010\" ], [ \"2009\", \"2009\" ], [ \"2008\", \"2008\" ], [ \"2007\", \"2007\" ], [ \"2006\", \"2006\" ], [ \"2005\", \"2005\" ], [ \"2004\", \"2004\" ], [ \"2003\", \"2003\" ], [ \"2002\", \"2002\" ], [ \"2001\", \"2001\" ], [ \"2000\", \"2000\" ] ], [ \"Latest\", \"\" ], false ], [ \"categories\", [ [ \"Feature Stories\", 165 ], [ \"Press Releases\", 19 ], [ \"Spotlights\", 184 ], [ \"Status Reports\", 204 ] ], [ \"All Categories\", \"\" ], false ] ]","conditions":null,"scope_in_title":true,"options":{"blank_scope":"Latest"},"results_in_title":false}'>
          <section class="grid_gallery module list_view" data-reactroot="">
           <div class="grid_layout">
            <header class="gallery_header">
             <h2 class="module_title">
              News
             </h2>
             <section class="filter_bar">
              <div class="section_search">
               <div class="search_binder">
                <input class="search_field" name="search" placeholder="search" type="text" value=""/>
                <input class="search_submit" type="submit" value=""/>
               </div>
               <select class="filter" id="date" name="date">
                <option value="">
                 Latest
                </option>
                <option value="2018">
                 2018
                </option>
                <option value="2017">
                 2017
                </option>
                <option value="2016">
                 2016
                </option>
                <option value="2015">
                 2015
                </option>
                <option value="2014">
                 2014
                </option>
                <option value="2013">
                 2013
                </option>
                <option value="2012">
                 2012
                </option>
                <option value="2011">
                 2011
                </option>
                <option value="2010">
                 2010
                </option>
                <option value="2009">
                 2009
                </option>
                <option value="2008">
                 2008
                </option>
                <option value="2007">
                 2007
                </option>
                <option value="2006">
                 2006
                </option>
                <option value="2005">
                 2005
                </option>
                <option value="2004">
                 2004
                </option>
                <option value="2003">
                 2003
                </option>
                <option value="2002">
                 2002
                </option>
                <option value="2001">
                 2001
                </option>
                <option value="2000">
                 2000
                </option>
               </select>
               <select class="filter" id="categories" name="categories">
                <option value="">
                 All Categories
                </option>
                <option value="165">
                 Feature Stories
                </option>
                <option value="19">
                 Press Releases
                </option>
                <option value="184">
                 Spotlights
                </option>
                <option value="204">
                 Status Reports
                </option>
               </select>
              </div>
             </section>
             <!-- react-text: 37 -->
             <!-- /react-text -->
            </header>
            <ul class="item_list ">
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8361/nasas-insight-passes-halfway-to-mars-instruments-check-in/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA's InSight spacecraft, en route to a Nov. 26 landing on Mars, passed the halfway mark on Aug. 6. All of its instruments have been tested and are working well.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This artist's concept shows the InSight spacecraft, encapsulated in its aeroshell, as it cruises to Mars." src="/system/news_items/list_view_images/8361_PIA22547_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA's InSight Passes Halfway to Mars, Instruments Check In
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 August 20, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8361/nasas-insight-passes-halfway-to-mars-instruments-check-in/" target="_self">
                  NASA's InSight Passes Halfway to Mars, Instruments Check In
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA's InSight spacecraft, en route to a Nov. 26 landing on Mars, passed the halfway mark on Aug. 6. All of its instruments have been tested and are working well.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8360/six-things-about-opportunitys-recovery-efforts/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  The global dust storm on Mars could soon let in enough sunlight for the Opportunity rover to recharge.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8360_8354_Mars_Dust_Storm_PIA22487_thumb.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Six Things About Opportunity's Recovery Efforts
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 August 16, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8360/six-things-about-opportunitys-recovery-efforts/" target="_self">
                  Six Things About Opportunity's Recovery Efforts
                 </a>
                </div>
                <div class="article_teaser_body">
                 The global dust storm on Mars could soon let in enough sunlight for the Opportunity rover to recharge.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8359/meet-the-people-behind-nasas-insight-mars-lander/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  A series of NASA videos highlight scientists and engineers leading the next mission to Mars.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="L-R: Troy Hudson, Ravi Prakash and Marleen Sundgaard as they appear in a new video series profiling the scientists and engineers behind NASA's InSight spacecraft. Credit: NASA 360" src="/system/news_items/list_view_images/8359_people20180802-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Meet the People Behind NASA's InSight Mars Lander
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 August  2, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8359/meet-the-people-behind-nasas-insight-mars-lander/" target="_self">
                  Meet the People Behind NASA's InSight Mars Lander
                 </a>
                </div>
                <div class="article_teaser_body">
                 A series of NASA videos highlight scientists and engineers leading the next mission to Mars.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8358/mars-terraforming-not-possible-using-present-day-technology/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Transforming the inhospitable Martian environment into a place astronauts could explore without life support is not possible without technology well beyond today’s capabilities.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This is an artist's model of an early Mars - billions of years ago - which may have had oceans and a thicker atmosphere. It was created by filling Mars' lower altitudes with water and adding cloud cover." src="/system/news_items/list_view_images/8358_maven20180730-320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Mars Terraforming Not Possible Using Present-Day Technology
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 30, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8358/mars-terraforming-not-possible-using-present-day-technology/" target="_self">
                  Mars Terraforming Not Possible Using Present-Day Technology
                 </a>
                </div>
                <div class="article_teaser_body">
                 Transforming the inhospitable Martian environment into a place astronauts could explore without life support is not possible without technology well beyond today’s capabilities.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  It's the beginning of the end for the planet-encircling dust storm on Mars. But it could still be weeks, or even months, before skies are clear enough for NASA's Opportunity rover to recharge its batteries and phone home.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This series of images shows simulated views of a darkening Martian sky blotting out the Sun from NASA’s Opportunity rover’s point of view, with the right side simulating Opportunity’s current view in the global dust storm (June 2018). " src="/system/news_items/list_view_images/8348_PIA22521-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Opportunity Hunkers Down During Dust Storm
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 26, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/" target="_self">
                  Opportunity Hunkers Down During Dust Storm
                 </a>
                </div>
                <div class="article_teaser_body">
                 It's the beginning of the end for the planet-encircling dust storm on Mars. But it could still be weeks, or even months, before skies are clear enough for NASA's Opportunity rover to recharge its batteries and phone home.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8357/nasa-statement-on-possible-subsurface-lake-near-martian-south-pole/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  A new paper suggests that liquid water may be sitting under a layer of ice at Mars' south pole.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="A new paper suggests that liquid water may be sitting under a layer of ice at Mars' south pole." src="/system/news_items/list_view_images/8357_Screen-Shot-2018-07-25-at-4.24_320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Statement on Possible Subsurface Lake near Martian South Pole
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 25, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8357/nasa-statement-on-possible-subsurface-lake-near-martian-south-pole/" target="_self">
                  NASA Statement on Possible Subsurface Lake near Martian South Pole
                 </a>
                </div>
                <div class="article_teaser_body">
                 A new paper suggests that liquid water may be sitting under a layer of ice at Mars' south pole.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8356/jpls-martians-are-coming-to-griffith-observatory/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  On July 30, when Mars and Earth are closer than they've been since 2003, JPL scientists and engineers will be at a free public event at Griffith Observatory in Los Angeles.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Griffith Observatory in Los Angeles." src="/system/news_items/list_view_images/8356_21969_griffith_observatory_mars_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   JPL's 'Martians' Are Coming to Griffith Observatory
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 25, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8356/jpls-martians-are-coming-to-griffith-observatory/" target="_self">
                  JPL's 'Martians' Are Coming to Griffith Observatory
                 </a>
                </div>
                <div class="article_teaser_body">
                 On July 30, when Mars and Earth are closer than they've been since 2003, JPL scientists and engineers will be at a free public event at Griffith Observatory in Los Angeles.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8355/nasas-maven-spacecraft-finds-that-stolen-electrons-enable-unusual-aurora-on-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Auroras appear on Earth as ghostly displays of colorful light in the night sky, usually near the poles.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Animation of Proton Aurora at Mars" src="/system/news_items/list_view_images/8355_marsprotonauroramovie-320x240.gif"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA's MAVEN Spacecraft Finds That 'Stolen' Electrons Enable Unusual Aurora on Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 23, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8355/nasas-maven-spacecraft-finds-that-stolen-electrons-enable-unusual-aurora-on-mars/" target="_self">
                  NASA's MAVEN Spacecraft Finds That 'Stolen' Electrons Enable Unusual Aurora on Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 Auroras appear on Earth as ghostly displays of colorful light in the night sky, usually near the poles.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8354/storm-chasers-on-mars-searching-for-dusty-secrets/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Scientists with NASA's Mars orbiters have been waiting years for an event like the current Mars global dust storm.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8354_Mars_Dust_Storm_PIA22487_thumb.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   'Storm Chasers' on Mars Searching for Dusty Secrets
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 July 19, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8354/storm-chasers-on-mars-searching-for-dusty-secrets/" target="_self">
                  'Storm Chasers' on Mars Searching for Dusty Secrets
                 </a>
                </div>
                <div class="article_teaser_body">
                 Scientists with NASA's Mars orbiters have been waiting years for an event like the current Mars global dust storm.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8353/nasa-mars-mission-adds-southern-california-dates/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Looking for summer fun? Southern California families have their choice of the beach, movies, museums -- and even NASA's next mission to Mars.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="The Mars InSight Roadshow van at San Francisco's Exploratorium in April 2018. The Roadshow van will stop at different California venues to share public exhibits and lectures about NASA's InSight mission. " src="/system/news_items/list_view_images/8353_list_image.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Mars Mission Adds Southern California Dates
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June 26, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8353/nasa-mars-mission-adds-southern-california-dates/" target="_self">
                  NASA Mars Mission Adds Southern California Dates
                 </a>
                </div>
                <div class="article_teaser_body">
                 Looking for summer fun? Southern California families have their choice of the beach, movies, museums -- and even NASA's next mission to Mars.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8351/curiosity-captures-photos-of-thickening-dust/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  A storm of tiny dust particles has engulfed much of Mars over the last two weeks.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="A self-portrait by NASA's Curiosity rover taken on Sol 2082 (June 15, 2018). A Martian dust storm has reduced sunlight and visibility at the rover's location in Gale Crater. " src="/system/news_items/list_view_images/8351_PIA22486-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Curiosity Captures Photos of Thickening Dust
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June 20, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8351/curiosity-captures-photos-of-thickening-dust/" target="_self">
                  Curiosity Captures Photos of Thickening Dust
                 </a>
                </div>
                <div class="article_teaser_body">
                 A storm of tiny dust particles has engulfed much of Mars over the last two weeks.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8350/nasa-encounters-the-perfect-storm-for-science/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  One of the most intense Martian dust storms ever observed is being studied by a record number of NASA spacecraft.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This set of images from NASA’s Mars Reconnaissance Orbiter shows a fierce dust storm is kicking up on Mars, with rovers on the surface indicated as icons." src="/system/news_items/list_view_images/8350_marci-dgm-v04-for-home-page-5-br.gif"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Encounters the Perfect Storm for Science
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June 13, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8350/nasa-encounters-the-perfect-storm-for-science/" target="_self">
                  NASA Encounters the Perfect Storm for Science
                 </a>
                </div>
                <div class="article_teaser_body">
                 One of the most intense Martian dust storms ever observed is being studied by a record number of NASA spacecraft.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8349/media-telecon-about-mars-dust-storm-opportunity/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA will host a media telecon on Wednesday, June 13, about a massive Martian dust storm affecting the Opportunity rover, and how various missions can obtain unique science.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Mars, as seen by Mars Global Surveyor in 2003." src="/system/news_items/list_view_images/8349_PIA04591-br.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Media Telecon About Mars Dust Storm, Opportunity
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June 12, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8349/media-telecon-about-mars-dust-storm-opportunity/" target="_self">
                  Media Telecon About Mars Dust Storm, Opportunity
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA will host a media telecon on Wednesday, June 13, about a massive Martian dust storm affecting the Opportunity rover, and how various missions can obtain unique science.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA’s Curiosity rover has found evidence on Mars with implications for NASA’s search for life.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="​​NASA's Curiosity rover has discovered ancient organic molecules on Mars, embedded within sedimentary rocks that are billions of years old." src="/system/news_items/list_view_images/8347_curiosity_methane-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Finds Ancient Organic Material, Mysterious Methane on Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June  7, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/" target="_self">
                  NASA Finds Ancient Organic Material, Mysterious Methane on Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA’s Curiosity rover has found evidence on Mars with implications for NASA’s search for life.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8346/nasa-to-host-live-discussion-on-new-mars-science-results/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Questions are welcome during a live discussion at 11 a.m. PDT (2 p.m. EDT) Thursday, June 7, on new science results from NASA's Mars Curiosity rover.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Selfie of the Curiosity rover" src="/system/news_items/list_view_images/8346_PIA22207-br.png"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA to Host Live Discussion on New Mars Science Results
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June  6, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8346/nasa-to-host-live-discussion-on-new-mars-science-results/" target="_self">
                  NASA to Host Live Discussion on New Mars Science Results
                 </a>
                </div>
                <div class="article_teaser_body">
                 Questions are welcome during a live discussion at 11 a.m. PDT (2 p.m. EDT) Thursday, June 7, on new science results from NASA's Mars Curiosity rover.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8345/mars-curiositys-labs-are-back-in-action/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA's Curiosity rover is analyzing drilled samples on Mars in one of its onboard labs for the first time in more than a year.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="The drill bit of NASA's Curiosity Mars rover over one of the sample inlets on the rover's deck. The inlets lead to Curiosity's onboard laboratories. This image was taken on Sol 2068 by the rover's Mast Camera (Mastcam). It has been white balanced and contrast-enhanced. " src="/system/news_items/list_view_images/8345_PIA22327-br.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Mars Curiosity's Labs Are Back in Action
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June  4, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8345/mars-curiositys-labs-are-back-in-action/" target="_self">
                  Mars Curiosity's Labs Are Back in Action
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA's Curiosity rover is analyzing drilled samples on Mars in one of its onboard labs for the first time in more than a year.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8344/nasa-cubesats-steer-toward-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA has achieved a first for the class of tiny spacecraft known as CubeSats, which are opening new access to space.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's concept of one of NASA's MarCO CubeSats. " src="/system/news_items/list_view_images/8344_marco_tcm_20180601-320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA CubeSats Steer Toward Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June  1, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8344/nasa-cubesats-steer-toward-mars/" target="_self">
                  NASA CubeSats Steer Toward Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA has achieved a first for the class of tiny spacecraft known as CubeSats, which are opening new access to space.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8343/scientists-shrink-chemistry-lab-to-seek-evidence-of-life-on-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  An international team of scientists has created a tiny chemistry lab for a rover that will drill beneath the Martian surface looking for signs of past or present life.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8343_wilkinson-moma_320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Scientists Shrink Chemistry Lab to Seek Evidence of Life on Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 June  1, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8343/scientists-shrink-chemistry-lab-to-seek-evidence-of-life-on-mars/" target="_self">
                  Scientists Shrink Chemistry Lab to Seek Evidence of Life on Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 An international team of scientists has created a tiny chemistry lab for a rover that will drill beneath the Martian surface looking for signs of past or present life.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8342/insight-steers-toward-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  The spacecraft has completed its first trajectory correction maneuver.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="NASA's InSight spacecraft is currently cruising to Mars. Yesterday, it performed its first course correction guiding it to the Red Planet." src="/system/news_items/list_view_images/8342_insight20180523-320x240o.gif"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   InSight Steers Toward Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May 23, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8342/insight-steers-toward-mars/" target="_self">
                  InSight Steers Toward Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 The spacecraft has completed its first trajectory correction maneuver.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8341/drilling-success-curiosity-is-collecting-mars-rocks/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Engineers will now test delivering samples to instruments inside NASA's Curiosity Mars rover.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="NASA's Curiosity rover successfully drilled a 2-inch-deep hole in a target called &quot;Duluth&quot; on May 20. It was the first rock sample captured by the drill since October 2016. This image was taken by Curiosity's Mast Camera (Mastcam) on Sol 2057. " src="/system/news_items/list_view_images/8341_PIA22325-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Drilling Success: Curiosity is Collecting Mars Rocks
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May 23, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8341/drilling-success-curiosity-is-collecting-mars-rocks/" target="_self">
                  Drilling Success: Curiosity is Collecting Mars Rocks
                 </a>
                </div>
                <div class="article_teaser_body">
                 Engineers will now test delivering samples to instruments inside NASA's Curiosity Mars rover.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8339/nasas-curiosity-rover-aims-to-get-its-rhythm-back/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Rover engineers at JPL will try to restore percussive drilling on Mars this week, part of a larger series of tests that will last through summer.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="A test of a new percussive drilling technique at NASA's JPL. Later this week, NASA's Curiosity rover will test percussive drilling on Mars for the first time since December 2016." src="/system/news_items/list_view_images/8339_Curiosity_drill_PIA22324-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA's Curiosity Rover Aims to Get Its Rhythm Back
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May 17, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8339/nasas-curiosity-rover-aims-to-get-its-rhythm-back/" target="_self">
                  NASA's Curiosity Rover Aims to Get Its Rhythm Back
                 </a>
                </div>
                <div class="article_teaser_body">
                 Rover engineers at JPL will try to restore percussive drilling on Mars this week, part of a larger series of tests that will last through summer.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8338/a-pale-blue-dot-as-seen-by-a-cubesat/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  One of NASA's MarCO CubeSats has taken its first image.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This image taken by NASA's Mars Cube One (MarCO) CubeSats contains a photograph of Earth and Mars at a distance. " src="/system/news_items/list_view_images/8338_PIA22323_main-320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   A Pale Blue Dot, As Seen by a CubeSat
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May 15, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8338/a-pale-blue-dot-as-seen-by-a-cubesat/" target="_self">
                  A Pale Blue Dot, As Seen by a CubeSat
                 </a>
                </div>
                <div class="article_teaser_body">
                 One of NASA's MarCO CubeSats has taken its first image.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8335/mars-helicopter-to-fly-on-nasas-next-red-planet-rover-mission/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA is adding a Mars helicopter to the agency’s next mission to the Red Planet, Mars 2020.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8335_helicopter20180511-16-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Mars Helicopter to Fly on NASA’s Next Red Planet Rover Mission
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May 11, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8335/mars-helicopter-to-fly-on-nasas-next-red-planet-rover-mission/" target="_self">
                  Mars Helicopter to Fly on NASA’s Next Red Planet Rover Mission
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA is adding a Mars helicopter to the agency’s next mission to the Red Planet, Mars 2020.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8334/nasas-first-deep-space-cubesats-say-polo/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  MarCO is a pair of tiny spacecraft that launched with NASA's InSight lander today.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's rendering of the twin Mars Cube One (MarCO) spacecraft on their cruise to Mars. " src="/system/news_items/list_view_images/8334_PIA22314-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA's First Deep-Space CubeSats Say: 'Polo!'
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May  5, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8334/nasas-first-deep-space-cubesats-say-polo/" target="_self">
                  NASA's First Deep-Space CubeSats Say: 'Polo!'
                 </a>
                </div>
                <div class="article_teaser_body">
                 MarCO is a pair of tiny spacecraft that launched with NASA's InSight lander today.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8333/nasa-ula-launch-mission-to-study-how-mars-was-made/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA’s Mars InSight mission launched this morning on a 300-million-mile trip to Mars to study for the first time what lies deep beneath the surface of the Red Planet.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt='	The NASA InSight spacecraft launches onboard a United Launch Alliance Atlas-V rocket, Saturday, May 5, 2018, from Vandenberg Air Force Base in California. InSight, short for Interior Exploration using Seismic Investigations, Geodesy and Heat Transport, is a Mars lander designed to study the "inner space" of Mars: its crust, mantle, and core.' src="/system/news_items/list_view_images/8333_41864015862_4eb1b8de31_o-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA, ULA Launch Mission to Study How Mars Was Made
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May  5, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8333/nasa-ula-launch-mission-to-study-how-mars-was-made/" target="_self">
                  NASA, ULA Launch Mission to Study How Mars Was Made
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA’s Mars InSight mission launched this morning on a 300-million-mile trip to Mars to study for the first time what lies deep beneath the surface of the Red Planet.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8332/nasas-first-mission-to-study-the-interior-of-mars-awaits-may-5-launch/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  All systems are go for NASA’s next launch to the Red Planet.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's impression of the InSight lander on Mars. Credit: NASA/JPL-Caltech" src="/system/news_items/list_view_images/8332_21438_PIA22226_320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA’s First Mission to Study the Interior of Mars Awaits May 5 Launch
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 May  3, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8332/nasas-first-mission-to-study-the-interior-of-mars-awaits-may-5-launch/" target="_self">
                  NASA’s First Mission to Study the Interior of Mars Awaits May 5 Launch
                 </a>
                </div>
                <div class="article_teaser_body">
                 All systems are go for NASA’s next launch to the Red Planet.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8331/vice-president-pence-visits-jpl-previews-nasas-next-mars-mission-launch/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  A week before NASA's next Mars launch, Vice President Mike Pence toured the birthplace of the InSight Mars Lander and numerous other past, present and future space missions.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="U.S. Vice President Mike Pence, right, is presented a plaque by JPL Director Michael Watkins during a tour of NASA's Jet Propulsion Laboratory, Saturday, April 28, 2018 in Pasadena, California. The plaque presents a view of the Mars Science Laboratory rover Curiosity on the surface of Mars. Photo Credit: (NASA/Bill Ingalls)" src="/system/news_items/list_view_images/8331_vicepresidentpencejplvisit-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Vice President Pence Visits JPL, Previews NASA’s Next Mars Mission Launch
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 April 30, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8331/vice-president-pence-visits-jpl-previews-nasas-next-mars-mission-launch/" target="_self">
                  Vice President Pence Visits JPL, Previews NASA’s Next Mars Mission Launch
                 </a>
                </div>
                <div class="article_teaser_body">
                 A week before NASA's next Mars launch, Vice President Mike Pence toured the birthplace of the InSight Mars Lander and numerous other past, present and future space missions.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8330/nasa-sets-sights-on-may-5-launch-of-insight-to-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA’s next mission to Mars, InSight, is scheduled to launch Saturday, May 5, on a first-ever mission to study the heart of the Red Planet.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's rendering of a rocket launching with the InSight spacecraft in May." src="/system/news_items/list_view_images/8330_insight20180329-th.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Sets Sights on May 5 Launch of InSight to Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 April 27, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8330/nasa-sets-sights-on-may-5-launch-of-insight-to-mars/" target="_self">
                  NASA Sets Sights on May 5 Launch of InSight to Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA’s next mission to Mars, InSight, is scheduled to launch Saturday, May 5, on a first-ever mission to study the heart of the Red Planet.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8329/results-of-heat-shield-testing/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  A post-test inspection of the composite structure for a heat shield to be used on the Mars 2020 mission revealed that a fracture occurred during structural testing.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Artist's concept of the Mars Science Laboratory entry into the Martian atmosphere." src="/system/news_items/list_view_images/8329_PIA14835_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Results of Heat Shield Testing
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 April 26, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8329/results-of-heat-shield-testing/" target="_self">
                  Results of Heat Shield Testing
                 </a>
                </div>
                <div class="article_teaser_body">
                 A post-test inspection of the composite structure for a heat shield to be used on the Mars 2020 mission revealed that a fracture occurred during structural testing.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8328/nasa-engineers-dream-big-with-small-spacecraft/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  The first CubeSat mission to deep space will launch in May.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8328_PIA22314_320.png"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Engineers Dream Big with Small Spacecraft
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 April 19, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8328/nasa-engineers-dream-big-with-small-spacecraft/" target="_self">
                  NASA Engineers Dream Big with Small Spacecraft
                 </a>
                </div>
                <div class="article_teaser_body">
                 The first CubeSat mission to deep space will launch in May.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8327/bound-for-mars-countdown-to-first-interplanetary-launch-from-california/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  On May 5, millions of Californians may witness the historic first interplanetary launch from America’s West Coast.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="" src="/system/news_items/list_view_images/8327_InterplanetaryLaunch-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Bound for Mars: Countdown to First Interplanetary Launch from California
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 April  6, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8327/bound-for-mars-countdown-to-first-interplanetary-launch-from-california/" target="_self">
                  Bound for Mars: Countdown to First Interplanetary Launch from California
                 </a>
                </div>
                <div class="article_teaser_body">
                 On May 5, millions of Californians may witness the historic first interplanetary launch from America’s West Coast.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8326/nasa-invests-in-visionary-technology/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA is investing in technology concepts, including several from JPL, that may one day be used for future space exploration missions.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="NASA is investing in technology concepts, including several from JPL, that may one day be used for future space exploration missions." src="/system/news_items/list_view_images/8326_niac320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Invests in Visionary Technology
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 30, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8326/nasa-invests-in-visionary-technology/" target="_self">
                  NASA Invests in Visionary Technology
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA is investing in technology concepts, including several from JPL, that may one day be used for future space exploration missions.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA is about to go on a journey to study the center of Mars.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's rendering of the InSight spacecraft's cruise stage entering the Martian atmosphere." src="/system/news_items/list_view_images/8325_insight20180329b_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA is Ready to Study the Heart of Mars
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 29, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/" target="_self">
                  NASA is Ready to Study the Heart of Mars
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA is about to go on a journey to study the center of Mars.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8324/marsquakes-could-shake-up-planetary-science/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  InSight, the next mission to the Red Planet, will use seismology to see into the depths of Mars.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Inner Structure of Mars" src="/system/news_items/list_view_images/8324_PIA16078-320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   ‘Marsquakes’ Could Shake Up Planetary Science
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 28, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8324/marsquakes-could-shake-up-planetary-science/" target="_self">
                  ‘Marsquakes’ Could Shake Up Planetary Science
                 </a>
                </div>
                <div class="article_teaser_body">
                 InSight, the next mission to the Red Planet, will use seismology to see into the depths of Mars.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8323/mars-curiosity-celebrates-sol-2000/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA's Mars Curiosity rover just hit a new milestone: its two-thousandth Martian day on the Red Planet. An image mosaic taken recently offers a preview of what comes next.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="rugged landscape of hills " src="/system/news_items/list_view_images/8323_PIA22313_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Mars Curiosity Celebrates Sol 2,000
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 22, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8323/mars-curiosity-celebrates-sol-2000/" target="_self">
                  Mars Curiosity Celebrates Sol 2,000
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA's Mars Curiosity rover just hit a new milestone: its two-thousandth Martian day on the Red Planet. An image mosaic taken recently offers a preview of what comes next.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA’s next mission to Mars will be the topic of a media briefing Thursday, March 29, at JPL. The briefing will air live on NASA Television and the agency’s website.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="An artist's rendition of the InSight lander operating on the surface of Mars. " src="/system/news_items/list_view_images/8322_PIA22228_320.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Briefing on First Mission to Study Mars Interior
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 22, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/" target="_self">
                  NASA Briefing on First Mission to Study Mars Interior
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA’s next mission to Mars will be the topic of a media briefing Thursday, March 29, at JPL. The briefing will air live on NASA Television and the agency’s website.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA spacecraft travel to far-off destinations in space, but a new mobile app produced by NASA's Jet Propulsion Laboratory, Pasadena, California, brings spacecraft to users.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Free Spacecraft AR app uses Google ARCore technology" src="/system/news_items/list_view_images/8321_list_image.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   New 'AR' Mobile App Features 3-D NASA Spacecraft
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 20, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/" target="_self">
                  New 'AR' Mobile App Features 3-D NASA Spacecraft
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA spacecraft travel to far-off destinations in space, but a new mobile app produced by NASA's Jet Propulsion Laboratory, Pasadena, California, brings spacecraft to users.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8319/nasa-mars-mission-tours-california/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  Scientists and engineers with NASA's next mission to Mars will be touring California cities starting this month.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="This artist's concept shows the InSight lander, its sensors, cameras and instruments" src="/system/news_items/list_view_images/8319_PIA22227_320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   NASA Mars Mission Tours California
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 14, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8319/nasa-mars-mission-tours-california/" target="_self">
                  NASA Mars Mission Tours California
                 </a>
                </div>
                <div class="article_teaser_body">
                 Scientists and engineers with NASA's next mission to Mars will be touring California cities starting this month.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8318/next-nasa-mars-rover-reaches-key-manufacturing-milestone/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA's Mars 2020 mission has begun the assembly, test and launch operations (ATLO) phase of its development, on track for a July 2020 launch to Mars.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="A technician works on the descent stage for NASA’s Mars 2020 mission inside JPL’s Spacecraft Assembly Facility. " src="/system/news_items/list_view_images/8318_PIA22342_320x240.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Next NASA Mars Rover Reaches Key Manufacturing Milestone
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 13, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8318/next-nasa-mars-rover-reaches-key-manufacturing-milestone/" target="_self">
                  Next NASA Mars Rover Reaches Key Manufacturing Milestone
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA's Mars 2020 mission has begun the assembly, test and launch operations (ATLO) phase of its development, on track for a July 2020 launch to Mars.
                </div>
               </div>
              </div>
             </li>
             <li class="slide">
              <div class="image_and_description_container">
               <a href="/news/8317/witness-first-mars-launch-from-west-coast/" target="_self">
                <div class="rollover_description">
                 <div class="rollover_description_inner">
                  NASA invites digital creators to apply for social media credentials to cover the launch of the InSight mission to Mars, May 3-5, at California's Vandenberg Air Force Base.
                 </div>
                 <div class="overlay_arrow">
                  <img alt="More" src="/assets/overlay-arrow.png"/>
                 </div>
                </div>
                <div class="list_image">
                 <img alt="Launch of LDCM from Vandenberg AFB, California" src="/system/news_items/list_view_images/8317_list_image.jpg"/>
                </div>
                <div class="bottom_gradient">
                 <div>
                  <h3>
                   Witness First Mars Launch from West Coast
                  </h3>
                 </div>
                </div>
               </a>
               <div class="list_text">
                <div class="list_date">
                 March 12, 2018
                </div>
                <div class="content_title">
                 <a href="/news/8317/witness-first-mars-launch-from-west-coast/" target="_self">
                  Witness First Mars Launch from West Coast
                 </a>
                </div>
                <div class="article_teaser_body">
                 NASA invites digital creators to apply for social media credentials to cover the launch of the InSight mission to Mars, May 3-5, at California's Vandenberg Air Force Base.
                </div>
               </div>
              </div>
             </li>
            </ul>
            <footer class="list_footer more_button">
             <div class="loading">
             </div>
             <a class="button" href="#" type="button">
              More
             </a>
            </footer>
           </div>
          </section>
         </div>
         <section class="module suggested_features">
          <div class="grid_layout">
           <header>
            <h2 class="module_title">
             You Might Also Like
            </h2>
           </header>
           <section>
            <script>
             $(document).ready(function(){
        $(".features").slick({
          dots: false,
          infinite: true,
          speed: 300,
          slide: '.features .slide',
          slidesToShow: 3,
          slidesToScroll: 3,
          lazyLoad: 'ondemand',
          centerMode: false,
          arrows: true,
          appendArrows: '.features .slick-nav',
          appendDots: ".features .slick-nav",
          responsive: [{"breakpoint":953,"settings":{"slidesToShow":2,"slidesToScroll":2,"centerMode":false}},{"breakpoint":480,"settings":{"slidesToShow":1,"slidesToScroll":1,"centerMode":true,"arrows":false,"centerPadding":"25px"}}]
        });
      });
            </script>
            <div class="features slick-initialized slick-slider">
             <div class="slick-list draggable" tabindex="0">
              <div class="slick-track" style="opacity: 1; width: 4200px; transform: translate3d(-1050px, 0px, 0px);">
               <div class="slide slick-slide slick-cloned" index="-3" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA is about to go on a journey to study the center of Mars.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA is Ready to Study the Heart of Mars" class="img-lazy" data-lazy="/system/news_items/list_view_images/8325_insight20180329b_320.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/">
                  NASA is Ready to Study the Heart of Mars
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-cloned" index="-2" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA’s next mission to Mars will be the topic of a media briefing Thursday, March 29, at JPL. The briefing will air live on NASA Television and the agency’s website.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Briefing on First Mission to Study Mars Interior" class="img-lazy" data-lazy="/system/news_items/list_view_images/8322_PIA22228_320.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/">
                  NASA Briefing on First Mission to Study Mars Interior
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-cloned" index="-1" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA spacecraft travel to far-off destinations in space, but a new mobile app produced by NASA's Jet Propulsion Laboratory, Pasadena, California, brings spacecraft to users.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="New 'AR' Mobile App Features 3-D NASA Spacecraft" class="img-lazy" data-lazy="/system/news_items/list_view_images/8321_list_image.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/">
                  New 'AR' Mobile App Features 3-D NASA Spacecraft
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-active" index="0" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    It's the beginning of the end for the planet-encircling dust storm on Mars. But it could still be weeks, or even months, before skies are clear enough for NASA's Opportunity rover to recharge its batteries and phone home.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="Opportunity Hunkers Down During Dust Storm" class="img-lazy" src="/system/news_items/list_view_images/8348_PIA22521-320.jpg?1534891891379" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/">
                  Opportunity Hunkers Down During Dust Storm
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-active" index="1" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA’s Curiosity rover has found evidence on Mars with implications for NASA’s search for life.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Finds Ancient Organic Material, Mysterious Methane on Mars" class="img-lazy" src="/system/news_items/list_view_images/8347_curiosity_methane-320.jpg?1534891891380" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/">
                  NASA Finds Ancient Organic Material, Mysterious Methane on Mars
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-active" index="2" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8326/nasa-invests-in-visionary-technology/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA is investing in technology concepts, including several from JPL, that may one day be used for future space exploration missions.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Invests in Visionary Technology " class="img-lazy" src="/system/news_items/list_view_images/8326_niac320.jpg?1534891891380" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8326/nasa-invests-in-visionary-technology/">
                  NASA Invests in Visionary Technology
                 </a>
                </div>
               </div>
               <div class="slide slick-slide" index="3" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA is about to go on a journey to study the center of Mars.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA is Ready to Study the Heart of Mars" class="img-lazy" data-lazy="/system/news_items/list_view_images/8325_insight20180329b_320.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8325/nasa-is-ready-to-study-the-heart-of-mars/">
                  NASA is Ready to Study the Heart of Mars
                 </a>
                </div>
               </div>
               <div class="slide slick-slide" index="4" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA’s next mission to Mars will be the topic of a media briefing Thursday, March 29, at JPL. The briefing will air live on NASA Television and the agency’s website.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Briefing on First Mission to Study Mars Interior" class="img-lazy" data-lazy="/system/news_items/list_view_images/8322_PIA22228_320.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8322/nasa-briefing-on-first-mission-to-study-mars-interior/">
                  NASA Briefing on First Mission to Study Mars Interior
                 </a>
                </div>
               </div>
               <div class="slide slick-slide" index="5" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA spacecraft travel to far-off destinations in space, but a new mobile app produced by NASA's Jet Propulsion Laboratory, Pasadena, California, brings spacecraft to users.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="New 'AR' Mobile App Features 3-D NASA Spacecraft" class="img-lazy" data-lazy="/system/news_items/list_view_images/8321_list_image.jpg" src="/assets/loading_320x240.png"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8321/new-ar-mobile-app-features-3-d-nasa-spacecraft/">
                  New 'AR' Mobile App Features 3-D NASA Spacecraft
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-cloned" index="6" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    It's the beginning of the end for the planet-encircling dust storm on Mars. But it could still be weeks, or even months, before skies are clear enough for NASA's Opportunity rover to recharge its batteries and phone home.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="Opportunity Hunkers Down During Dust Storm" class="img-lazy" src="/system/news_items/list_view_images/8348_PIA22521-320.jpg?1534891891380" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8348/opportunity-hunkers-down-during-dust-storm/">
                  Opportunity Hunkers Down During Dust Storm
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-cloned" index="7" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA’s Curiosity rover has found evidence on Mars with implications for NASA’s search for life.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Finds Ancient Organic Material, Mysterious Methane on Mars" class="img-lazy" src="/system/news_items/list_view_images/8347_curiosity_methane-320.jpg?1534891891380" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8347/nasa-finds-ancient-organic-material-mysterious-methane-on-mars/">
                  NASA Finds Ancient Organic Material, Mysterious Methane on Mars
                 </a>
                </div>
               </div>
               <div class="slide slick-slide slick-cloned" index="8" style="width: 332px;">
                <div class="image_and_description_container">
                 <a href="/news/8326/nasa-invests-in-visionary-technology/">
                  <div class="rollover_description">
                   <div class="rollover_description_inner">
                    NASA is investing in technology concepts, including several from JPL, that may one day be used for future space exploration missions.
                   </div>
                   <div class="overlay_arrow">
                    <img alt="More" src="/assets/overlay-arrow.png"/>
                   </div>
                  </div>
                  <img alt="NASA Invests in Visionary Technology " class="img-lazy" src="/system/news_items/list_view_images/8326_niac320.jpg?1534891891381" style="opacity: 1;"/>
                 </a>
                </div>
                <div class="content_title">
                 <a href="/news/8326/nasa-invests-in-visionary-technology/">
                  NASA Invests in Visionary Technology
                 </a>
                </div>
               </div>
              </div>
             </div>
             <div class="grid_layout">
              <div class="slick-nav_container">
               <div class="slick-nav">
                <button class="slick-prev" data-role="none" style="display: block;" type="button">
                 Previous
                </button>
                <button class="slick-next" data-role="none" style="display: block;" type="button">
                 Next
                </button>
               </div>
              </div>
             </div>
            </div>
           </section>
          </div>
         </section>
        </div>
        <footer id="site_footer">
         <div class="grid_layout">
          <section class="upper_footer">
           <div class="share">
            <h2>
             Follow the Journey
            </h2>
            <div class="social_icons">
             <!-- AddThis Button BEGIN -->
             <div class="addthis_toolbox addthis_default_style addthis_32x32_style">
              <a addthis:userid="NASABeAMartian" class="addthis_button_twitter_follow icon at300b" href="//twitter.com/NASABeAMartian" target="_blank" title="Follow on Twitter">
               <img alt="twitter" src="/assets/twitter_icon@2x.png"/>
               <span class="addthis_follow_label">
                Twitter
               </span>
              </a>
              <a addthis:userid="NASABEAM" class="addthis_button_facebook_follow icon at300b" href="http://www.facebook.com/NASABEAM" target="_blank" title="Follow on Facebook">
               <img alt="facebook" src="/assets/facebook_icon@2x.png"/>
               <span class="addthis_follow_label">
                Facebook
               </span>
              </a>
              <a addthis:userid="nasa" class="addthis_button_instagram_follow icon at300b" href="http://instagram.com/nasa" target="_blank" title="Follow on Instagram">
               <img alt="instagram" src="/assets/instagram_icon@2x.png"/>
               <span class="addthis_follow_label">
                Instagram
               </span>
              </a>
              <a addthis:url="https://mars.nasa.gov/rss/api/?feed=news&amp;category=all&amp;feedtype=rss" class="addthis_button_rss_follow icon at300b" href="https://mars.nasa.gov/rss/api/?feed=news&amp;category=all&amp;feedtype=rss" target="_blank" title="Follow on RSS">
               <img alt="rss" src="/assets/rss_icon@2x.png"/>
               <span class="addthis_follow_label">
                RSS
               </span>
              </a>
              <div class="atclear">
              </div>
             </div>
             <script>
              addthis_loader.init("//s7.addthis.com/js/300/addthis_widget.js#pubid=ra-5a690e4c1320e328", {follow: true})
             </script>
             <!-- AddThis Button END -->
            </div>
           </div>
           <div class="gradient_line">
           </div>
          </section>
          <section class="sitemap">
           <div class="sitemap_directory" id="sitemap_directory" style="position: relative; height: 293.516px;">
            <div class="sitemap_block" style="position: absolute; left: 0px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#red_planet">
                The Red Planet
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li>
                   <a href="/#red_planet/0" target="_self">
                    Dashboard
                   </a>
                  </li>
                  <li>
                   <a href="/#red_planet/1" target="_self">
                    Science Goals
                   </a>
                  </li>
                  <li>
                   <a href="/#red_planet/2" target="_self">
                    The Planet
                   </a>
                  </li>
                  <li>
                   <a href="/#red_planet/3" target="_self">
                    Atmosphere
                   </a>
                  </li>
                  <li>
                   <a href="/#red_planet/4" target="_self">
                    Astrobiology
                   </a>
                  </li>
                  <li>
                   <a href="/#red_planet/5" target="_self">
                    Past, Present, Future, Timeline
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 194px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#mars_exploration_program">
                The Program
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li>
                   <a href="/#mars_exploration_program/0" target="_self">
                    Mission Statement
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/1" target="_self">
                    About the Program
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/2" target="_self">
                    Organization
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/3" target="_self">
                    Why Mars?
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/4" target="_self">
                    Research Programs
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/5" target="_self">
                    Planetary Resources
                   </a>
                  </li>
                  <li>
                   <a href="/#mars_exploration_program/6" target="_self">
                    Technologies
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 388px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#news_and_events">
                News &amp; Events
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li class="current">
                   <a href="/news" target="_self">
                    News
                   </a>
                  </li>
                  <li>
                   <a href="/events" target="_self">
                    Events
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 582px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#multimedia">
                Multimedia
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li>
                   <a href="/multimedia/images/" target="_self">
                    Images
                   </a>
                  </li>
                  <li>
                   <a href="/multimedia/videos/" target="_self">
                    Videos
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 776px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#missions">
                Missions
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li>
                   <a href="/mars-exploration/missions/?category=167" target="_self">
                    Past
                   </a>
                  </li>
                  <li>
                   <a href="/mars-exploration/missions/?category=170" target="_self">
                    Present
                   </a>
                  </li>
                  <li>
                   <a href="/mars-exploration/missions/?category=171" target="_self">
                    Future
                   </a>
                  </li>
                  <li>
                   <a href="/mars-exploration/partners" target="_self">
                    International Partners
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 970px; top: 0px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/#more">
                More
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
            <div class="sitemap_block" style="position: absolute; left: 970px; top: 54px;">
             <div class="footer_sitemap_item">
              <h3 class="sitemap_title">
               <a href="/legacy">
                Legacy Site
               </a>
              </h3>
              <ul>
               <li>
                <div class="global_subnav_container">
                 <ul class="subnav">
                  <li>
                   <a class="" href="/legacy" target="_self">
                    Legacy Site
                   </a>
                  </li>
                 </ul>
                </div>
               </li>
              </ul>
             </div>
            </div>
           </div>
           <div class="gradient_line">
           </div>
          </section>
          <section class="lower_footer">
           <div class="nav_container">
            <nav>
             <ul>
              <li>
               <a href="http://science.nasa.gov/" target="_blank">
                NASA Science Mission Directorate
               </a>
              </li>
              <li>
               <a href="https://www.jpl.nasa.gov/copyrights.php" target="_blank">
                Privacy
               </a>
              </li>
              <li>
               <a href="http://www.jpl.nasa.gov/imagepolicy/" target="_blank">
                Image Policy
               </a>
              </li>
              <li>
               <a href="https://mars.nasa.gov/feedback/" target="_self">
                Feedback
               </a>
              </li>
              <li>
               <a href="http://mars.nasa.gov/legacy" target="_blank">
                Legacy Mars Site
               </a>
              </li>
             </ul>
            </nav>
           </div>
           <div class="credits">
            <div class="footer_brands_top">
             <p>
              Managed by the Mars Exploration Program and the Jet Propulsion Laboratory for NASA’s Science Mission Directorate
             </p>
            </div>
            <!-- .footer_brands -->
            <!-- %a.jpl{href: "", target: "_blank"}Institution -->
            <!-- -->
            <!-- %a.caltech{href: "", target: "_blank"}Institution -->
            <!-- .staff -->
            <!-- %p -->
            <!-- - get_staff_for_category(get_field_from_admin_config(:web_staff_category_id)) -->
            <!-- - @staff.each_with_index do |staff, idx| -->
            <!-- - unless staff.is_in_footer == 0 -->
            <!-- = staff.title + ": " -->
            <!-- - if staff.contact_link =~ /@/ -->
            <!-- = mail_to staff.contact_link, staff.name, :subject => "[#{@site_title}]" -->
            <!-- - elsif staff.contact_link.present? -->
            <!-- = link_to staff.name, staff.contact_link -->
            <!-- - else -->
            <!-- = staff.name -->
            <!-- - unless (idx + 1 == @staff.size) -->
            <!-- %br -->
           </div>
          </section>
         </div>
        </footer>
       </div>
      </div>
      <script id="_fed_an_ua_tag" src="https://dap.digitalgov.gov/Universal-Federated-Analytics-Min.js?agency=NASA&amp;subagency=JPL-Mars-MEPJPL&amp;pua=UA-9453474-9,UA-118212757-11&amp;dclink=true&amp;sp=searchbox&amp;exts=tif,tiff,wav" type="text/javascript">
      </script>
      <div id="_atssh" style="visibility: hidden; height: 1px; width: 1px; position: absolute; top: -9999px; z-index: 100000;">
       <iframe id="_atssh854" src="https://s7.addthis.com/static/sh.e4e8af4de595fdb10ec1459d.html#rand=0.3086598206302549&amp;iit=1534891892859&amp;tmr=load%3D1534891892027%26core%3D1534891892101%26main%3D1534891892844%26ifr%3D1534891892866&amp;cb=0&amp;cdn=0&amp;md=0&amp;kw=Mars%2Cmissions%2CNASA%2Crover%2CCuriosity%2COpportunity%2CInSight%2CMars%20Reconnaissance%20Orbiter%2Cfacts&amp;ab=-&amp;dh=mars.nasa.gov&amp;dr=&amp;du=https%3A%2F%2Fmars.nasa.gov%2Fnews%2F%3Fpage%3D0%26per_page%3D40%26order%3Dpublish_date%2Bdesc%252Ccreated_at%2Bdesc%26search%3D%26category%3D19%252C165%252C184%252C204%26blank_scope%3DLatest&amp;href=https%3A%2F%2Fmars.nasa.gov%2Fnews%2F&amp;dt=News%20%20%E2%80%93%20NASA%E2%80%99s%20Mars%20Exploration%20Program&amp;dbg=0&amp;cap=tc%3D0%26ab%3D0&amp;inst=1&amp;jsl=1&amp;prod=undefined&amp;lng=en&amp;ogt=image%2Cupdated_time%2Ctype%3Darticle%2Curl%2Ctitle%2Cdescription%2Csite_name&amp;pc=men&amp;pub=ra-5a690e4c1320e328&amp;ssl=1&amp;sid=5b7c9774008384fe&amp;srf=0.01&amp;ver=300&amp;xck=1&amp;xtr=0&amp;og=site_name%3DNASA%25E2%2580%2599s%2520Mars%2520Exploration%2520Program%26description%3DNASA%25E2%2580%2599s%2520real-time%2520portal%2520for%2520Mars%2520exploration%252C%2520featuring%2520the%2520latest%2520news%252C%2520images%252C%2520and%2520discoveries%2520from%2520the%2520Red%2520Planet.%26title%3DNews%2520%2520%25E2%2580%2593%2520NASA%25E2%2580%2599s%2520Mars%2520Exploration%2520Program%26url%3Dhttps%253A%252F%252Fmars.nasa.gov%252Fnews%26type%3Darticle%26updated_time%3D2017-09-22%252019%253A53%253A22%2520UTC%26image%3Dhttps%253A%252F%252Fmars.nasa.gov%252Fsystem%252Fsite_config_values%252Fmeta_share_images%252F1_142497main_PIA03154-200.jpg&amp;csi=undefined&amp;rev=v8.3.27-wp&amp;ct=1&amp;xld=1&amp;xd=1" style="height: 1px; width: 1px; position: absolute; top: 0px; z-index: 100000; border: 0px; left: 0px;" title="AddThis utility frame">
       </iframe>
      </div>
      <style id="service-icons-0">
      </style>
      <div aria-labelledby="at4-share-label" class="addthis-smartlayers addthis-smartlayers-desktop" role="region">
       <div id="at4-share-label">
        AddThis Sharing Sidebar
       </div>
       <div class="at4-share addthis_32x32_style atss atss-left addthis-animated slideInLeft" id="at4-share">
        <a class="at-share-btn at-svc-facebook" role="button" tabindex="1">
         <span class="at4-visually-hidden">
          Share to Facebook
         </span>
         <span class="at-icon-wrapper" style="background-color: rgb(59, 89, 152);">
          <svg aria-labelledby="at-svg-facebook-1" class="at-icon at-icon-facebook" role="img" style="fill: rgb(255, 255, 255);" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-facebook-1" xmlns="http://www.w3.org/1999/xhtml">
            Facebook
           </title>
           <g>
            <path d="M22 5.16c-.406-.054-1.806-.16-3.43-.16-3.4 0-5.733 1.825-5.733 5.17v2.882H9v3.913h3.837V27h4.604V16.965h3.823l.587-3.913h-4.41v-2.5c0-1.123.347-1.903 2.198-1.903H22V5.16z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-twitter" role="button" tabindex="1">
         <span class="at4-visually-hidden">
          Share to Twitter
         </span>
         <span class="at-icon-wrapper" style="background-color: rgb(29, 161, 242);">
          <svg aria-labelledby="at-svg-twitter-2" class="at-icon at-icon-twitter" role="img" style="fill: rgb(255, 255, 255);" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-twitter-2" xmlns="http://www.w3.org/1999/xhtml">
            Twitter
           </title>
           <g>
            <path d="M27.996 10.116c-.81.36-1.68.602-2.592.71a4.526 4.526 0 0 0 1.984-2.496 9.037 9.037 0 0 1-2.866 1.095 4.513 4.513 0 0 0-7.69 4.116 12.81 12.81 0 0 1-9.3-4.715 4.49 4.49 0 0 0-.612 2.27 4.51 4.51 0 0 0 2.008 3.755 4.495 4.495 0 0 1-2.044-.564v.057a4.515 4.515 0 0 0 3.62 4.425 4.52 4.52 0 0 1-2.04.077 4.517 4.517 0 0 0 4.217 3.134 9.055 9.055 0 0 1-5.604 1.93A9.18 9.18 0 0 1 6 23.85a12.773 12.773 0 0 0 6.918 2.027c8.3 0 12.84-6.876 12.84-12.84 0-.195-.005-.39-.014-.583a9.172 9.172 0 0 0 2.252-2.336" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-reddit" role="button" tabindex="1">
         <span class="at4-visually-hidden">
          Share to Reddit
         </span>
         <span class="at-icon-wrapper" style="background-color: rgb(255, 87, 0);">
          <svg aria-labelledby="at-svg-reddit-3" class="at-icon at-icon-reddit" role="img" style="fill: rgb(255, 255, 255);" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-reddit-3" xmlns="http://www.w3.org/1999/xhtml">
            Reddit
           </title>
           <g>
            <path d="M27 15.5a2.452 2.452 0 0 1-1.338 2.21c.098.38.147.777.147 1.19 0 1.283-.437 2.47-1.308 3.563-.872 1.092-2.06 1.955-3.567 2.588-1.506.634-3.143.95-4.91.95-1.768 0-3.403-.316-4.905-.95-1.502-.632-2.69-1.495-3.56-2.587-.872-1.092-1.308-2.28-1.308-3.562 0-.388.045-.777.135-1.166a2.47 2.47 0 0 1-1.006-.912c-.253-.4-.38-.842-.38-1.322 0-.678.237-1.26.712-1.744a2.334 2.334 0 0 1 1.73-.726c.697 0 1.29.26 1.78.782 1.785-1.258 3.893-1.928 6.324-2.01l1.424-6.467a.42.42 0 0 1 .184-.26.4.4 0 0 1 .32-.063l4.53 1.006c.147-.306.368-.553.662-.74a1.78 1.78 0 0 1 .97-.278c.508 0 .94.18 1.302.54.36.36.54.796.54 1.31 0 .512-.18.95-.54 1.315-.36.364-.794.546-1.302.546-.507 0-.94-.18-1.295-.54a1.793 1.793 0 0 1-.533-1.308l-4.1-.92-1.277 5.86c2.455.074 4.58.736 6.37 1.985a2.315 2.315 0 0 1 1.757-.757c.68 0 1.256.242 1.73.726.476.484.713 1.066.713 1.744zm-16.868 2.47c0 .513.178.95.534 1.315.356.365.787.547 1.295.547.508 0 .942-.182 1.302-.547.36-.364.54-.802.54-1.315 0-.513-.18-.95-.54-1.31-.36-.36-.794-.54-1.3-.54-.5 0-.93.183-1.29.547a1.79 1.79 0 0 0-.54 1.303zm9.944 4.406c.09-.09.135-.2.135-.323a.444.444 0 0 0-.44-.447c-.124 0-.23.042-.32.124-.336.348-.83.605-1.486.77a7.99 7.99 0 0 1-1.964.248 7.99 7.99 0 0 1-1.964-.248c-.655-.165-1.15-.422-1.486-.77a.456.456 0 0 0-.32-.124.414.414 0 0 0-.306.124.41.41 0 0 0-.135.317.45.45 0 0 0 .134.33c.352.355.837.636 1.455.843.617.207 1.118.33 1.503.366a11.6 11.6 0 0 0 1.117.056c.36 0 .733-.02 1.117-.056.385-.037.886-.16 1.504-.366.62-.207 1.104-.488 1.456-.844zm-.037-2.544c.507 0 .938-.182 1.294-.547.356-.364.534-.802.534-1.315 0-.505-.18-.94-.54-1.303a1.75 1.75 0 0 0-1.29-.546c-.506 0-.94.18-1.3.54-.36.36-.54.797-.54 1.31s.18.95.54 1.315c.36.365.794.547 1.3.547z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-email" role="button" tabindex="1">
         <span class="at4-visually-hidden">
          Share to Email
         </span>
         <span class="at-icon-wrapper" style="background-color: rgb(132, 132, 132);">
          <svg aria-labelledby="at-svg-email-4" class="at-icon at-icon-email" role="img" style="fill: rgb(255, 255, 255);" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-email-4" xmlns="http://www.w3.org/1999/xhtml">
            Email
           </title>
           <g>
            <g fill-rule="evenodd">
            </g>
            <path d="M27 22.757c0 1.24-.988 2.243-2.19 2.243H7.19C5.98 25 5 23.994 5 22.757V13.67c0-.556.39-.773.855-.496l8.78 5.238c.782.467 1.95.467 2.73 0l8.78-5.238c.472-.28.855-.063.855.495v9.087z">
            </path>
            <path d="M27 9.243C27 8.006 26.02 7 24.81 7H7.19C5.988 7 5 8.004 5 9.243v.465c0 .554.385 1.232.857 1.514l9.61 5.733c.267.16.8.16 1.067 0l9.61-5.733c.473-.283.856-.96.856-1.514v-.465z">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-compact" role="button" tabindex="1">
         <span class="at4-visually-hidden">
          More AddThis Share options
         </span>
         <span class="at-icon-wrapper" style="background-color: rgb(255, 101, 80);">
          <svg aria-labelledby="at-svg-addthis-5" class="at-icon at-icon-addthis" role="img" style="fill: rgb(255, 255, 255);" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-addthis-5" xmlns="http://www.w3.org/1999/xhtml">
            Addthis
           </title>
           <g>
            <path d="M18 14V8h-4v6H8v4h6v6h4v-6h6v-4h-6z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <div class="at-custom-sidebar-counter" style="width: 48px; word-wrap: break-word;">
         <div class="at-custom-sidebar-count" style="color: rgb(34, 34, 34);">
         </div>
         <div class="at-custom-sidebar-text" style="color: rgb(34, 34, 34);">
          SHARES
         </div>
        </div>
        <div class="at-share-close-control ats-transparent at4-hide-content at4-show" id="at4-scc" title="Hide">
         <div class="at4-arrow at-left">
          Hide
         </div>
        </div>
       </div>
       <div class="at-share-open-control at-share-open-control-left ats-transparent at4-hide" id="at4-soc" title="Show">
        <div class="at4-arrow at-right">
         Show
        </div>
       </div>
      </div>
      <div aria-labelledby="at-thankyou-label" class="at4-thankyou at4-thankyou-background at4-hide ats-transparent at4-thankyou-desktop addthis-smartlayers addthis-animated fadeIn at4-show" id="at4-thankyou" role="dialog">
       <div class="at4lb-inner">
        <button class="at4x" title="Close">
         Close
        </button>
        <a id="at4-palogo">
         <div>
          <a class="at-branding-logo" href="//www.addthis.com/website-tools/overview?utm_source=AddThis%20Tools&amp;utm_medium=image" target="_blank" title="Powered by AddThis">
           <div class="at-branding-icon">
           </div>
           <span class="at-branding-addthis">
            AddThis
           </span>
          </a>
         </div>
        </a>
        <div class="at4-thankyou-inner">
         <div class="thankyou-title" id="at-thankyou-label">
         </div>
         <div class="thankyou-description">
         </div>
         <div class="at4-thankyou-layer">
         </div>
        </div>
       </div>
      </div>
      <div aria-labelledby="at-share-dock-label" class="at-share-dock-outer at4-hide addthis-smartlayers at4-visually-hidden addthis-smartlayers-mobile" role="region">
       <div class="at4-hide" id="at-share-dock-label">
        AddThis Sharing
       </div>
       <div class="at-share-dock atss atss-bottom at-shfs-small addthis-animated slideInUp at4-show at4-hide" id="at-share-dock">
        <a class="at4-count" href="#" style="width: 16.6667%;">
         <span class="at4-counter">
         </span>
         <span class="at4-share-label">
          SHARES
         </span>
        </a>
        <a class="at-share-btn at-svc-facebook" role="button" style="width: 16.6667%;" tabindex="1" title="Facebook">
         <span class="at-icon-wrapper" style="background-color: rgb(59, 89, 152);">
          <svg alt="Facebook" aria-labelledby="at-svg-facebook-6" class="at-icon at-icon-facebook" role="img" style="fill: rgb(255, 255, 255); width: 24px; height: 24px;" title="Facebook" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-facebook-6" xmlns="http://www.w3.org/1999/xhtml">
            Facebook
           </title>
           <g>
            <path d="M22 5.16c-.406-.054-1.806-.16-3.43-.16-3.4 0-5.733 1.825-5.733 5.17v2.882H9v3.913h3.837V27h4.604V16.965h3.823l.587-3.913h-4.41v-2.5c0-1.123.347-1.903 2.198-1.903H22V5.16z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-twitter" role="button" style="width: 16.6667%;" tabindex="1" title="Twitter">
         <span class="at-icon-wrapper" style="background-color: rgb(29, 161, 242);">
          <svg alt="Twitter" aria-labelledby="at-svg-twitter-7" class="at-icon at-icon-twitter" role="img" style="fill: rgb(255, 255, 255); width: 24px; height: 24px;" title="Twitter" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-twitter-7" xmlns="http://www.w3.org/1999/xhtml">
            Twitter
           </title>
           <g>
            <path d="M27.996 10.116c-.81.36-1.68.602-2.592.71a4.526 4.526 0 0 0 1.984-2.496 9.037 9.037 0 0 1-2.866 1.095 4.513 4.513 0 0 0-7.69 4.116 12.81 12.81 0 0 1-9.3-4.715 4.49 4.49 0 0 0-.612 2.27 4.51 4.51 0 0 0 2.008 3.755 4.495 4.495 0 0 1-2.044-.564v.057a4.515 4.515 0 0 0 3.62 4.425 4.52 4.52 0 0 1-2.04.077 4.517 4.517 0 0 0 4.217 3.134 9.055 9.055 0 0 1-5.604 1.93A9.18 9.18 0 0 1 6 23.85a12.773 12.773 0 0 0 6.918 2.027c8.3 0 12.84-6.876 12.84-12.84 0-.195-.005-.39-.014-.583a9.172 9.172 0 0 0 2.252-2.336" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-reddit" role="button" style="width: 16.6667%;" tabindex="1" title="Reddit">
         <span class="at-icon-wrapper" style="background-color: rgb(255, 87, 0);">
          <svg alt="Reddit" aria-labelledby="at-svg-reddit-8" class="at-icon at-icon-reddit" role="img" style="fill: rgb(255, 255, 255); width: 24px; height: 24px;" title="Reddit" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-reddit-8" xmlns="http://www.w3.org/1999/xhtml">
            Reddit
           </title>
           <g>
            <path d="M27 15.5a2.452 2.452 0 0 1-1.338 2.21c.098.38.147.777.147 1.19 0 1.283-.437 2.47-1.308 3.563-.872 1.092-2.06 1.955-3.567 2.588-1.506.634-3.143.95-4.91.95-1.768 0-3.403-.316-4.905-.95-1.502-.632-2.69-1.495-3.56-2.587-.872-1.092-1.308-2.28-1.308-3.562 0-.388.045-.777.135-1.166a2.47 2.47 0 0 1-1.006-.912c-.253-.4-.38-.842-.38-1.322 0-.678.237-1.26.712-1.744a2.334 2.334 0 0 1 1.73-.726c.697 0 1.29.26 1.78.782 1.785-1.258 3.893-1.928 6.324-2.01l1.424-6.467a.42.42 0 0 1 .184-.26.4.4 0 0 1 .32-.063l4.53 1.006c.147-.306.368-.553.662-.74a1.78 1.78 0 0 1 .97-.278c.508 0 .94.18 1.302.54.36.36.54.796.54 1.31 0 .512-.18.95-.54 1.315-.36.364-.794.546-1.302.546-.507 0-.94-.18-1.295-.54a1.793 1.793 0 0 1-.533-1.308l-4.1-.92-1.277 5.86c2.455.074 4.58.736 6.37 1.985a2.315 2.315 0 0 1 1.757-.757c.68 0 1.256.242 1.73.726.476.484.713 1.066.713 1.744zm-16.868 2.47c0 .513.178.95.534 1.315.356.365.787.547 1.295.547.508 0 .942-.182 1.302-.547.36-.364.54-.802.54-1.315 0-.513-.18-.95-.54-1.31-.36-.36-.794-.54-1.3-.54-.5 0-.93.183-1.29.547a1.79 1.79 0 0 0-.54 1.303zm9.944 4.406c.09-.09.135-.2.135-.323a.444.444 0 0 0-.44-.447c-.124 0-.23.042-.32.124-.336.348-.83.605-1.486.77a7.99 7.99 0 0 1-1.964.248 7.99 7.99 0 0 1-1.964-.248c-.655-.165-1.15-.422-1.486-.77a.456.456 0 0 0-.32-.124.414.414 0 0 0-.306.124.41.41 0 0 0-.135.317.45.45 0 0 0 .134.33c.352.355.837.636 1.455.843.617.207 1.118.33 1.503.366a11.6 11.6 0 0 0 1.117.056c.36 0 .733-.02 1.117-.056.385-.037.886-.16 1.504-.366.62-.207 1.104-.488 1.456-.844zm-.037-2.544c.507 0 .938-.182 1.294-.547.356-.364.534-.802.534-1.315 0-.505-.18-.94-.54-1.303a1.75 1.75 0 0 0-1.29-.546c-.506 0-.94.18-1.3.54-.36.36-.54.797-.54 1.31s.18.95.54 1.315c.36.365.794.547 1.3.547z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-email" role="button" style="width: 16.6667%;" tabindex="1" title="Email">
         <span class="at-icon-wrapper" style="background-color: rgb(132, 132, 132);">
          <svg alt="Email" aria-labelledby="at-svg-email-9" class="at-icon at-icon-email" role="img" style="fill: rgb(255, 255, 255); width: 24px; height: 24px;" title="Email" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-email-9" xmlns="http://www.w3.org/1999/xhtml">
            Email
           </title>
           <g>
            <g fill-rule="evenodd">
            </g>
            <path d="M27 22.757c0 1.24-.988 2.243-2.19 2.243H7.19C5.98 25 5 23.994 5 22.757V13.67c0-.556.39-.773.855-.496l8.78 5.238c.782.467 1.95.467 2.73 0l8.78-5.238c.472-.28.855-.063.855.495v9.087z">
            </path>
            <path d="M27 9.243C27 8.006 26.02 7 24.81 7H7.19C5.988 7 5 8.004 5 9.243v.465c0 .554.385 1.232.857 1.514l9.61 5.733c.267.16.8.16 1.067 0l9.61-5.733c.473-.283.856-.96.856-1.514v-.465z">
            </path>
           </g>
          </svg>
         </span>
        </a>
        <a class="at-share-btn at-svc-compact" role="button" style="width: 16.6667%;" tabindex="1" title="More">
         <span class="at-icon-wrapper" style="background-color: rgb(255, 101, 80);">
          <svg alt="More" aria-labelledby="at-svg-addthis-10" class="at-icon at-icon-addthis" role="img" style="fill: rgb(255, 255, 255); width: 24px; height: 24px;" title="More" version="1.1" viewbox="0 0 32 32" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
           <title id="at-svg-addthis-10" xmlns="http://www.w3.org/1999/xhtml">
            Addthis
           </title>
           <g>
            <path d="M18 14V8h-4v6H8v4h6v6h4v-6h6v-4h-6z" fill-rule="evenodd">
            </path>
           </g>
          </svg>
         </span>
        </a>
       </div>
      </div>
     </body>
    </html>



```python
# Collect the latest Mars News Title and Paragraph Text and store into variables

news_title = soup.find("div", class_="content_title").text
print(news_title)

news_p = soup.find("div", class_="article_teaser_body").text
print(news_p)
```

    NASA's InSight Passes Halfway to Mars, Instruments Check In
    NASA's InSight spacecraft, en route to a Nov. 26 landing on Mars, passed the halfway mark on Aug. 6. All of its instruments have been tested and are working well.


### JPL Mars Space Images - Featured Image


```python
# Initialize browser
browser = init_browser()

# Setup URL to JPL
url = 'https://www.jpl.nasa.gov/spaceimages/?search=&category=Mars'
browser.visit(url)

# Move browser to featured image page
browser.click_link_by_partial_text('FULL IMAGE')
time.sleep(5)
browser.click_link_by_partial_text('more info')

# Create BeautifulSoup object; parse with 'html.parser'
jpl_html = browser.html
soup = BeautifulSoup(jpl_html, 'html.parser')

# Get featured image
img_url = soup.find('img', class_='main_image')['src']
jpl_url = "https://www.jpl.nasa.gov"
featured_image_url = jpl_url + img_url

# Close browser
browser.quit()


print(featured_image_url)
```

    /usr/local/bin/chromedriver
    https://www.jpl.nasa.gov/spaceimages/images/largesize/PIA09320_hires.jpg


### Mars Weather


```python
# Setup URL to Mars Weather Twitter Account
url = 'https://twitter.com/marswxreport?lang=en'

# Retrieve page with the requests module
response = requests.get(url)

# Create BeautifulSoup object; parse with 'html.parser'
soup = BeautifulSoup(response.text, 'html.parser')

# Find latest tweet and save it
mars_weather = soup.find("p", class_="TweetTextSize TweetTextSize--normal js-tweet-text tweet-text").text

print(mars_weather)
```

    Sol 2145 (2018-08-19), high -10C/14F, low -66C/-86F, pressure at 8.68 hPa, daylight 05:29-17:43


### Mars Facts


```python
# Setup URL to Mars Facts webpage
url = 'https://space-facts.com/mars/'

# Scrape any table data from a page
tables = pd.read_html(url)
tables

```




    [                      0                              1
     0  Equatorial Diameter:                       6,792 km
     1       Polar Diameter:                       6,752 km
     2                 Mass:  6.42 x 10^23 kg (10.7% Earth)
     3                Moons:            2 (Phobos & Deimos)
     4       Orbit Distance:       227,943,824 km (1.52 AU)
     5         Orbit Period:           687 days (1.9 years)
     6  Surface Temperature:                  -153 to 20 °C
     7         First Record:              2nd millennium BC
     8          Recorded By:           Egyptian astronomers]




```python
# Create dataframe from scraped data
df = df = tables[0]
df.columns = ['Description', 'Value']
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Description</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Equatorial Diameter:</td>
      <td>6,792 km</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Polar Diameter:</td>
      <td>6,752 km</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mass:</td>
      <td>6.42 x 10^23 kg (10.7% Earth)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Moons:</td>
      <td>2 (Phobos &amp; Deimos)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Orbit Distance:</td>
      <td>227,943,824 km (1.52 AU)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Orbit Period:</td>
      <td>687 days (1.9 years)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Surface Temperature:</td>
      <td>-153 to 20 °C</td>
    </tr>
    <tr>
      <th>7</th>
      <td>First Record:</td>
      <td>2nd millennium BC</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Recorded By:</td>
      <td>Egyptian astronomers</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Set the index to the Description column
df.set_index('Description', inplace=True)
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Value</th>
    </tr>
    <tr>
      <th>Description</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Equatorial Diameter:</th>
      <td>6,792 km</td>
    </tr>
    <tr>
      <th>Polar Diameter:</th>
      <td>6,752 km</td>
    </tr>
    <tr>
      <th>Mass:</th>
      <td>6.42 x 10^23 kg (10.7% Earth)</td>
    </tr>
    <tr>
      <th>Moons:</th>
      <td>2 (Phobos &amp; Deimos)</td>
    </tr>
    <tr>
      <th>Orbit Distance:</th>
      <td>227,943,824 km (1.52 AU)</td>
    </tr>
    <tr>
      <th>Orbit Period:</th>
      <td>687 days (1.9 years)</td>
    </tr>
    <tr>
      <th>Surface Temperature:</th>
      <td>-153 to 20 °C</td>
    </tr>
    <tr>
      <th>First Record:</th>
      <td>2nd millennium BC</td>
    </tr>
    <tr>
      <th>Recorded By:</th>
      <td>Egyptian astronomers</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Convert dataframe to html table
html_table = df.to_html()
html_table
```




    '<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th>Value</th>\n    </tr>\n    <tr>\n      <th>Description</th>\n      <th></th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>Equatorial Diameter:</th>\n      <td>6,792 km</td>\n    </tr>\n    <tr>\n      <th>Polar Diameter:</th>\n      <td>6,752 km</td>\n    </tr>\n    <tr>\n      <th>Mass:</th>\n      <td>6.42 x 10^23 kg (10.7% Earth)</td>\n    </tr>\n    <tr>\n      <th>Moons:</th>\n      <td>2 (Phobos &amp; Deimos)</td>\n    </tr>\n    <tr>\n      <th>Orbit Distance:</th>\n      <td>227,943,824 km (1.52 AU)</td>\n    </tr>\n    <tr>\n      <th>Orbit Period:</th>\n      <td>687 days (1.9 years)</td>\n    </tr>\n    <tr>\n      <th>Surface Temperature:</th>\n      <td>-153 to 20 °C</td>\n    </tr>\n    <tr>\n      <th>First Record:</th>\n      <td>2nd millennium BC</td>\n    </tr>\n    <tr>\n      <th>Recorded By:</th>\n      <td>Egyptian astronomers</td>\n    </tr>\n  </tbody>\n</table>'




```python
# Clean up html table
html_table_clean = html_table.replace('\n', '')
html_table_clean
```




    '<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Value</th>    </tr>    <tr>      <th>Description</th>      <th></th>    </tr>  </thead>  <tbody>    <tr>      <th>Equatorial Diameter:</th>      <td>6,792 km</td>    </tr>    <tr>      <th>Polar Diameter:</th>      <td>6,752 km</td>    </tr>    <tr>      <th>Mass:</th>      <td>6.42 x 10^23 kg (10.7% Earth)</td>    </tr>    <tr>      <th>Moons:</th>      <td>2 (Phobos &amp; Deimos)</td>    </tr>    <tr>      <th>Orbit Distance:</th>      <td>227,943,824 km (1.52 AU)</td>    </tr>    <tr>      <th>Orbit Period:</th>      <td>687 days (1.9 years)</td>    </tr>    <tr>      <th>Surface Temperature:</th>      <td>-153 to 20 °C</td>    </tr>    <tr>      <th>First Record:</th>      <td>2nd millennium BC</td>    </tr>    <tr>      <th>Recorded By:</th>      <td>Egyptian astronomers</td>    </tr>  </tbody></table>'



### Mars Hemispheres


```python
# URL of page to be scraped
url = 'https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars'

# Retrieve page with the requests module
response = requests.get(url)

# Create BeautifulSoup object; parse with 'html.parser'
soup = BeautifulSoup(response.text, 'html.parser')

# Find all title links featured    
results = soup.find_all("div", class_="description")


for result in results:
    
    print(result.text)
```

    Cerberus Hemisphere Enhanced
    Schiaparelli Hemisphere Enhanced
    Syrtis Major Hemisphere Enhanced
    Valles Marineris Hemisphere Enhanced



```python
# Empty list of image urls
hemisphere_image_urls = []

# Define function to collect data from all title links
def scrape_usgs(link):
    
    # Initialize browser
    browser = init_browser()
    
    # Setup URL to usgs
    url = 'https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars'
    browser.visit(url)
    
    # Move browser to featured image page
    browser.click_link_by_partial_text(link)
    time.sleep(5)
    
    # Create BeautifulSoup object; parse with 'html.parser'
    usgs_html = browser.html
    soup = BeautifulSoup(usgs_html, 'html.parser')
    
    # Get usgs image
    img_url = soup.find('img', class_='wide-image')['src']
    usgs_url = "https://astrogeology.usgs.gov"
    usgs_image_url = usgs_url + img_url
    
    print(link)
    print(usgs_image_url)
    
    # Create a dictionary to return 
    d = dict();
    
    d['title'] = link
    d['img_url']   = usgs_image_url
   
    # Close browser
    browser.quit()
    
    # Return data dic to main program
    return d

# Loop throught all title links and collect data
for result in results:
    temp = scrape_usgs(result.text)
    hemisphere_image_urls.append(temp)
    time.sleep(5)
    
print("-------------------------------------------------------------------------------------------")    
print(hemisphere_image_urls)
```

    /usr/local/bin/chromedriver
    Cerberus Hemisphere Enhanced
    https://astrogeology.usgs.gov/cache/images/cfa62af2557222a02478f1fcd781d445_cerberus_enhanced.tif_full.jpg
    /usr/local/bin/chromedriver
    Schiaparelli Hemisphere Enhanced
    https://astrogeology.usgs.gov/cache/images/3cdd1cbf5e0813bba925c9030d13b62e_schiaparelli_enhanced.tif_full.jpg
    /usr/local/bin/chromedriver

