### 逻辑
- 根据该算法的思路，修复加密算法的bug或实现解密算法
```js
let tool = {
    cStr: (function() {
        let r=[];
        for(let i=0x21,c;i<0x7E;i++){
            c=String.fromCharCode(i);
            /^[\w%.!~*'()]$/.test(c) && r.push(c);
        }
        return r.join('');
    })(),
    calMixOffset (t) {
        if (!t) return 0;
        for (var i = 21, n = 0; n < t.length; n++) i += t.charCodeAt(n);
        return i;
    },
    getKvMap (t) {
        let e = {}, i = t % (this.cStr.length-1) + 1;
        for(let r = 0,o=this.cStr; r < o.length; r++) {
            let a = o[r], s = r + i, c = s >= o.length ? o[s - o.length] : o[s];
            e[c] = a
        }
        return e;
    },
    getvKMap (t) {
        let e = {}, i = t % (this.cStr.length-1) + 1;
        for(let r = 0,o=this.cStr; r < o.length; r++) {
            let a = o[r],s= r + i, c = s >= o.length ? o[s - o.length] : o[s];
            e[a] = c;
          
        }
        return e;
    },
    encrypt (url) {
        let tmp = url.split('?',2);
        let domain = tmp[0].split('/')[2];
        let params = tmp[1].split('&');
        let t = this.calMixOffset(domain);
        let kvMap = this.getKvMap(t);
        let np = [];
        for(let i=0;i<params.length;i++){
            let kv=params[i].split('='),k='',v='';
            for(let i=0;kv[0]&&i<kv[0].length;i++) k+=kvMap[kv[0][i]];
            for(let i=0;kv[1]&&i<kv[1].length;i++) v+=kvMap[kv[1][i]];
            np.push([k,v].reverse().join('='));
        }
        return [tmp[0],np.join('&')].join('?');
    },
    decrypt (url) {
        let tmp = url.split('?',2);
        let domain = tmp[0].split('/')[2];
        let params = tmp[1].split('&');
        let t = this.calMixOffset(domain);
        let kvMap = this.getvKMap(t);
        let np = [];
        for(let i=0;i<params.length;i++){
            let vk=params[i].split('='),k='',v='';
            for(let i=0;vk[0]&&i<vk[0].length;i++) {
                v+=kvMap[vk[0][i]]
            };
            for(let i=0;vk[1]&&i<vk[1].length;i++) {

                k+=kvMap[vk[1][i]]
            };
            np.push([k,v].join('='));
        }
        return [tmp[0],np.join('&')].join('?');
    }
};
let aUrl = 'https://example.com?username=uell&age=23&address=%E5%B9%BF%E4%B8%9C%E7%9C%81%E5%B9%BF%E5%B7%9E%E5%B8%82%E7%99%BD%E4%BA%91%E5%8C%BA';

// test
let temp = aUrl;
temp = tool.encrypt(temp);
console.log('encrypted data:', temp);

temp = tool.decrypt(temp);
console.log('decrypted data:', temp);

console.log('ok?', temp===aUrl);
```

### 调试
- 这个页面只有携带正确的参数才能正常显示内容，尝试通过调试获取访问此页面正确的参数。地址：https://dzkandian.github.io/interview/ant-debug.html

### 特定需求实现
- TODO

### 特定布局实现
- TODO
