---
template: blog_post.html
title: Home Server - pyload
description: Home Server | pyload | إدارة تحميل الملفات
date: 2023-02-14
---

# <div dir="rtl">تطبيق pyload لتحميل ملفات الانترنت على السيرفر المنزلي</div>

![type:video](https://www.youtube.com/embed/04tRhBVwatQ)

<div dir="rtl">
خدمة تحميل الملفات على السيرفر المنزلي عبر تطبيق pyload، الذي يتميز بأداء ثابت واقتصادي في استهلاك موارد السيرفر.
</div>
<div dir="rtl">
تقوم الخدمة بمتابعة وإدارة تحميل الملفات بشكل منفصل تمامًا عن الكمبيوتر الشخصي، أستخدم الخدمة بشكل أساسي لتحميل الملفات الكبيرة.
</div>
<div dir="rtl">
نشرح في الفيديو الخدمة بداية من التنصيب مرورًا بالضبط وحتى طريقة الاستخدام.
</div>

<p hidden>#more</p>

## <div dir="rtl">إعداد مجلد التحميل الأساسي</div>

```sh
sudo mkdir /mnt/srv/downloads
```

``` sh
sudo chown $USER:$USER /mnt/srv/downloads
```

## <div dir="rtl">إعداد مجلدات الخدمة</div>

```sh
mkdir -p /mnt/srv/downloads/pyload /mnt/srv/docker/cont/pyload
```

## <div dir="rtl">إضافة الخدمة إلى docker-compose file</div>

<div dir="rtl">بالأمر:</div>

```sh
nano /mnt/srv/docker/comp/docker-compose.yml
```

<div dir="rtl">وإضافة ما يلي:</div>

``` yaml title="docker-compose.yml"

 # =====================================
  pyload:
    image: lscr.io/linuxserver/pyload-ng:latest
    container_name: pyload
    networks:
      - hs
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Africa/Cairo
    volumes:
      - /mnt/srv/docker/cont/pyload:/config
      - /mnt/srv/downloads/pyload:/downloads
    ports:
      - 8001:8000
      - 9666:9666 #optional
    restart: unless-stopped

```

## <div dir="rtl">إنشاء وتشغيل الخدمة</div>

```sh
docker compose -f /mnt/srv/docker/comp/docker-compose.yml up -d
```

## <div dir="rtl">ارسال روابط الملفات من المتصفح باستخدام pyload-bookmarklet</div>

<div dir="rtl">إضافة رابط مرجعي جديد بالمحتوى التالي:</div>

!!! danger "بياناتك"

    <div dir="rtl">تأكد من تعديل رابط الخدمة واسم المستخدم وكلمة المرور في نهاية النص</div>

``` js
javascript:"use strict";(function(A,B,C,u){function D(){for(var c=[],d={},b=0;b<m.document.links.length;b++){var a=m.document.links[b];a.href.match(/^(?:mailto:|javascript:|data:)/)||d[a.href]||(d[a.href]=!0,c.push({href:a.href,name:a.text.replace(/(<([^>]+)>)/ig,"").trim()}))}return c}function x(){var c=e.document.getElementById("searchInput").value;if(c)var d="<h4>Filtered links: &quot;"+c+"&quot;</h4>";else c="",d="<h4>Links:</h4>";c=c.toLocaleLowerCase();d+='<div id="container" class="form-group form-check"><input type="checkbox" id="checkall" class="form-check-input"><label for="checkall">All</label><div class="form-check">';for(var b=0;b<k.length;b++)if(-1!==k[b].href.toLocaleLowerCase().indexOf(c)||-1!==k[b].name.toLocaleLowerCase().indexOf(c))d+='<div class="form-check"><input class="form-check-input" type="checkbox"'+(n[b]?" checked":"")+' id="link'+b+'"><label class="form-check-label small" for="link'+b+'">'+(k[b].name?'<span class="badge badge-warning">'+k[b].name+"</span>&nbsp;":"")+'<a href="'+k[b].href+'">'+k[b].href+"</a></label></div>";e.document.getElementById("divLinks").innerHTML=d+"</div></div>"}function v(c,d){var b=e.document.getElementById("link"+c);b&&(b.checked=d);p+=(n[c]===d?0:1)*(d?1:-1);n[c]=d;b=e.document.getElementById("checkall");b.checked=0<p;b.indeterminate=0<p&&p<n.length}function w(){var c=0===p&&!e.document.getElementById("checkurl").checked;e.document.getElementById("toClipboard").disabled=c;e.document.getElementById("toPyload").disabled=c||0===e.document.getElementById("packagename").value.length}function E(){var c=[],d=e.document.getElementById("packagename").value,b=e.document.getElementById("serveraddr").value;e.document.getElementById("checkurl").checked&&c.push(r);for(var a=0;a<n.length;a++)n[a]&&c.push(k[a].href);if(1===u)var f=y(b+"/flash/add",{name:d,urls:c.join("\n")});else 2===u?f=window.open(b+'/api/addPackage?name="'+d+'"&links='+encodeURIComponent(JSON.stringify(c)),"","resizable=no, location=no, width=100, height=100, menubar=no, status=no, scrollbars=no, menubar=no"):3===u&&(JSON.stringify(c),f=y(b+"/api/addPackage",{name:JSON.stringify(d),links:JSON.stringify(c),a:B,p:C}));setTimeout(function(){f.close()},1E3);e.close()}function y(c,d){var b=document.createElement("form");b.style.display="none";b.target="_Pyload";b.method="POST";b.action=c;for(var a in d)if(d.hasOwnProperty(a)){var f=document.createElement("input");f.type="text";f.name=a;f.value=d[a];b.appendChild(f)}e.document.body.appendChild(b);f=e.open("","_Pyload","resizable=no, location=no, width=100, height=100, menubar=no, status=no, scrollbars=no, menubar=no");b.submit();return f}var m=window;var e=window.open("","_blank","width="+screen.availWidth+",height="+screen.availHeight+",scrollbars,resizable,menubar");e.moveTo(0,0);var k=function(){var c=[],d={},b=(m.getSelection?m.getSelection():m.document.getSelection?m.document.getSelection():m.document.selection?m.document.selection.createRange().text:"").toString();if(""!==b){b=b.match(/(?:ht|f)tp(?:s?):\/\/[a-zA-Z0-9-./?=_&%#:]+(?:[<%20"'\r\n\t]|$)/ig)||[];for(var%20a=0;a<b.length;a++){var%20f=b[a].replace(/[<%20"'\r\n\t]/g,"");d[f]||(d[f]=!0,c.push({href:f,name:""}))}}return%20c}();k.length||(k=D());var%20n=Array.apply(null,Array(k.length)).map(Boolean.prototype.valueOf,!1);var%20p=0;var%20F=function(){var%20c=[],d=document.createElement("a");k.forEach(function(b){d.href=b.href;b=d.hostname.match(/[^.]+\.(?:\w{3,}|\w{2,3}\.\w{2})$/);null!==b&&0>c.indexOf(b[0].toLowerCase())&&c.push(b[0].toLowerCase())});return%20c}();var%20r=m.document.URL;(function(){var%20c=document.createElement("link");c.setAttribute("href","https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css");c.setAttribute("rel","stylesheet");e.document.head.appendChild(c);c=document.createElement("div");var%20d=document.createElement("div");d.innerHTML="<h4>Package%20Name:</h4>";var%20b=document.createElement("div");b.setAttribute("class","input-group");var%20a=document.createElement("input");a.setAttribute("id","packagename");a.setAttribute("type","text");a.setAttribute("size","70");a.setAttribute("class","form-control%20form-control-sm");a.setAttribute("value",m.document.title);a.setAttribute("placeholder","Package%20Name");a.oninput=function(){w()};b.appendChild(a);var%20f=document.createElement("div");f.setAttribute("class","input-group-append");a=document.createElement("button");a.setAttribute("id","toPyload");a.setAttribute("disabled","");a.setAttribute("class","btn%20btn-primary%20btn-sm");a.setAttribute("style","margin-left:%205px;%20border-radius:%20.25rem;");a.innerHTML="Send%20to%20pyLoad";a.onclick=function(){E()};f.appendChild(a);a=document.createElement("button");a.setAttribute("id","toClipboard");a.setAttribute("disabled","");a.setAttribute("class","btn%20btn-secondary%20btn-sm");a.setAttribute("style","margin-left:%205px;%20border-radius:%20.25rem;");a.innerHTML="&#128203;";a.setAttribute("title","Copy%20links%20to%20clipboard");a.onclick=function(){for(var%20h=e.document.getElementById("checkurl").checked?[r]:[],g=0;g<n.length;g++)n[g]&&h.push(k[g].href);g=document.createElement("textarea");g.style.fontSize="12pt";g.style.border="0";g.style.padding="0";g.style.margin="0";g.style.position="absolute";g.style.left="-9999px";g.style.top=(e.pageYOffset||e.document.documentElement.scrollTop)+"px";g.setAttribute("readonly","");g.value=h.join("\n");e.document.body.appendChild(g);g.select();try{var%20q=e.document.execCommand("copy");e.alert("Copy%20"+(q?"successful":"unsuccessful"))}catch(t){e.alert("Oops,%20unable%20to%20copy"+t)}e.document.body.removeChild(g)};f.appendChild(a);a=document.createElement("button");a.setAttribute("id","configtoggle");a.setAttribute("class","btn%20btn-secondary%20btn-sm");a.setAttribute("style","margin-left:%205px;%20border-radius:%20.25rem;");a.innerHTML="&#9881;";a.onclick=function(){var%20h=e.document.getElementById("config");h.style.display="block"===h.style.display?"none":"block"};f.appendChild(a);b.appendChild(f);d.appendChild(b);c.appendChild(d);a=document.createElement("fieldset");a.setAttribute("id","config");a.style.display="none";d=(d="undefined"!==typeof%20e.Storage?e.localStorage.getItem("pyloadServer"):"")||A;a.innerHTML="<h5>Configuration</h5><label%20for='serveraddr'>pyLoad's%20address:</label><div%20class='input-group'><input%20type='text'%20class='form-control%20form-control-sm'%20id='serveraddr'%20placeholder='scheme://address:port'%20value='"+d+"'%20/><div%20class='input-group-append'><button%20id='save-config'%20class='btn%20btn-danger%20btn-sm'>Save</button></div></div>";a.querySelector("#save-config").onclick=function(){var%20h=e.document.getElementById("serveraddr").value;0<h.length&&(e.localStorage.setItem("pyloadServer",h),e.alert('Saved%20for%20domain%20"'+window.top.location.hostname+'"'))};c.appendChild(a);d=document.createElement("div");d.innerHTML='<hr%20style="color:lightgrey;"><h4>Page%20URL:</h4><div%20class="form-group%20form-check%20text-nowrap"><input%20type="checkbox"%20class="form-check-input"%20id="checkurl"><label%20for="checkurl"><a%20href="'+r+'">'+r+'</a></label></div><hr%20style="color:lightgrey;">';b=document.createElement("div");b.innerHTML='<fieldset%20class="form-inline"><input%20id="searchInput"%20type="text"%20class="form-control%20form-control-sm%20mr-2"%20placeholder="Filter%20links..."%20style="width:%2060%;"%20/><select%20id="selectDomain"%20class="form-control%20form-control-sm"><option>Or%20check%20domain...</option></select></fieldset>';var%20G=b.querySelector("#selectDomain");F.forEach(function(h){var%20g=document.createElement("option");g.text=h;g.onclick=function(){var%20q=[],t=!1;var%20l=e.document.getElementById("selectDomain").value;if(""!==l){var%20H=new%20RegExp("(?://|\\.)"+l.replace(".","\\.")+"(?:[?/]|$)","i");for(l=0;l<k.length;l++){var%20z=k[l].href.match(/:\/\/[^/?]+(?:[?/]|$)/g);z&&z[0].match(H)&&(q.push(l),!1===n[l]&&(t=!0))}for(l=0;l<q.length;l++)v(q[l],t);w()}};G.appendChild(g)});a=document.createElement("div");a.setAttribute("id","divLinks");f=document.createElement("div");f.setAttribute("class","container-fluid");f.appendChild(c);f.appendChild(d);f.appendChild(b);f.appendChild(a);e.document.body.appendChild(f);e.document.title="Send%20to%20pyLoad";e.document.getElementById("searchInput").oninput=function(){x()};e.document.addEventListener("click",function(h){if(h.target&&"checkbox"===h.target.type){if(0===h.target.id.indexOf("link")){var%20g=parseInt(h.target.id.replace("link",""),10);v(g,h.target.checked)}else%20if("checkall"===h.target.id)for(g=p<n.length,h.target.checked=g,h.target.indeterminate=!1,h=0;h<k.length;h++)v(h,g);w()}else"A"===h.target.nodeName&&h.preventDefault()});x()})()})%20("http://SERVER-IP:PORT","USER","PASSWORD",2);
```

## <div dir="rtl">مراجع</div>

- [pyload Docker Image](https://hub.docker.com/r/linuxserver/pyload-ng)
- [pyload-bookmarklet](https://github.com/pyload/pyload-bookmarklet)
- [pyload on Github](https://github.com/pyload/pyload)
- [pyload on PyPI](https://pypi.org/project/pyload-ng)