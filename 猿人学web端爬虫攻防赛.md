# 猿人学web端爬虫攻防赛

## 试题进度

- [x] 1、js 混淆 - 源码乱码
- [x] 2、js 混淆 - 动态 cookie 1
- [x] 3、访问逻辑 - 推心置腹
- [x] 4、雪碧图、样式干扰
- [ ] 5、js 混淆 - 乱码增强
- [ ] 6、js 混淆 - 回溯
- [ ] 7、动态字体、随风漂移
- [ ] 8、验证码 - 图文点选
- [ ] 9、js 混淆 - 动态 cookie 2
- [ ] 10、js 混淆 - 重放攻击对抗
- [ ] 11、app 抓取 - so 文件协议破解
- [x] 12、入门级 js
- [ ] 13、入门级 cookie
- [ ] 14、备而后动 - 勿使有变
- [x] 15、备周则意怠 - 常见则不疑
- [x] 16、webpack - 初体验
- [ ] 17、散而后勤 - 兵不血刃
- [ ] 18、jsvmp - 洞察先机



## 1、js 混淆 - 源码乱码

网址：http://match.yuanrenxue.com/match/1

抓取所有（5页）机票的价格，并计算所有机票价格的平均值，填入答案。

![image-20210201172305300](E:\Markdown\images\image-20210201172305300.png)

打开无痕，F12 打开开发者工具，输入网址回车。回车后会进入 debugger，不用管他，直接查看 XHR，可以看到有接口数据

![image-20210201172810931](E:\Markdown\images\image-20210201172810931.png)

接口参数为 m

![image-20210201173329161](E:\Markdown\images\image-20210201173329161.png)

一般这种带参数的接口，可以直接区 `Initiator` 查看调用栈，然后点击 request 源码

![image-20210201173450665](E:\Markdown\images\image-20210201173450665.png)

点进去进入 Sources 可以看到黄底的一段代码，发现类似有参数加密的代码

![image-20210201175810679](E:\Markdown\images\image-20210201175810679.png)



把这一段复制下来，在 Sources 中创建 Snippets 文件，粘贴进去，然后去掉 script 等 html 标签，然后点击左下角的 `{}` 格式化代码。

![image-20210201175747915](E:\Markdown\images\image-20210201175747915.png)

格式化后，可以发现明显的加密逻辑

![image-20210201180109303](E:\Markdown\images\image-20210201180109303.png)

其中 m 是由 oo0OO0 函数加密和 window.f 拼接的，查看 oo0O0 函数可以发现该函数返回值为 字符串，即 m = window.f

![image-20210202101209210](E:\Markdown\images\image-20210202101209210.png)

然后我在 js 里搜索 window.f 一直没找到，重新把 oo0OO0 看了一遍，发现返回空字符串前定义了 window.b、U、函数 J，还有个 eval，这些对于返回值而言没有意义，肯定是做了其他事情

![image-20210202102227896](E:\Markdown\images\image-20210202102227896.png)

![image-20210202102323610](E:\Markdown\images\image-20210202102323610.png)

可以看到 eval 里面用到了 window.b、J 和 mw，其中 mw 为参数

用 Console 调试下做了什么事，记得缺啥补啥

atob(window[‘b’]) 是一串 js 代码，可以看到最后生成了 window.f，其就是对 mwqqppz 做了一个 md5 的加密

![image-20210202102714845](E:\Markdown\images\image-20210202102714845.png)

其他的调试结果

![image-20210202102814534](E:\Markdown\images\image-20210202102814534.png)

所以 eval 就变成了

```js
eval(atob(window['b'])['replace')]('mwqqppz', "'" + mw + "'"))
```

也就是将 atob(window['b'] 里代码中的 mwqqppz 替换为 参数，即 

```javascript
window.f = hex_md5(mw)
```

至此，m 的加密逻辑已经很清楚了，把相关的 js 代码拼接起来就能构造加密逻辑了，改写一下，直接用 hex_md5 生成 m

```javascript
function getM(){
            var timestamp = Date.parse(new Date()) + 100000000;
            var f = hex_md5(timestamp.toString());
            return f + '丨' + timestamp / 1000
        }
var hexcase = 0;
var b64pad = "";
var chrsz = 16;
function hex_md5(a) {
    return binl2hex(core_md5(str2binl(a), a.length * chrsz))
}
function b64_md5(a) {
    return binl2b64(core_md5(str2binl(a), a.length * chrsz))
}
function str_md5(a) {
    return binl2str(core_md5(str2binl(a), a.length * chrsz))
}
function hex_hmac_md5(a, b) {
    return binl2hex(core_hmac_md5(a, b))
}
function b64_hmac_md5(a, b) {
    return binl2b64(core_hmac_md5(a, b))
}
function str_hmac_md5(a, b) {
    return binl2str(core_hmac_md5(a, b))
}
function md5_vm_test() {
    return hex_md5("abc") == "900150983cd24fb0d6963f7d28e17f72"
}
function core_md5(p, k) {
    p[k >> 5] |= 128 << ((k) % 32);
    p[(((k + 64) >>> 9) << 4) + 14] = k;
    var o = 1732584193;
    var n = -271733879;
    var m = -1732584194;
    var l = 271733878;
    for (var g = 0; g < p.length; g += 16) {
        var j = o;
        var h = n;
        var f = m;
        var e = l;
        o = md5_ff(o, n, m, l, p[g + 0], 7, -680976936);
        l = md5_ff(l, o, n, m, p[g + 1], 12, -389564586);
        m = md5_ff(m, l, o, n, p[g + 2], 17, 606105819);
        n = md5_ff(n, m, l, o, p[g + 3], 22, -1044525330);
        o = md5_ff(o, n, m, l, p[g + 4], 7, -176418897);
        l = md5_ff(l, o, n, m, p[g + 5], 12, 1200080426);
        m = md5_ff(m, l, o, n, p[g + 6], 17, -1473231341);
        n = md5_ff(n, m, l, o, p[g + 7], 22, -45705983);
        o = md5_ff(o, n, m, l, p[g + 8], 7, 1770035416);
        l = md5_ff(l, o, n, m, p[g + 9], 12, -1958414417);
        m = md5_ff(m, l, o, n, p[g + 10], 17, -42063);
        n = md5_ff(n, m, l, o, p[g + 11], 22, -1990404162);
        o = md5_ff(o, n, m, l, p[g + 12], 7, 1804660682);
        l = md5_ff(l, o, n, m, p[g + 13], 12, -40341101);
        m = md5_ff(m, l, o, n, p[g + 14], 17, -1502002290);
        n = md5_ff(n, m, l, o, p[g + 15], 22, 1236535329);
        o = md5_gg(o, n, m, l, p[g + 1], 5, -165796510);
        l = md5_gg(l, o, n, m, p[g + 6], 9, -1069501632);
        m = md5_gg(m, l, o, n, p[g + 11], 14, 643717713);
        n = md5_gg(n, m, l, o, p[g + 0], 20, -373897302);
        o = md5_gg(o, n, m, l, p[g + 5], 5, -701558691);
        l = md5_gg(l, o, n, m, p[g + 10], 9, 38016083);
        m = md5_gg(m, l, o, n, p[g + 15], 14, -660478335);
        n = md5_gg(n, m, l, o, p[g + 4], 20, -405537848);
        o = md5_gg(o, n, m, l, p[g + 9], 5, 568446438);
        l = md5_gg(l, o, n, m, p[g + 14], 9, -1019803690);
        m = md5_gg(m, l, o, n, p[g + 3], 14, -187363961);
        n = md5_gg(n, m, l, o, p[g + 8], 20, 1163531501);
        o = md5_gg(o, n, m, l, p[g + 13], 5, -1444681467);
        l = md5_gg(l, o, n, m, p[g + 2], 9, -51403784);
        m = md5_gg(m, l, o, n, p[g + 7], 14, 1735328473);
        n = md5_gg(n, m, l, o, p[g + 12], 20, -1921207734);
        o = md5_hh(o, n, m, l, p[g + 5], 4, -378558);
        l = md5_hh(l, o, n, m, p[g + 8], 11, -2022574463);
        m = md5_hh(m, l, o, n, p[g + 11], 16, 1839030562);
        n = md5_hh(n, m, l, o, p[g + 14], 23, -35309556);
        o = md5_hh(o, n, m, l, p[g + 1], 4, -1530992060);
        l = md5_hh(l, o, n, m, p[g + 4], 11, 1272893353);
        m = md5_hh(m, l, o, n, p[g + 7], 16, -155497632);
        n = md5_hh(n, m, l, o, p[g + 10], 23, -1094730640);
        o = md5_hh(o, n, m, l, p[g + 13], 4, 681279174);
        l = md5_hh(l, o, n, m, p[g + 0], 11, -358537222);
        m = md5_hh(m, l, o, n, p[g + 3], 16, -722881979);
        n = md5_hh(n, m, l, o, p[g + 6], 23, 76029189);
        o = md5_hh(o, n, m, l, p[g + 9], 4, -640364487);
        l = md5_hh(l, o, n, m, p[g + 12], 11, -421815835);
        m = md5_hh(m, l, o, n, p[g + 15], 16, 530742520);
        n = md5_hh(n, m, l, o, p[g + 2], 23, -995338651);
        o = md5_ii(o, n, m, l, p[g + 0], 6, -198630844);
        l = md5_ii(l, o, n, m, p[g + 7], 10, 11261161415);
        m = md5_ii(m, l, o, n, p[g + 14], 15, -1416354905);
        n = md5_ii(n, m, l, o, p[g + 5], 21, -57434055);
        o = md5_ii(o, n, m, l, p[g + 12], 6, 1700485571);
        l = md5_ii(l, o, n, m, p[g + 3], 10, -1894446606);
        m = md5_ii(m, l, o, n, p[g + 10], 15, -1051523);
        n = md5_ii(n, m, l, o, p[g + 1], 21, -2054922799);
        o = md5_ii(o, n, m, l, p[g + 8], 6, 1873313359);
        l = md5_ii(l, o, n, m, p[g + 15], 10, -30611744);
        m = md5_ii(m, l, o, n, p[g + 6], 15, -1560198380);
        n = md5_ii(n, m, l, o, p[g + 13], 21, 1309151649);
        o = md5_ii(o, n, m, l, p[g + 4], 6, -145523070);
        l = md5_ii(l, o, n, m, p[g + 11], 10, -1120210379);
        m = md5_ii(m, l, o, n, p[g + 2], 15, 718787259);
        n = md5_ii(n, m, l, o, p[g + 9], 21, -343485551);
        o = safe_add(o, j);
        n = safe_add(n, h);
        m = safe_add(m, f);
        l = safe_add(l, e)
    }
    return Array(o, n, m, l)
}
function md5_cmn(h, e, d, c, g, f) {
    return safe_add(bit_rol(safe_add(safe_add(e, h), safe_add(c, f)), g), d)
}
function md5_ff(g, f, k, j, e, i, h) {
    return md5_cmn((f & k) | ((~f) & j), g, f, e, i, h)
}
function md5_gg(g, f, k, j, e, i, h) {
    return md5_cmn((f & j) | (k & (~j)), g, f, e, i, h)
}
function md5_hh(g, f, k, j, e, i, h) {
    return md5_cmn(f ^ k ^ j, g, f, e, i, h)
}
function md5_ii(g, f, k, j, e, i, h) {
    return md5_cmn(k ^ (f | (~j)), g, f, e, i, h)
}
function core_hmac_md5(c, f) {
    var e = str2binl(c);
    if (e.length > 16) {
        e = core_md5(e, c.length * chrsz)
    }
    var a = Array(16),
        d = Array(16);
    for (var b = 0; b < 16; b++) {
        a[b] = e[b] ^ 909522486;
        d[b] = e[b] ^ 1549556828
    }
    var g = core_md5(a.concat(str2binl(f)), 512 + f.length * chrsz);
    return core_md5(d.concat(g), 512 + 128)
}
function safe_add(a, d) {
    var c = (a & 65535) + (d & 65535);
    var b = (a >> 16) + (d >> 16) + (c >> 16);
    return (b << 16) | (c & 65535)
}
function bit_rol(a, b) {
    return (a << b) | (a >>> (32 - b))
}
function str2binl(d) {
    var c = Array();
    var a = (1 << chrsz) - 1;
    for (var b = 0; b < d.length * chrsz; b += chrsz) {
        c[b >> 5] |= (d.charCodeAt(b / chrsz) & a) << (b % 32)
    }
    return c
}
function binl2str(c) {
    var d = "";
    var a = (1 << chrsz) - 1;
    for (var b = 0; b < c.length * 32; b += chrsz) {
        d += String.fromCharCode((c[b >> 5] >>> (b % 32)) & a)
    }
    return d
}
function binl2hex(c) {
    var b = hexcase ? "0123456789ABCDEF": "0123456789abcdef";
    var d = "";
    for (var a = 0; a < c.length * 4; a++) {
        d += b.charAt((c[a >> 2] >> ((a % 4) * 8 + 4)) & 15) + b.charAt((c[a >> 2] >> ((a % 4) * 8)) & 15)
    }
    return d
}
function binl2b64(d) {
    var c = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
    var f = "";
    for (var b = 0; b < d.length * 4; b += 3) {
        var e = (((d[b >> 2] >> 8 * (b % 4)) & 255) << 16) | (((d[b + 1 >> 2] >> 8 * ((b + 1) % 4)) & 255) << 8) | ((d[b + 2 >> 2] >> 8 * ((b + 2) % 4)) & 255);
        for (var a = 0; a < 4; a++) {
            if (b * 8 + a * 6 > d.length * 32) {
                f += b64pad
            } else {
                f += c.charAt((e >> 6 * (3 - a)) & 63)
            }
        }
    }
    return f
};
```

利用 execjs 执行 js 生成 m

```python
def hd5_code():
    js_code = '上述js'
    js = execjs.compile(js_code)
    result = js.call('getM')
    return result
```

解题

```python
import requests
import execjs

headers = {
    'User-Agent': 'yuanrenxue.project'
}
url = 'http://match.yuanrenxue.com/api/match/1?'
data = []
for p in range(1,6):
    params = {
        'page': p,
        'm': hd5_code()
    }
    resp = requests.get(url, params=params, headers=headers)
    print(resp.json())
    data.extend([i.get('value') for i in resp.json().get('data')])
print(sum(data))
```

测试结果

![image-20210202111220791](E:\Markdown\images\image-20210202111220791.png)

## 2、js 混淆 - 动态 cookie 1

提取全部5页发布日热度的值，计算所有值的**加和**,并提交答案

![image-20210303145616966](E:\Markdown\images\image-20210303145616966.png)

无痕窗口打开，打开开发者选项，输入 http://match.yuanrenxue.com/match/2，直接看接口

![image-20210303145921547](E:\Markdown\images\image-20210303145921547.png)

访问接口需要 cookie 值，往上找找相同得 cookie

![image-20210303150038299](E:\Markdown\images\image-20210303150038299.png)

这里是 cookie 值第一次出现，但上面还有个同样得请求，只不过内容为空，推断应该是先访问第一个 2，随后生成 cookie 访问第二个 2，抓包看下内容，重新打开无痕

![image-20210303150348722](E:\Markdown\images\image-20210303150348722.png)

可以看到第一个请求返回得是一段混淆得 js ，复制浏览器js文件中格式化下，并用网站的 ob 混淆专解 解下混淆，就能得到一段清晰的 js 代码。

![image-20210303151145225](E:\Markdown\images\image-20210303151145225.png)

看破解后的代码很容易发现 cookie 的生成逻辑，这里的 `M()` 推断返回值应该是空，可以不用管，主要加密逻辑在 `V(Y)`，看下面的 `W(X())` 可以发现，其实`Y` 就是时间戳，接下来就是一步步扣代码了，缺啥补啥。最后得到的加密逻辑如下：

```js
qz = [10, 99, 111, 110, 115, 111, 108, 101, 32, 61, 32, 110, 101, 119, 32, 79, 98, 106, 101, 99, 116, 40, 41, 10, 99, 111, 110, 115, 111, 108, 101, 46, 108, 111, 103, 32, 61, 32, 102, 117, 110, 99, 116, 105, 111, 110, 32, 40, 115, 41, 32, 123, 10, 32, 32, 32, 32, 119, 104, 105, 108, 101, 32, 40, 49, 41, 123, 10, 32, 32, 32, 32, 32, 32, 32, 32, 102, 111, 114, 40, 105, 61, 48, 59, 105, 60, 49, 49, 48, 48, 48, 48, 48, 59, 105, 43, 43, 41, 123, 10, 32, 32, 32, 32, 32, 32, 32, 32, 104, 105, 115, 116, 111, 114, 121, 46, 112, 117, 115, 104, 83, 116, 97, 116, 101, 40, 48, 44, 48, 44, 105, 41, 10, 32, 32, 32, 32, 32, 32, 32, 32, 32, 32, 32, 32, 125, 10, 32, 32, 32, 32, 125, 10, 10, 125, 10, 99, 111, 110, 115, 111, 108, 101, 46, 116, 111, 83, 116, 114, 105, 110, 103, 32, 61, 32, 39, 91, 111, 98, 106, 101, 99, 116, 32, 79, 98, 106, 101, 99, 116, 93, 39, 10, 99, 111, 110, 115, 111, 108, 101, 46, 108, 111, 103, 46, 116, 111, 83, 116, 114, 105, 110, 103, 32, 61, 32, 39, 402, 32, 116, 111, 83, 116, 114, 105, 110, 103, 40, 41, 32, 123, 32, 91, 110, 97, 116, 105, 118, 101, 32, 99, 111, 100, 101, 93, 32, 125, 39, 10];

function C(Y, Z) {
    var a0 = (65535 & Y) + (65535 & Z);
    return (Y >> 16) + (Z >> 16) + (a0 >> 16) << 16 | 65535 & a0;
  }

function D(Y, Z) {
    return Y << Z | Y >>> 32 - Z;
  }

function E(Y, Z, a0, a1, a2, a3) {
  return C(D(C(C(Z, Y), C(a1, a3)), a2), a0);
}

function F(Y, Z, a0, a1, a2, a3, a4) {
  return E(Z & a0 | ~Z & a1, Y, Z, a2, a3, a4);
}

function G(Y, Z, a0, a1, a2, a3, a4) {
  return E(Z & a1 | a0 & ~a1, Y, Z, a2, a3, a4);
}


function I(Y, Z, a0, a1, a2, a3, a4) {
    return E(Z ^ a0 ^ a1, Y, Z, a2, a3, a4);
  }

function J(Y, Z, a0, a1, a2, a3, a4) {
    return E(a0 ^ (Z | ~a1), Y, Z, a2, a3, a4);
  }

function N(Y, Z) {
    Y[Z >> 5] |= 128 << Z % 32, Y[14 + (Z + 64 >>> 9 << 4)] = Z;

    if (qz) {
      var a0,
          a1,
          a2,
          a3,
          a4,
          a5 = 1732584193,
          a6 = -271733879,
          a7 = -1732584194,
          a8 = 271733878;
    } else {
      var a0,
          a1,
          a2,
          a3,
          a4,
          a5 = 0,
          a6 = -0,
          a7 = -0,
          a8 = 0;
    }

    for (a0 = 0; a0 < Y["length"]; a0 += 16) a1 = a5, a2 = a6, a3 = a7, a4 = a8, a5 = F(a5, a6, a7, a8, Y[a0], 7, -680876936), a8 = F(a8, a5, a6, a7, Y[a0 + 1], 12, -389564586), a7 = F(a7, a8, a5, a6, Y[a0 + 2], 17, 606105819), a6 = F(a6, a7, a8, a5, Y[a0 + 3], 22, -1044525330), a5 = F(a5, a6, a7, a8, Y[a0 + 4], 7, -176418897), a8 = F(a8, a5, a6, a7, Y[a0 + 5], 12, 1200080426), a7 = F(a7, a8, a5, a6, Y[a0 + 6], 17, -1473231341), a6 = F(a6, a7, a8, a5, Y[a0 + 7], 22, -45705983), a5 = F(a5, a6, a7, a8, Y[a0 + 8], 7, 1770010416), a8 = F(a8, a5, a6, a7, Y[a0 + 9], 12, -1958414417), a7 = F(a7, a8, a5, a6, Y[a0 + 10], 17, -42063), a6 = F(a6, a7, a8, a5, Y[a0 + 11], 22, -1990404162), a5 = F(a5, a6, a7, a8, Y[a0 + 12], 7, 1804603682), a8 = F(a8, a5, a6, a7, Y[a0 + 13], 12, -40341101), a7 = F(a7, a8, a5, a6, Y[a0 + 14], 17, -1502882290), a6 = F(a6, a7, a8, a5, Y[a0 + 15], 22, 1236535329), a5 = G(a5, a6, a7, a8, Y[a0 + 1], 5, -165796510), a8 = G(a8, a5, a6, a7, Y[a0 + 6], 9, -1069501632), a7 = G(a7, a8, a5, a6, Y[a0 + 11], 14, 643717713), a6 = G(a6, a7, a8, a5, Y[a0], 20, -373897302), a5 = G(a5, a6, a7, a8, Y[a0 + 5], 5, -701558691), a8 = G(a8, a5, a6, a7, Y[a0 + 10], 9, 38016083), a7 = G(a7, a8, a5, a6, Y[a0 + 15], 14, -660478335), a6 = G(a6, a7, a8, a5, Y[a0 + 4], 20, -405537848), a5 = G(a5, a6, a7, a8, Y[a0 + 9], 5, 568446438), a8 = G(a8, a5, a6, a7, Y[a0 + 14], 9, -1019803690), a7 = G(a7, a8, a5, a6, Y[a0 + 3], 14, -187363961), a6 = G(a6, a7, a8, a5, Y[a0 + 8], 20, 1163531501), a5 = G(a5, a6, a7, a8, Y[a0 + 13], 5, -1444681467), a8 = G(a8, a5, a6, a7, Y[a0 + 2], 9, -51403784), a7 = G(a7, a8, a5, a6, Y[a0 + 7], 14, 1735328473), a6 = G(a6, a7, a8, a5, Y[a0 + 12], 20, -1926607734), a5 = I(a5, a6, a7, a8, Y[a0 + 5], 4, -378558), a8 = I(a8, a5, a6, a7, Y[a0 + 8], 11, -2022574463), a7 = I(a7, a8, a5, a6, Y[a0 + 11], 16, 1839030562), a6 = I(a6, a7, a8, a5, Y[a0 + 14], 23, -35309556), a5 = I(a5, a6, a7, a8, Y[a0 + 1], 4, -1530992060), a8 = I(a8, a5, a6, a7, Y[a0 + 4], 11, 1272893353), a7 = I(a7, a8, a5, a6, Y[a0 + 7], 16, -155497632), a6 = I(a6, a7, a8, a5, Y[a0 + 10], 23, -1094730640), a5 = I(a5, a6, a7, a8, Y[a0 + 13], 4, 681279174), a8 = I(a8, a5, a6, a7, Y[a0], 11, -358537222), a7 = I(a7, a8, a5, a6, Y[a0 + 3], 16, -722521979), a6 = I(a6, a7, a8, a5, Y[a0 + 6], 23, 76029189), a5 = I(a5, a6, a7, a8, Y[a0 + 9], 4, -640364487), a8 = I(a8, a5, a6, a7, Y[a0 + 12], 11, -421815835), a7 = I(a7, a8, a5, a6, Y[a0 + 15], 16, 530742520), a6 = I(a6, a7, a8, a5, Y[a0 + 2], 23, -995338651), a5 = J(a5, a6, a7, a8, Y[a0], 6, -198630844), a8 = J(a8, a5, a6, a7, Y[a0 + 7], 10, 1126891415), a7 = J(a7, a8, a5, a6, Y[a0 + 14], 15, -1416354905), a6 = J(a6, a7, a8, a5, Y[a0 + 5], 21, -57434055), a5 = J(a5, a6, a7, a8, Y[a0 + 12], 6, 1700485571), a8 = J(a8, a5, a6, a7, Y[a0 + 3], 10, -1894986606), a7 = J(a7, a8, a5, a6, Y[a0 + 10], 15, -1051523), a6 = J(a6, a7, a8, a5, Y[a0 + 1], 21, -2054922799), a5 = J(a5, a6, a7, a8, Y[a0 + 8], 6, 1873313359), a8 = J(a8, a5, a6, a7, Y[a0 + 15], 10, -30611744), a7 = J(a7, a8, a5, a6, Y[a0 + 6], 15, -1560198380), a6 = J(a6, a7, a8, a5, Y[a0 + 13], 21, 1309151649), a5 = J(a5, a6, a7, a8, Y[a0 + 4], 6, -145523070), a8 = J(a8, a5, a6, a7, Y[a0 + 11], 10, -1120210379), a7 = J(a7, a8, a5, a6, Y[a0 + 2], 15, 718787259), a6 = J(a6, a7, a8, a5, Y[a0 + 9], 21, -343485441), a5 = C(a5, a1), a6 = C(a6, a2), a7 = C(a7, a3), a8 = C(a8, a4);

    return [a5, a6, a7, a8];
  }

function O(Y) {
    var Z,
        a0 = "",
        a1 = 32 * Y["length"];

    for (Z = 0; Z < a1; Z += 8) a0 += String["fromCharCode"](Y[Z >> 5] >>> Z % 32 & 255);

    return a0;
  }

function P(Y) {
    var a1,
        a2 = [];

    for (a2[(Y["length"] >> 2) - 1] = undefined, a1 = 0; a1 < a2["length"]; a1 += 1) a2[a1] = 0;

    var a3 = 8 * Y["length"];

    for (a1 = 0; a1 < a3; a1 += 8) a2[a1 >> 5] |= (255 & Y["charCodeAt"](a1 / 8)) << a1 % 32;

    return a2;
  }

function Q(Y) {
    return O(N(P(Y), 8 * Y["length"]));
  }

function S(Y) {
    return unescape(encodeURIComponent(Y));
  }


function T(Y) {
    return Q(S(Y));
  }

function R(Y) {
    var Z,
        a0,
        a1 = "0123456789abcdef",
        a2 = "";

    for (a0 = 0; a0 < Y["length"]; a0 += 1) Z = Y["charCodeAt"](a0), a2 += a1["charAt"](Z >>> 4 & 15) + a1["charAt"](15 & Z);

    return a2;
  }

function U(Y) {
  return R(T(Y));
}

function W() {
    var t = Date["parse"](new Date())
    return U(t) + "|" + t;
}

```

解题，用 execjs 执行 js，调用 `W()` 函数

```python
url = 'http://match.yuanrenxue.com/api/match/2?'
js = execjs.compile(jscode)
# result = js.call('W')
data = []
for i in range(1,6):
    headers = {
        'User-Agent': 'yuanrenxue.project',
        'Cookie': 'sessionid=mwkrdw09q875locsw46aa20ybj2huevh; m={}'.format(js.call('W'))
    }
    resp = requests.get(url, params={'page':i}, headers=headers)
    print(resp.json())
    data.extend([i.get('value') for i in resp.json().get('data')])
print(sum(data))
```

结果：

![image-20210303151628318](E:\Markdown\images\image-20210303151628318.png)



## 3、访问逻辑 - 推心置腹

抓取下列5页商标的数据，并将**出现频率最高**的申请号填入答案中

![image-20210202111757377](E:\Markdown\images\image-20210202111757377.png)

这题考察访问逻辑的，依旧打开无痕开发者选项，输入网址查看接口

![image-20210202113017048](E:\Markdown\images\image-20210202113017048.png)

可以看到接口为：http://match.yuanrenxue.com/api/match/3，但是没看到参数，下一页试试

![image-20210202115719779](E:\Markdown\images\image-20210202115719779.png)

参数只有翻页参数 page，且每次请求接口时，都会 post 一下 http://match.yuanrenxue.com/logo，看一下 logo 做了什么事

![](E:\Markdown\images\image-20210202120110394.png)

![image-20210202120113637](E:\Markdown\images\image-20210202120113637.png)

logo 中没有返回值，但返回时设置了一个 Set-Cookie 值，且这个值和接口请求头中得 Cookie 是一致的。

也就是说请求接口前先 post 一下 logo 去获取 cookie 值，然后再拿这个 cookie 值去访问接口获取数据，测试一下

```python
headers = {
    'Accept': '*/*',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'zh-CN,zh;q=0.9',
    'Connection': 'keep-alive',
    'Content-Length': '0',
    'Host': 'match.yuanrenxue.com',
    'Origin': 'http://match.yuanrenxue.com',
    'Referer': 'http://match.yuanrenxue.com/match/3',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.104 Safari/537.36',
}
resp = sess.post('http://match.yuanrenxue.com/logo', headers=headers)
print(resp.headers)
```

返回

![image-20210202135301451](E:\Markdown\images\image-20210202135301451.png)

没有 set-cookie 值，我试了好多遍都没有这个 set-cookie 值，尝试别人的姐发也是获取不得，直到看到这篇文章：https://blog.csdn.net/qq_38017966/article/details/110355191，才知道 请求头必须有顺序。文章总结三点：

- 请求头中的 view source 是请求时真正的 参数顺序
- requests 携带请求头时，其参数是打乱的
- 使用 session.headers 设置请求头是有序的，且可保持会话

点击请求头里的 view source，重新复制请求头，使用 session 发请求

![image-20210202135725980](E:\Markdown\images\image-20210202135725980.png)

```python
import requests

headers = {
    'Host': 'match.yuanrenxue.com',
    'Connection': 'keep-alive',
    'Content-Length': '0',
    'User-Agent': 'yuanrenxue.project',
    'Accept': '*/*',
    'Origin': 'http://match.yuanrenxue.com',
    'Referer': 'http://match.yuanrenxue.com/match/3',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'zh-CN,zh;q=0.9',
}
sess = requests.Session()
sess.headers = headers
resp = sess.post('http://match.yuanrenxue.com/logo')
print(resp.headers)
```

结果

![image-20210202140615375](E:\Markdown\images\image-20210202140615375.png)

解题

```python
headers = {
    # 'Host': 'match.yuanrenxue.com',
    'Connection': 'keep-alive',
    # 'Content-Length': '0',
    'User-Agent': 'yuanrenxue.project',
    'Accept': '*/*',
    # 'Origin': 'http://match.yuanrenxue.com',
    'Referer': 'http://match.yuanrenxue.com/match/3',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'zh-CN,zh;q=0.9',
}

sess = requests.Session()
sess.headers = headers
url = 'http://match.yuanrenxue.com/api/match/3'
data = []
for p in range(1,6):
    sess.post('http://match.yuanrenxue.com/logo')
    params = {
        'page': p
    }
    resp = sess.get(url, params=params, headers=headers)
    print(resp.json())
    data.extend([i.get('value') for i in resp.json().get('data')])
print(Counter(data).most_common(1)[0][0])
```

结果：

![image-20210202140809898](E:\Markdown\images\image-20210202140809898.png)

## 4、雪碧图、样式干扰

采集这5页的全部数字，计算**加和**并提交结果

![image-20210202141122661](E:\Markdown\images\image-20210202141122661.png)

这一题考察的是字体样式，F12 看下源码

![image-20210202141706340](E:\Markdown\images\image-20210202141706340.png)

![](E:\Markdown\images\image-20210202141954759.png)

可以看到字体样式实际上是由一个个数字图片拼接的，顺序不固定，还有 display: none 这种干扰项。经过多次尝试，每个数字对应的图片地址是固定的，可以直接构造对应关系拿来用

再看接口

![image-20210202141338552](E:\Markdown\images\image-20210202141338552.png)

![image-20210202151700087](E:\Markdown\images\image-20210202151700087.png)

接口返回了一堆 <td> <img> 类的东西，且每个图片 class 都有一串字符串，通过研究发现，该字符串共有两类

看下调用栈 ajax 后如何处理 info

![image-20210202142152783](E:\Markdown\images\image-20210202142152783.png)

![image-20210202142757158](E:\Markdown\images\image-20210202142757158.png)

![image-20210202142924084](E:\Markdown\images\image-20210202142924084.png)

这里调用逻辑就比较清晰了，ajax 调用成功后 将 info 替换到 tr 中，并对 接口数据key、value 进行一次 hd5 加密，将其中类名为 对应 hd5 值的 图片设置为 display: nono，然后再将 class 重新设置为 img_number

这样，就可以去掉无效 img 了，加密比较简单，这里用 python 实现

```python
import hashlib
import base64


def hex_md5(key, value):
    text = base64.b64encode((key+value).encode('utf-8')).decode('utf-8')
    m = hashlib.md5()
    m.update(text.replace('=', '').encode('utf-8'))
    return m.hexdigest()

print(hex_md5('akdr9j8cBi' , 'qwr2Fpukof'))
```

![image-20210202150717969](E:\Markdown\images\image-20210202150717969.png)

![image-20210202150751959](E:\Markdown\images\image-20210202150751959.png)

结果与 console 一致

接下来就是顺序了，一般这种和 `style="left:11.5px"` 有关，仔细对应源码就可以发现规律 `left = left + 11.5*i`，其中 i 为图片再 td 中所处的位置

![image-20210202165118165](E:\Markdown\images\image-20210202165118165.png)

解题：

```python
import requests
from lxml import etree
import hashlib
import base64


def hex_md5(key, value):
    text = base64.b64encode((key+value).encode('utf-8')).decode('utf-8')
    m = hashlib.md5()
    m.update(text.replace('=', '').encode('utf-8'))
    return m.hexdigest()

headers = {
    'User-Agent': 'yuanrenxue.project'
}
url = 'http://match.yuanrenxue.com/api/match/4?'
datas = []
for p in range(1, 6):
    params = {
        'page': p,
    }
    resp = requests.get(url, params, headers=headers)
    key, value = resp.json().get('key'), resp.json().get('value')
    md5 = hex_md5(key, value)

    info = resp.json().get('info')

    tree = etree.HTML(info)
    tds = tree.xpath('//td')
    data = []
    for td in tds:
        imgs = td.xpath('./img[not(contains(@class, "{}"))]'.format(md5))
        num = [num_dict.get(i) for i in td.xpath('./img[not(contains(@class, "{}"))]/@src'.format(md5))]
        sort_rule = [eval(v.replace('left:', '').replace('px', ''))+i*11.5 for i, v in enumerate(td.xpath('./img[not(contains(@class, "{}"))]/@style'.format(md5)))]
        number = ''.join([i[1] for i in sorted(zip(sort_rule, num))])
        data.append(int(number))
    datas.extend(data)
    print(data)
print(sum(datas))
```

结果：

![image-20210224162826797](E:\Markdown\images\image-20210224162826797.png)



## 16、webpack - 初体验

任务十六：抓取这5页的数字，计算加和并提交结果

![image-20210224163422110](E:\Markdown\images\image-20210224163422110.png)

打开 F12 查看接口，发现会跳转到首页，先打开 F12 在进入题目，虽然还是跳转但是能找到接口

![image-20210224171025803](E:\Markdown\images\image-20210224171025803.png)

接口为：http://match.yuanrenxue.com/api/match/16?

参数为：

- m=D6GG8ME6YXh3cWe5697eb298443f360d599694736dde8f3e6haN2AiP8

- t=1614157633000

其中 t 很明显是时间戳，m 是某种加密

![image-20210224171445002](E:\Markdown\images\image-20210224171445002.png)

查看调用栈，很明显是 ajax 请求。将接口添加到 XHR 断点，重新进入题目

![](E:\Markdown\images\image-20210224173815528.png)

格式化下，查看 window.request 调用

![image-20210224174127715](E:\Markdown\images\image-20210224174127715.png)

可以看到 ajax 请求参数为 i ，参数 i 实际就是 r，r.t 是 p_s，也就是 `Date[e(496)](new Date)[e(517)]()`，是个时间戳，console 验证一下

![image-20210224174513257](E:\Markdown\images\image-20210224174513257.png)

r.m 是由 `n[e(528)](btoa, p_s)`，往上找可以发现 n 就是 t，`t[e(528)] = function(e,t){ return e(t)}`，即 `n[e(528)](btoa, p_s)` 实际为 `btoa(p_s)`。一般 `btoa` 为 js 的 base64 加密，但这里不是，应该是重新定义了，跳进去看下

![](E:\Markdown\images\image-20210224175615408.png)

这里 `window[u(208)]` 就是 btoa，将代码复制下来，打开一个新的浏览器窗口调试执行，修改下函数名，缺啥补啥

![image-20210224180132372](E:\Markdown\images\image-20210224180132372.png)

运行后，在 console 调试

![image-20210224180328080](E:\Markdown\images\image-20210224180328080.png)

缺少 u，原代码中调试查找

![image-20210224180612080](E:\Markdown\images\image-20210224180612080.png)

![image-20210224180734976](E:\Markdown\images\image-20210224180734976.png)

这里 u 实际就是 l，l 中 `_0x34e7` 是前面定义的数组，但鼠标放到上面发现，该数组与初始定义的数组不一致，应该是对其进行了修改

![image-20210224181052600](E:\Markdown\images\image-20210224181052600.png)

打断点重新刷新网页调试下

![image-20210224181418464](E:\Markdown\images\image-20210224181418464.png)

![image-20210225092430546](E:\Markdown\images\image-20210225092430546.png)

通过调试发现，定义 `_0x34e7` 之后又定义了 a、s，其中 s 是对 a 也就是 `_0x34e7 ` 数组修改的函数，然后在后面的多元表达式中定义了 r，并执行 `r.getCookie` 函数，在 `r.getCookie` 中 执行了 `s(134)` 对数组进行了修改

将这些代码复制到 js 文件中，修改下接着调试执行

![image-20210225093629680](E:\Markdown\images\image-20210225093629680.png)

调试补充 f

![image-20210225093741152](E:\Markdown\images\image-20210225093741152.png)

f 是一个字符串，并在前面找到了 f 的定义，调试对比下

![image-20210225094125800](E:\Markdown\images\image-20210225094125800.png)

与上面字符串一致，这里 u 就是上一步的 u，复制到 js 文件中继续调试

![image-20210225102959711](E:\Markdown\images\image-20210225102959711.png)

调试补充d

![image-20210225103146049](E:\Markdown\images\image-20210225103146049.png)

![image-20210225103241061](E:\Markdown\images\image-20210225103241061.png)

调试补充 md5

![image-20210225103747023](E:\Markdown\images\image-20210225103747023.png)

这里就很清晰了，思路基本和 `btoa` 一致，将 md5 补充到 js 文件中调试

![image-20210225103942538](E:\Markdown\images\image-20210225103942538.png)

出来结果了，访问测试下

```python
url = 'http://match.yuanrenxue.com/api/match/16?'

headers = {
    'User-Agent': 'yuanrenxue.project',
}
params = {
    'page': 1,
    't': 1614216839000,
    'm': 'Kjty5Sm2AsWDkxb17e7f0f103d679a564158e97b1b2937dsxEzP5hYDJ',
}
resp = requests.get(url, params=params, headers=headers)
print(resp.json())
```

结果：

```python
{'error': 'Unexpected token/Validation failed'}
```

试了好久结果都不对，最后调试发现，`btoa` 中 有个 `n.g`，且未在 `btoa` 中找到相关的定义

![image-20210225111305193](E:\Markdown\images\image-20210225111305193.png)

跳转到n，下面有个定义的 n.g

![image-20210225111501294](E:\Markdown\images\image-20210225111501294.png)

复制到 js 文件中，调试测试

![image-20210225111709964](E:\Markdown\images\image-20210225111709964.png)

结果：

```python
{'status': '1', 'state': 'success', 'data': [{'value': 1102}, {'value': 8185}, {'value': 7105}, {'value': 5140}, {'value': 9583}, {'value': 7644}, {'value': 3836}, {'value': 6827}, {'value': 9183}, {'value': 8035}]}
```

修改下 js 代码，并用 execjs 执行，这里需注意 `btoa` 中正则表达式 `[^\u0000-\u00ff]` 中 `\` 需转义，否则 execjs 执行会报错

```python
import requests
import execjs
import time
# 猿人学第 16 题，难度：简单
# webpack 初体验
# http://match.yuanrenxue.com/match/16


url = 'http://match.yuanrenxue.com/api/match/16?'

headers = {
    'User-Agent': 'yuanrenxue.project',
    'Cookie': 'sessionid=kbcfe9oznl8lp4iimdwjfgo3k15msmtx',
}

js_code = '''var e, t;
_0x4c28 = ["18|38|15|2", "ucisR", "wWwRM", "LzcOo", "yWGcu", "PlAEw", "ihcci", "hBKtU", "rvloG", "xcQTI", "uhJgH", "vRqUp", "EQEzR", "abc", "QgSUn", "0|45|44|19", "WMqBp", "koePJ", "jGSEC", "IKbhW", "wEOgn", "|49|71|11|", "xgzfr", "ABCDEF", "DdHPB", "aFxRD", "sFtiw", "concat", "YhaCC", "YVBwM", "abYok", "2|28|6|36|", "NLOsy", "bRLIN", "xGAWc", "length", "zYRlD", "14|67|61|3", "bolvy", "pagBT", "mdsJQ", "4|69|41|26", "kaXPV", "IWxBE", "pviAr", "5|0|2", "lvwPz", "YcDFe", "yGmJD", "FcYqi", "AAZoR", "|46|5|3|50", "PnITs", "ABCDEFGHIJ", "charCodeAt", "KLMNOPQRST", "prrXX", "FDiNG", "split", "oBesn", "9|24|10|56", "VaXsK", "fromCharCo", "FDfcp", "rrdPR", "HHkBN", "89+/", "mfuQZ", "PbrnX", "FcXlo", "rNapo", "fEXNi", "qtIDJ", "60|53|21|5", "Rtsed", "SUrST", "nsaps", "vyNVU", "2|29|23|64", "0|43|57|4|", "NNXUu", "nCrbn", "wQPIq", "XBcOb", "39|40|47|6", "ljkOt", "yMPhx", "TXzzv", "0123456789", "fmdcS", "iXQwu", "grCxb", "3|6|1|4|7|", "wKeAM", "Iekey", "opqrstuvwx", "|7|17", "BQgZQ", "BtzmV", "jZUAt", "HYhpy", "Yvoqt", "VyzBI", "NNVLf", "dbmfK", "0|58|16|32", "UAFHv", "WNIsZ", "2|1|4|3|5|", "JFqRJ", "zObVA", "d24fb0d696", "XfWkD", "MFmWH", "lZISZ", "WzbFA", "kaQlD", "3f7d28e17f", "eSwEi", "YpeFX", "kZhzK", "KxKIe", "LAIPf", "LjyKQ", "YLwOK", "iqfMz", "51|8|0|65|", "JRihE", "nqEyg", "|37|22|27|", "ZXsFi", "goEwl", "|31|63|48|", "wvVCN", "wnDlW", "Myvqp", "UlhBp", "fwCDC", "charAt", "Lmhlz", "WQCAS", "UXeVn", "KIXRL", "HiEZt", "WNzfT", "lNWda", "tsNzQ"],
e = _0x4c28,
t = 368,
function(t) {
    for (; --t; )
        e.push(e.shift())
}(++t);

var n = function(e, t) {
                return _0x4c28[e -= 0]
            };

md5 = function(e) {
                var t = n
                  , r = {
                    fEXNi: function(e, t) {
                        return e(t)
                    },
                    LzcOo: function(e, t, n) {
                        return e(t, n)
                    }
                };
                r[t(3)] = function(e, t) {
                    return e(t)
                }
                ,
                r.wEOgn = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(120)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(69)] = function(e, t) {
                    return e == t
                }
                ,
                r[t(109)] = function(e, t) {
                    return e(t)
                }
                ,
                r[t(112)] = t(86),
                r.oBesn = "900150983c" + t(37) + t(43) + "72",
                r[t(70)] = t(18) + t(118),
                r[t(16)] = function(e, t) {
                    return e < t
                }
                ,
                r[t(2)] = t(110) + t(5) + t(133) + "|55|13|12|" + t(146) + t(114) + t(94) + "35|68|33|4" + t(104) + t(52) + t(73) + t(88) + t(55) + "25|34|1|2|" + t(10) + t(4) + t(124) + t(58) + "52|59|66|7" + t(31) + t(22),
                r[t(53)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(35)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(141)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(91)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(65)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(38)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(19)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(117)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(92)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(82)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(111)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(78)] = function(e, t) {
                    return e + t
                }
                ,
                r.lZISZ = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.Iekey = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.AAZoR = function(e, t) {
                    return e + t
                }
                ,
                r[t(67)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.UlhBp = function(e, t) {
                    return e + t
                }
                ,
                r.yMPhx = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(138)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(121)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(98)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r.kHuTw = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(50)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(142)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(87)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(90)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(59)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(28)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(119)] = function(e, t) {
                    return e + t
                }
                ,
                r.YpeFX = function(e, t) {
                    return e + t
                }
                ,
                r[t(7)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.prrXX = function(e, t) {
                    return e + t
                }
                ,
                r.kaQlD = function(e, t) {
                    return e + t
                }
                ,
                r.qtIDJ = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.xGAWc = function(e, t) {
                    return e + t
                }
                ,
                r[t(134)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(89)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(15)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(9)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(56)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(6)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(32)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(99)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(39)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(113)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(106)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(66)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r.TXzzv = function(e, t) {
                    return e + t
                }
                ,
                r.NNVLf = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(79)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(1)] = function(e, t, n, r, i, o, a, s) {
                    return e(t, n, r, i, o, a, s)
                }
                ,
                r[t(81)] = function(e, t) {
                    return e + t
                }
                ,
                r.MXnIN = function(e, t) {
                    return e >> t
                }
                ,
                r[t(23)] = function(e, t) {
                    return e << t
                }
                ,
                r.nqEyg = function(e, t) {
                    return e % t
                }
                ,
                r.kaXPV = function(e, t) {
                    return e >>> t
                }
                ,
                r[t(24)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(44)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(30)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(143)] = function(e, t) {
                    return e | t
                }
                ,
                r[t(101)] = function(e, t) {
                    return e & t
                }
                ,
                r[t(122)] = function(e, t, n, r, i, o, a) {
                    return e(t, n, r, i, o, a)
                }
                ,
                r.ZpUiH = function(e, t) {
                    return e & t
                }
                ,
                r[t(72)] = function(e, t) {
                    return e ^ t
                }
                ,
                r[t(130)] = function(e, t) {
                    return e ^ t
                }
                ,
                r[t(41)] = function(e, t) {
                    return e | t
                }
                ,
                r[t(116)] = function(e, t) {
                    return e > t
                }
                ,
                r[t(80)] = function(e, t) {
                    return e(t)
                }
                ,
                r[t(33)] = function(e, t, n) {
                    return e(t, n)
                }
                ,
                r[t(83)] = function(e, t) {
                    return e(t)
                }
                ,
                r[t(60)] = function(e, t) {
                    return e + t
                }
                ,
                r.FDfcp = function(e, t) {
                    return e * t
                }
                ,
                r[t(95)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(51)] = function(e, t) {
                    return e & t
                }
                ,
                r.DdHPB = function(e, t) {
                    return e >> t
                }
                ,
                r.abYok = function(e, t) {
                    return e | t
                }
                ,
                r[t(84)] = function(e, t) {
                    return e << t
                }
                ,
                r[t(105)] = function(e, t) {
                    return e & t
                }
                ,
                r[t(8)] = function(e, t) {
                    return e - t
                }
                ,
                r[t(137)] = function(e) {
                    return e()
                }
                ,
                r.YVBwM = function(e, t) {
                    return e << t
                }
                ,
                r[t(27)] = function(e, t) {
                    return e & t
                }
                ,
                r[t(26)] = function(e, t) {
                    return e / t
                }
                ,
                r[t(74)] = function(e, t) {
                    return e * t
                }
                ,
                r[t(49)] = t(14) + "abcdef",
                r[t(36)] = function(e, t) {
                    return e >> t
                }
                ,
                r[t(46)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(75)] = function(e, t) {
                    return e >> t
                }
                ,
                r[t(47)] = function(e, t) {
                    return e * t
                }
                ,
                r[t(11)] = t(126) + t(128) + "UVWXYZabcdefghijklmn" + t(21) + "yz01234567" + t(139),
                r[t(63)] = function(e, t) {
                    return e * t
                }
                ,
                r.KIXRL = function(e, t) {
                    return e << t
                }
                ,
                r[t(57)] = function(e, t) {
                    return e % t
                }
                ,
                r[t(77)] = function(e, t) {
                    return e << t
                }
                ,
                r[t(71)] = function(e, t) {
                    return e >> t
                }
                ,
                r.jZUAt = function(e, t) {
                    return e >> t
                }
                ,
                r[t(48)] = function(e, t) {
                    return e + t
                }
                ,
                r[t(17)] = function(e, t) {
                    return e % t
                }
                ,
                r[t(85)] = function(e, t) {
                    return e * t
                }
                ,
                r[t(61)] = function(e, t) {
                    return e < t
                }
                ,
                r.mfuQZ = function(e, t) {
                    return e + t
                }
                ,
                r[t(125)] = function(e, t) {
                    return e * t
                }
                ,
                r[t(0)] = function(e, t) {
                    return e(t)
                }
                ;
                var i = r;
                function o(e, n) {
                    for (var r = t, o = i.WNzfT[r(131)]("|"), a = 0; ; ) {
                        switch (o[a++]) {
                        case "0":
                            for (var d = 0; i.iXQwu(d, e.length); d += 16)
                                for (var p = i[r(2)][r(131)]("|"), h = 0; ; ) {
                                    switch (p[h++]) {
                                    case "0":
                                        w = i[r(53)](l, w, b, x, T, e[d + 2], 9, -51403784);
                                        continue;
                                    case "1":
                                        x = u(x, T, w, b, e[d + 6], 23, 76029189);
                                        continue;
                                    case "2":
                                        b = i[r(53)](u, b, x, T, w, e[i.JFqRJ(d, 9)], 4, -640364487);
                                        continue;
                                    case "3":
                                        T = i[r(141)](c, T, w, b, x, e[d + 10], 15, -1051523);
                                        continue;
                                    case "4":
                                        T = s(T, w, b, x, e[i.JFqRJ(d, 2)], 17, 606105819);
                                        continue;
                                    case "5":
                                        w = i[r(91)](c, w, b, x, T, e[i[r(65)](d, 3)], 10, -1894446606);
                                        continue;
                                    case "6":
                                        w = i.XfWkD(l, w, b, x, T, e[i.wKeAM(d, 14)], 9, -1019803690);
                                        continue;
                                    case "7":
                                        T = i.pviAr(f, T, v);
                                        continue;
                                    case "8":
                                        b = i.XfWkD(l, b, x, T, w, e[i[r(92)](d, 13)], 5, -1444681467);
                                        continue;
                                    case "9":
                                        x = i[r(38)](s, x, T, w, b, e[i[r(82)](d, 3)], 22, -1044525330);
                                        continue;
                                    case "10":
                                        w = s(w, b, x, T, e[i[r(82)](d, 5)], 12, 1200080426);
                                        continue;
                                    case "11":
                                        x = i[r(38)](l, x, T, w, b, e[i[r(82)](d, 0)], 20, -373897302);
                                        continue;
                                    case "12":
                                        w = i[r(38)](s, w, b, x, T, e[i[r(82)](d, 9)], 12, -1958435417);
                                        continue;
                                    case "13":
                                        b = i.XfWkD(s, b, x, T, w, e[i.xcQTI(d, 8)], 7, 1770035416);
                                        continue;
                                    case "14":
                                        var m = b;
                                        continue;
                                    case "15":
                                        w = i[r(38)](u, w, b, x, T, e[i.xcQTI(d, 8)], 11, -2022574463);
                                        continue;
                                    case "16":
                                        b = f(b, m);
                                        continue;
                                    case "17":
                                        w = i[r(111)](f, w, g);
                                        continue;
                                    case "18":
                                        x = l(x, T, w, b, e[i[r(78)](d, 12)], 20, -1921207734);
                                        continue;
                                    case "19":
                                        w = i[r(40)](u, w, b, x, T, e[d + 4], 11, 1272893353);
                                        continue;
                                    case "20":
                                        T = i[r(20)](u, T, w, b, x, e[i.PlAEw(d, 11)], 16, 1839030562);
                                        continue;
                                    case "21":
                                        b = s(b, x, T, w, e[i[r(123)](d, 12)], 7, 1804550682);
                                        continue;
                                    case "22":
                                        x = u(x, T, w, b, e[i[r(123)](d, 10)], 23, -1094730640);
                                        continue;
                                    case "23":
                                        T = i[r(67)](c, T, w, b, x, e[d + 14], 15, -1416354905);
                                        continue;
                                    case "24":
                                        b = s(b, x, T, w, e[i[r(123)](d, 4)], 7, -176418897);
                                        continue;
                                    case "25":
                                        w = i.UXeVn(u, w, b, x, T, e[d + 0], 11, -358537222);
                                        continue;
                                    case "26":
                                        b = i.UXeVn(l, b, x, T, w, e[i[r(62)](d, 1)], 5, -165796510);
                                        continue;
                                    case "27":
                                        b = i.UXeVn(u, b, x, T, w, e[i[r(62)](d, 13)], 4, 681279174);
                                        continue;
                                    case "28":
                                        b = i[r(12)](l, b, x, T, w, e[i[r(138)](d, 9)], 5, 568446438);
                                        continue;
                                    case "29":
                                        w = i.yMPhx(c, w, b, x, T, e[d + 7], 10, 11261161415);
                                        continue;
                                    case "30":
                                        var g = w;
                                        continue;
                                    case "31":
                                        b = c(b, x, T, w, e[i.yGmJD(d, 8)], 6, 1873313359);
                                        continue;
                                    case "32":
                                        x = i.aFxRD(f, x, y);
                                        continue;
                                    case "33":
                                        T = i[r(12)](l, T, w, b, x, e[i[r(121)](d, 15)], 14, -660478335);
                                        continue;
                                    case "34":
                                        T = i.kHuTw(u, T, w, b, x, e[d + 3], 16, -722881979);
                                        continue;
                                    case "35":
                                        b = i[r(50)](l, b, x, T, w, e[i[r(121)](d, 5)], 5, -701520691);
                                        continue;
                                    case "36":
                                        T = l(T, w, b, x, e[i[r(121)](d, 3)], 14, -187363961);
                                        continue;
                                    case "37":
                                        T = i[r(142)](u, T, w, b, x, e[i.QgSUn(d, 7)], 16, -155497632);
                                        continue;
                                    case "38":
                                        b = i.FcXlo(u, b, x, T, w, e[i.koePJ(d, 5)], 4, -378558);
                                        continue;
                                    case "39":
                                        w = i[r(142)](u, w, b, x, T, e[i[r(90)](d, 12)], 11, -421815835);
                                        continue;
                                    case "40":
                                        T = i[r(59)](u, T, w, b, x, e[i[r(28)](d, 15)], 16, 530742520);
                                        continue;
                                    case "41":
                                        x = i.wvVCN(s, x, T, w, b, e[d + 15], 22, 1236531029);
                                        continue;
                                    case "42":
                                        x = i[r(59)](l, x, T, w, b, e[i[r(119)](d, 4)], 20, -405537848);
                                        continue;
                                    case "43":
                                        b = i[r(59)](s, b, x, T, w, e[i.lvwPz(d, 0)], 7, -680976936);
                                        continue;
                                    case "44":
                                        b = i[r(59)](u, b, x, T, w, e[i[r(45)](d, 1)], 4, -1530992060);
                                        continue;
                                    case "45":
                                        x = i.nCrbn(u, x, T, w, b, e[i[r(129)](d, 14)], 23, -35311556);
                                        continue;
                                    case "46":
                                        b = c(b, x, T, w, e[i[r(42)](d, 12)], 6, 1700485571);
                                        continue;
                                    case "47":
                                        x = i[r(7)](u, x, T, w, b, e[i.kaQlD(d, 2)], 23, -995338651);
                                        continue;
                                    case "48":
                                        T = c(T, w, b, x, e[d + 6], 15, -1560198380);
                                        continue;
                                    case "49":
                                        w = i[r(145)](l, w, b, x, T, e[i[r(107)](d, 6)], 9, -1069501632);
                                        continue;
                                    case "50":
                                        x = i[r(134)](c, x, T, w, b, e[i[r(89)](d, 1)], 21, -2054922799);
                                        continue;
                                    case "51":
                                        x = i.fmdcS(l, x, T, w, b, e[d + 8], 20, 1163531501);
                                        continue;
                                    case "52":
                                        x = i[r(15)](c, x, T, w, b, e[i[r(9)](d, 13)], 21, 1309151649);
                                        continue;
                                    case "53":
                                        x = i[r(15)](s, x, T, w, b, e[i[r(56)](d, 11)], 22, -1990404162);
                                        continue;
                                    case "54":
                                        w = i[r(6)](s, w, b, x, T, e[i[r(32)](d, 13)], 12, -40341101);
                                        continue;
                                    case "55":
                                        x = i.sFtiw(s, x, T, w, b, e[i.UAFHv(d, 7)], 22, -45705983);
                                        continue;
                                    case "56":
                                        T = i.sFtiw(s, T, w, b, x, e[i.MFmWH(d, 6)], 17, -1473231341);
                                        continue;
                                    case "57":
                                        w = i[r(99)](s, w, b, x, T, e[i.MFmWH(d, 1)], 12, -389564586);
                                        continue;
                                    case "58":
                                        x = c(x, T, w, b, e[i[r(39)](d, 9)], 21, -343485551);
                                        continue;
                                    case "59":
                                        b = i[r(113)](c, b, x, T, w, e[i[r(39)](d, 4)], 6, -145523070);
                                        continue;
                                    case "60":
                                        T = i.bRLIN(s, T, w, b, x, e[i[r(39)](d, 10)], 17, -42063);
                                        continue;
                                    case "61":
                                        var v = T;
                                        continue;
                                    case "62":
                                        b = i[r(66)](c, b, x, T, w, e[d + 0], 6, -198630844);
                                        continue;
                                    case "63":
                                        w = i[r(66)](c, w, b, x, T, e[i[r(13)](d, 15)], 10, -30611744);
                                        continue;
                                    case "64":
                                        x = c(x, T, w, b, e[d + 5], 21, -57434055);
                                        continue;
                                    case "65":
                                        T = i[r(29)](l, T, w, b, x, e[i[r(13)](d, 7)], 14, 1735328473);
                                        continue;
                                    case "66":
                                        w = i[r(29)](c, w, b, x, T, e[i[r(79)](d, 11)], 10, -1120210379);
                                        continue;
                                    case "67":
                                        var y = x;
                                        continue;
                                    case "68":
                                        w = i[r(1)](l, w, b, x, T, e[d + 10], 9, 38016083);
                                        continue;
                                    case "69":
                                        T = i[r(1)](s, T, w, b, x, e[i[r(79)](d, 14)], 17, -1502002290);
                                        continue;
                                    case "70":
                                        T = i.SUrST(c, T, w, b, x, e[i[r(79)](d, 2)], 15, 718787259);
                                        continue;
                                    case "71":
                                        T = l(T, w, b, x, e[i[r(81)](d, 11)], 14, 643717713);
                                        continue
                                    }
                                    break
                                }
                            continue;
                        case "1":
                            var b = 1732584193;
                            continue;
                        case "2":
                            return Array(b, x, T, w);
                        case "3":
                            e[i.MXnIN(n, 5)] |= i[r(23)](128, i[r(54)](n, 32));
                            continue;
                        case "4":
                            var x = -271733879;
                            continue;
                        case "5":
                            var w = 271733878;
                            continue;
                        case "6":
                            e[i.BQgZQ(i[r(115)](n + 64, 9), 4) + 14] = n;
                            continue;
                        case "7":
                            var T = -1732584194;
                            continue
                        }
                        break
                    }
                }
                function a(e, n, r, o, a, s) {
                    var l = t;
                    return f(i.BtzmV(d, i[l(44)](f, i.dbmfK(f, n, e), i[l(30)](f, o, s)), a), r)
                }
                function s(e, n, r, o, s, l, u) {
                    var c = t;
                    return a(i[c(143)](i[c(101)](n, r), i[c(101)](~n, o)), e, n, s, l, u)
                }
                function l(e, n, r, o, s, l, u) {
                    var c = t;
                    return i[c(122)](a, i[c(143)](i.ZpUiH(n, o), i.ZpUiH(r, ~o)), e, n, s, l, u)
                }
                function u(e, n, r, o, s, l, u) {
                    return i[t(122)](a, i.tsNzQ(n ^ r, o), e, n, s, l, u)
                }
                function c(e, n, r, o, s, l, u) {
                    var c = t;
                    return i[c(122)](a, i[c(130)](r, i[c(41)](n, ~o)), e, n, s, l, u)
                }
                function f(e, n) {
                    var r = t
                      , o = i[r(95)](65535 & e, i.iqfMz(n, 65535))
                      , a = i[r(95)](e >> 16, i[r(97)](n, 16)) + i[r(97)](o, 16);
                    return i[r(103)](i[r(84)](a, 16), i[r(105)](o, 65535))
                }
                function d(e, n) {
                    var r = t;
                    return i.abYok(e << n, e >>> i[r(8)](32, n))
                }
                function p(e) {
                    for (var n = t, r = i[n(137)](Array), o = i[n(8)](i.vRqUp(1, 16), 1), a = 0; a < i.FDfcp(e[n(108)], 16); a += 16)
                        r[i[n(97)](a, 5)] |= i[n(102)](i[n(27)](e[n(127)](i[n(26)](a, 16)), o), i[n(54)](a, 32));
                    return r
                }
                function h(e) {
                    for (var n = t, r = i[n(49)], o = "", a = 0; i.iXQwu(a, i[n(74)](e[n(108)], 4)); a++)
                        o += i.xgzfr(r[n(64)](15 & i[n(36)](e[i[n(36)](a, 2)], i[n(46)](i[n(74)](a % 4, 8), 4))), r[n(64)](15 & i.wWwRM(e[a >> 2], i[n(47)](a % 4, 8))));
                    return o
                }
                return i[t(0)]((function(e) {
                    var n = t;
                    return i[n(144)](h, i[n(76)](o, i.vyNVU(p, e), 16 * e[n(108)]))
                }
                ), e)
            }



var a,s

_0x34e7 = ["AqLWq", "0zyxwvutsr", "TKgNw", "eMnqD", "thjIz", "btoa", "MNPQRSTWXY", "oPsqh", "niIlq", "evetF", "LVZVH", "fYWEX", "kmnprstwxy", "aYkvo", "tsrqpomnlk", "HfLqY", "aQCDK", "lGBLj", "test", "3210zyxwvu", "QWK2Fi", 'return /" ', "hsJtK", "jdwcO", "SlFsj", "OWUOc", "LCaAn", "[^ ]+)+)+[", "FAVYf", "2Fi+987654", "floor", "join", "EuwBW", "OXYrZ", "charCodeAt", "SkkHG", "iYuJr", "GwoYF", "kPdGe", "cVCcp", "INQRH", "INVALID_CH", "charAt", "push", "apply", "lalCJ", "kTcRS", '+ this + "', "ykpOn", "gLnjm", "gmBaq", "kukBH", "dvEWE", "SFKLi", "^([^ ]+( +", "qpomnlkjih", "^ ]}", "pHtmC", "length", "split", "ABHICESQWK", "FKByN", "U987654321", "lmHcG", "dICfr", "Szksx", "Bgrij", "iwnNJ", "jihgfdecba", "GfTek", "gfdecbaZXY", "constructo", "QIoXW", "jLRMs"]
a = _0x34e7
s = function(e) {
                for (; --e; )
                    a.push(a.shift())
            }
s(134)

var l = function(e, t) {
            return _0x34e7[e -= 188]
        }

var u = l

var f = u(191) + u(204) + u(258) + u(199) + "WVUTSRQPON" + u(189) + u(232) + u(222) + u(217) + u(197) + "ZXYWVUTSRQPONABHICES" + u(223);
function d(e) {
                var t = u
                  , n = {};
                n[t(214)] = function(e, t) {
                    return e || t
                }
                ,
                n.bWcgB = function(e, t) {
                    return e * t
                }
                ,
                n[t(227)] = "ABCDEFGHJK" + t(209) + "Zabcdefhij" + t(215) + "z2345678";
                for (var r = n, o = "1|3|0|4|2|5"[t(188)]("|"), a = 0; ; ) {
                    switch (o[a++]) {
                    case "0":
                        var s = l[t(261)];
                        continue;
                    case "1":
                        e = r[t(214)](e, 32);
                        continue;
                    case "2":
                        for (i = 0; i < e; i++)
                            c += l[t(245)](Math[t(233)](r.bWcgB(Math.random(), s)));
                        continue;
                    case "3":
                        var l = r[t(227)];
                        continue;
                    case "4":
                        var c = "";
                        continue;
                    case "5":
                        return c
                    }
                    break
                }
            }

btoa = function(e) {
        var t = u
          , r = {};
        r.TGmSp = t(244) + "ARACTER_ERR",
        r[t(238)] = t(224) + t(250) + "/",
        r[t(205)] = "^([^ ]+( +" + t(230) + t(259),
        r.aYkvo = function(e) {
            return e()
        }
        ,
        r[t(254)] = function(e, t) {
            return e % t
        }
        ,
        r.evetF = function(e, t) {
            return e >> t
        }
        ,
        r.GfTek = t(196),
        r[t(260)] = function(e, t) {
            return e << t
        }
        ,
        r[t(229)] = function(e, t) {
            return e | t
        }
        ,
        r[t(242)] = function(e, t) {
            return e << t
        }
        ,
        r[t(228)] = function(e, t) {
            return e & t
        }
        ,
        r[t(207)] = function(e, t) {
            return e << t
        }
        ,
        r[t(202)] = function(e, t) {
            return e & t
        }
        ,
        r.jdwcO = function(e, t) {
            return e === t
        }
        ,
        r.kPdGe = t(231),
        r[t(195)] = t(213),
        r[t(201)] = function(e, t) {
            return e & t
        }
        ,
        r[t(206)] = function(e, t) {
            return e == t
        }
        ,
        r[t(219)] = function(e, t) {
            return e + t
        }
        ,
        r[t(220)] = function(e, t) {
            return e(t)
        }
        ;
        var i = r;
        if (/([^\\u0000-\\u00ff])/.test(e))
            throw new Error(i.TGmSp);
        for (var o, a, s, l = 0, c = []; l < e[t(261)]; ) {
            switch (a = e[t(237)](l),
            s = i.kukBH(l, 6)) {
            case 0:
                delete window,
                delete document,
                c[t(246)](f[t(245)](i[t(212)](a, 2)));
                break;
            case 1:
                try {
                    "WhHMm" === i[t(198)] || n.g && c[t(246)](f[t(245)](i.pHtmC(2 & o, 3) | i.evetF(a, 4)))
                } catch (e) {
                    c[t(246)](f[t(245)](i[t(229)](i.cVCcp(3 & o, 4), a >> 4)))
                }
                break;
            case 2:
                c[t(246)](f[t(245)](i[t(229)](i[t(242)](15 & o, 2), i.evetF(a, 6)))),
                c[t(246)](f[t(245)](i[t(228)](a, 63)));
                break;
            case 3:
                c[t(246)](f[t(245)](i[t(212)](a, 3)));
                break;
            case 4:
                c.push(f[t(245)](i[t(229)](i[t(207)](i.OWUOc(o, 4), 6), i[t(212)](a, 6))));
                break;
            case 5:
                c[t(246)](f[t(245)](i[t(229)](i[t(207)](i[t(202)](o, 15), 4), a >> 8))),
                c.push(f.charAt(i[t(202)](a, 63)))
            }
            o = a,
            l++
        }
        
        return 0 == s ? i[t(226)](i[t(241)], i[t(195)]) || (c[t(246)](f[t(245)](i[t(201)](o, 3) << 4)),
        c.push("FM")) : i.eMnqD(s, 1) && (c[t(246)](f[t(245)]((15 & o) << 2)),
        c[t(246)]("K")),
        i[t(219)](i.aQCDK(d(15), md5(c[t(234)](""))), i[t(220)](d, 10))
    }

n.g = function () {
    if ("object" == typeof globalThis)
        return globalThis;
    try {
        return this || new Function("return this")()
    } catch (e) {
        if ("object" == typeof window)
            return window
    }
}();

function get_params(t){
    var m = btoa(t)
    return m
}
'''
js = execjs.compile(js_code)
t = str(int(time.time()) * 1000)
result = js.call('get_params', t)
s = []
for i in range(1,6):

    params = {
        'page': i,
        't': t,
        'm': result,
    }
    resp = requests.get(url, params=params, headers=headers)
    print(resp.json())
    s.extend([i.get('value') for i in resp.json().get('data')])
print(sum(s))
```

结果：

![image-20210225113741679](E:\Markdown\images\image-20210225113741679.png)