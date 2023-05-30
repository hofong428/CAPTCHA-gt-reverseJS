# CAPTCHA-gt-reverseJS
reverse the G**test slide CAPTHA
1.register-slide?t=1683591796433:(python)
--response:
    "challenge": "6fe1ab1199b46ae3b10976f8a8f93758",
    "gt": "019924a82c70bb123aae90d483087f94",

2.https://apiv6.geetest.com/get.php(2nd get)
--payload:
    gt: 019924a82c70bb123aae90d483087f94
    challenge: 6fe1ab1199b46ae3b10976f8a8f93758
    w: K7F)Pl4ny5x2TJwiP10wmYrwjKeQub0QGOZYW...
-- preview/response:
    test result/status
    c: [12, 58, 98, 36, 43, 95, 62, 15, 12]

///after "click" the verification: request slide code
3.https://api.geetest.com/ajax.php
--payload:
    gt: 019924a82c70bb123aae90d483087f94
    challenge: 6fe1ab1199b46ae3b10976f8a8f93758
    w: nAvkoc0pow4yhN6oJTkup1cevEadvs8TSka...
--preview/response:
    slide success

4.https://api.geetest.com/get.php(python)
--payload:
    gt: 019924a82c70bb123aae90d483087f94
    challenge: 6fe1ab1199b46ae3b10976f8a8f93758

--preview:
    c: [12, 58, 98, 36, 43, 95, 62, 15, 12]
    bg: a7a617ada.jpg == webp
    s : "35394f3f"

///after slide: test api
5.https://api.geetest.com/ajax.php(python)
--payload:
    gt: 019924a82c70bb123aae90d483087f94
    challenge: 6fe1ab1199b46ae3b10976f8a8f93758d8
    w: Obuk6qUN3M5EfWq6k62iNqQUgv)1tueRcy6Gzx18nhpKQebzjPo0AWVt4Wz9P0RQKNNI)..
--

gt and ch from server need not to be analyzed, but most of them need w

///target: w
1. after cleaning the cookie, click the ajax --> initiator --> click slide.js in and search for "w" --> "none" --> because it is base64 encrypted
2. use ast  to decrypt
3. enable the local override and substitute the ast decoded js for the original js
4. search for "w" again and get the "w" = h + u, and you can see all the parameters here in f={}
    lets print it out on the console, debug it on the f= , and output h + u, so the target to analyze u and h
5. go upward to find where h and u from, start from u, debug on the var u
6. print out the right side of u= on the console, it is 256bites & 16 decimal, it means it is a SHA512
7. we click the function then go to "$_CCDf" (=r[$_CAHJe(785)])
8. debug on the var under $_CCDf, click it.
9. start to step over till var e = new U(), you can find the e return under it later, so this is the encrypt formed location
10. [$_CBAGE(372)]: function, param=this[$_CBFJv(761)](t), the last is from the cleartext, hover it then click the functionlocation $_CCEV. (t is undefined)
11. go to "$_CCEV" and debug on the var under it(var $_CBFEs).but we find nothing from stepping over the function.
12. start stepping over till return 0t = rt(), hover it and click the functionlocation
13. go to return function and we debug on the var/ return = t()+t()+t()+t() at the same time.
14. print out the t(), it is 4 hexdecimal. we go upward and find the function t().
15. the function t() return an expression: return (65536 * (1 + Math[$_BFBEH(57)]()) | 0)[$_BFBEH(206)](16)[$_BFBEH(417)](1). lets copy it paste on new opened js.
16. print it out on the console, we find the result is also a 4 digits value and variable. it seems the concatation happens to t()+t()+t()+t() then get the cleartext.
17. we create a function on the new opened js (BS64 decoded):
        var _sj = function(){
        function t(){
        return (65536 * (1 + Math['random']()) | 0)['toString'](16)['substring'](1)
    }
    return function(){
            return t() + t() + t() + t()
    }
    }()
18. if we print it out, we will find it is 16 decimals. The cleartext is set.
    we clear the lines 3XXX breakpoints. Then we start analyze the algorithm: U().
19. debug on var e = new U()[$_CBGAE(372)](this[$_CBFJv(761)](t)), hover on U()[$_CBGAE(372)] then link it to line2441:
    E[$_IAJX(269)][$_IBAQ(372)] = function lt(t) {;
20. then, we copy all the codes of slide.js and paste to the js file we just created then run it on the webbrowser to see if it is runnable.
     we create the get_w() function, start to analyze the var u,
    -- function get_w(){
            var u = r[$_CAHJe(785)]()
        }
    -- substitute '$_CCDf' for $_CAHJe(785)
    -- analyze r, hover on it and enter the functionlocation
    -- it jump to "$_CCDF", lets search for it
    -- we find the function is under ee[$_CJFP(269)] = {...}
      , ne[$_CJFP(269)]
    -- so we export the ne[$_CJFP(269)], we write window._u = ne[$_CJFP(269)] under the arrays of it. we print it but get undefined.(complemet with window = global;
21. new U(): [$_CBGAE(372)] = ['encrypt'], hover on the new U()[$_CBGAE(372)] then enter the function (js:2441)
22. we create a snippet from the current js file, remember to change the window = global to = this.
    it seems to be an environment test, so we use the v_jstools to complement and generate the temp environment.
23. Reload all the webpage.
24. copy the temp environment form clipboard.
25. delete all the console.log()s
26. uncomment the var u = r[$_CAHJe(785)]() in the get_w() and print out u, it reply an error: r is not defined.
27. r is "window._u". we substitute window._u for r, also substitute the '$_CCDf' for $_CAHJe(785).
28. print out the u:
    function get_w(){
    var u = window._u['$_CCDf']()
    console.log(u)
        }
    get_w()
29. then we go to l (line:5212) and debug it. keep the var u and l debugs and waive others.
30. output the right side of l= on the console. it returns the arrays.(640)
    -- [37, 134, 227, 124, 105, 44, 120, 117, 129, 95, 19, 178, 94, 56, 65, 233, 30, 156, 208, 151, 219, 180, 193, 17, 69, 141, 101, 148, 222, 111, 16, 183, 82, 127, 19, 90, 195, 56, 201, 41, 69, 63, 142, 133, 249, 104, 176, 103, 128, 0, 149, 29, 45, 91, 72, 140, 241, 243, 136, 172, 86, 27, 10, 69, 98, 235, 120, 242, 213, 103, 51, 36, 15, 170, 122, 82, 118, 55, 201, 122, 135, 210, 209, 21, 42, 234, 4, 77, 238, 50, 217, 39, 92, 80, 228, 119, 53, 48, 25, 189, …]
31. decompose it (V[$_CAHJe(372)](gt[$_CAHJe(239)](o), r[$_CAIAj(761)]()), copy it to get_w()
    r[$_CAIAj(761)]() = _sj()
    [$_CAHJe(239)] = ['stringify']
    -- function get_w(){
        var u = window._u['$_CCDf']()
        var l = V[$_CAHJe(372)](gt[$_CAHJe(239)](o), r[$_CAIAj(761)]())
    }
    get_w()
32. substitute  _sj() for r[$_CAIAj(761)]()
33. output the gt[$_CAHJe(239)] on the console($_CAHJe(239) = 'stringify', gt = {stringify: ƒ}),
34. o = {
    "lang": "zh-cn",    ---- cleartext
    "userresponse": "cccc99cca3bb",   ---- x coordinate (move distance)
    "passtime": 1161,
    "imgload": 838,
    "aa": "X!),(!!R(ssswssssstssss!*tusuussuusBsss(!!(La-51/H1-033@QC89,43/-/7u0*$*M/523$3A",   ---- slide track
    "ep": {
        "v": "7.9.0",
        "$_BIo": false,
        "me": true,
        "tm": {
            "a": 1683616627339,
            "b": 1683616627768,
            "c": 1683616627768,
            "d": 0,
            "e": 0,
            "f": 1683616627341,
            "g": 1683616627341,
            "h": 1683616627341,
            "i": 1683616627341,
            "j": 1683616627341,
            "k": 0,
            "l": 1683616627347,
            "m": 1683616627761,
            "n": 1683616627764,
            "o": 1683616627782,
            "p": 1683616628758,
            "q": 1683616628758,
            "r": 1683616628788,
            "s": 1683616628792,
            "t": 1683616628792,
            "u": 1683616628792
        },
        "td": -1
    },
    "h9s9": "1816378497",    ---- need to analyze
    "rp": "38ee0556d0eacda6f403718b6859f00c"   ----
}
35. gt[$_CAHJe(239)](o) = {"lang":"zh-cn","userresponse":"cccc99cca3bb","passtime":1161,"imgload":838,"aa":"X!),(!!R(ssswssssstssss!*tusuussuusBsss(!!(La-51/H1-033@QC89,43/-/7u0*$*M/523$3A","ep":{"v":"7.9.0","$_BIo":false,"me":true,"tm":{"a":1683616627339,"b":1683616627768,"c":1683616627768,"d":0,"e":0,"f":1683616627341,"g":1683616627341,"h":1683616627341,"i":1683616627341,"j":1683616627341,"k":0,"l":1683616627347,"m":1683616627761,"n":1683616627764,"o":1683616627782,"p":1683616628758,"q":1683616628758,"r":1683616628788,"s":1683616628792,"t":1683616628792,"u":1683616628792},"td":-1},"h9s9":"1816378497","rp":"38ee0556d0eacda6f403718b6859f00c"} // strings
36. it means the gt[$_CAHJe(239)] equals to JSON.stringify, so do the substitution.
37. lets look upward to find o formation location: (line5153)
          o = {
            "lang": i[$_CAHJe(169)] || $_CAIAj(152),
            "userresponse": H(t, i[$_CAHJe(172)]),
            "passtime": n,
            "imgload": r[$_CAIAj(796)],
            "aa": e,
            "ep": r[$_CAHJe(724)]()
          };
      lets output these on console and analyze the compositions.
    -- "userresponse": H(t, i[$_CAHJe(172)]),
        H(t, i[$_CAHJe(172)]): '9cc99c99a3bb'
        i[$_CAHJe(172)]: 'df3fa392b2251cc561dbc15553991c6bat'
        $_CAHJe(172): 'challenge' ---> parameter
        i : includes bg, c, gt, slice, ... it comes from 4.https://api.geetest.com/get.php
        t = 56,
        lets follow the stack backward, then find u also equals to 56, var u = parseInt(_)
        _ = = e ? n[$_CJJJp(1064)][$_CJJJp(205)] : t[$_DAAAt(975)]() / a - n[$_CJJJp(955)]

38. we write a get_o function:
    -- go to the previous stack : $_CGII
    -- var u = parse(_),
    -- _ = e ? n[$_CJJJp(1064)][$_CJJJp(205)] : t[$_DAAAt(975)]() / a - n[$_CJJJp(955)]
    -- n[$_CJJJp(1064)] = {x: 56, y: -5}
    -- n[$_CJJJp(1064)][$_CJJJp(205)] = x coordinate
    -- parseInt(_) = 56
    -- so ... it is the x value
39. lets go to H of userresponse, hover on H and enter the functionlocation (js.626)
40. search for H(t,e) in the code and create a var to receive H: window._h = H
41. go to function get_o() and substitute window._h for H :(then print out the userresponse to check)
    function get_o(){
    userresponse = window._h(56, 'df3fa392b2251cc561dbc15553991c6bat'),
    t = {"x": 56, "y": -5}
    console.log(userresponse)
    }
42. next, we start to analyze "aa":
    -- "aa" = e under the o function (slide track), so we go to the previous call stack ($_CGII):
        we find the l parameter (equals to slide track):  n[$_DAAAt(912)][$_CJJJp(1034)](n[$_DAAAt(912)][$_CJJJp(1069)](), n[$_DAAAt(71)][$_DAAAt(1091)], n[$_CJJJp(71)][$_DAAAt(389)])
        lets restore it:
        --- aa = n[$_DAAAt(912)][$_CJJJp(1034)](n[$_DAAAt(912)][$_CJJJp(1069)](), n[$_DAAAt(71)][$_DAAAt(1091)], n[$_CJJJp(71)][$_DAAAt(389)])
        --- aa = n['$_CICa']['$_BBEI'](n['$_CICa']['$_FDL'](), n['$_CJV']['c'], n['$_CJV']['s'])
        --- we output the components to console: n['$_CJV']['s'] = '35394f3f', n['$_CJV']['c'] = (9) [12, 58, 98, 36, 43, 95, 62, 15, 12] ---> both from api.get.php,
        --- so we can fix these two numbers
        --- looks like the n['$_CICa']['$_FDL']() is the slide track. we analyze '$_FDL' first.
        --- we hover on the n[$_DAAAt(912)][$_CJJJp(1069)] under l then enter the functionlocation --> '$_FDL'
        --- debug on the var under it
        --- step over till var t = function (t){, step over it and t value appears:
            t = 0:(3) [31, 19, 0] (17).. it represents X, Y, time. (slide track variance from position 0)
        --- then we keep stepping over till return new ct(t)[$_BEHAJ(74)](function (t) {
        --- but if we look and hover on this[$_BEGJp(302)] above the
            r = [],
            i = [],
            o = [];, we find the real slide track:
            0: [-31, -19, 0]
            1: [0, 0, 0]
            2: [6, -4, 112]...
        --- step over till line3505: r[$_BEGJp(415)]($_BEHAJ(82)) + $_BEGJp(419) + i[$_BEGJp(415)]($_BEGJp(82)) + $_BEHAJ(419) + o[$_BEGJp(415)]($_BEGJp(82))
            (an encrpted slide track)
            let's decompose and restore it
            [$_BEGJp(415)]: ['join']
            it means encrypt each matrix independently then concat them.
        --- copy(JSON.stringify(this[$_BEGJp(302)])) and paste into function get_o():
            function get_o(){
            userresponse = window._h(30, 'df3fa392b2251cc561dbc15553991c6bat'),
            // t = {"x": 30, "y": -6}
            aa = n['$_CICa']['$_BBEI'](n['$_CICa']['$_FDL'](), [12, 58, 98, 36, 43, 95, 62, 15, 12], '35394f3f')
            xy = [[-31,-19,0],[0,0,0],[6,-4,112],[16,-4,117],[24,-5,128],[29,-5,131],[36,-5,141],[46,-5,143],[53,-5,149],[63,-5,155],[71,-5,159],[78,-5,169],[90,-7,173],[94,-7,179],[95,-7,186],[96,-7,199],[97,-7,236],[97,-7,587]]}
        --- search for '$_FDL':
            window._aa = W[$_CJEq(269)]["$_FDL"]
            put the _xy into the $_FDL function
        --- change the this[$_BEGJp(302)] into _xy --> for fix the track value
        --- then we rearrange the get_o():
            function get_o() {
            userresponse = window._h(30, 'df3fa392b2251cc561dbc15553991c6bat'),
                // t = {"x": 30, "y": -6}
                // console.log(userresponse)
            xy = [[-31, -19, 0], [0, 0, 0], [6, -4, 112], [16, -4, 117], [24, -5, 128], [29, -5, 131], [36, -5, 141], [46, -5, 143], [53, -5, 149], [63, -5, 155], [71, -5, 159], [78, -5, 169], [90, -7, 173], [94, -7, 179], [95, -7, 186], [96, -7, 199], [97, -7, 236], [97, -7, 587]]

            gls = window._aa(xy)
            console.log(gls)
            // aa = n['$_CICa']['$_BBEI'](n['$_CICa']['$_FDL'](), [12, 58, 98, 36, 43, 95, 62, 15, 12], '35394f3f')
        } then we get the track value;
        --- hover on n[$_DAAAt(912)][$_CJJJp(1034)] under l and enter the functionlocation --> "$_BBEI"
        --- change the get_o(){}:
            function get_o() {
            userresponse = window._h(30, 'df3fa392b2251cc561dbc15553991c6bat'),
                // t = {"x": 30, "y": -6}
                // console.log(userresponse)
            xy = [[-31, -19, 0], [0, 0, 0], [6, -4, 112], [16, -4, 117], [24, -5, 128], [29, -5, 131], [36, -5, 141], [46, -5, 143], [53, -5, 149], [63, -5, 155], [71, -5, 159], [78, -5, 169], [90, -7, 173], [94, -7, 179], [95, -7, 186], [96, -7, 199], [97, -7, 236], [97, -7, 587]]
            console.log(window._aa)
            // gls = window._aa(xy)
            // console.log(gls)
            // aa = n['$_CICa']['$_BBEI'](n['$_CICa']['$_FDL'](), [12, 58, 98, 36, 43, 95, 62, 15, 12], '35394f3f')
        }
        --- substitute window._aa for n['$_CICa'] and put the param xy in:
            function get_o() {
                userresponse = window._h(30, 'df3fa392b2251cc561dbc15553991c6bat'),
                    // t = {"x": 30, "y": -6}
                    // console.log(userresponse)
                xy = [[-31, -19, 0], [0, 0, 0], [6, -4, 112], [16, -4, 117], [24, -5, 128], [29, -5, 131], [36, -5, 141], [46, -5, 143], [53, -5, 149], [63, -5, 155], [71, -5, 159], [78, -5, 169], [90, -7, 173], [94, -7, 179], [95, -7, 186], [96, -7, 199], [97, -7, 236], [97, -7, 587]]
                //console.log(window._aa)
                // gls = window._aa(xy)
                // console.log(gls)
                aa = window._aa['$_BBEI'](window._aa['$_FDL'](xy), [12, 58, 98, 36, 43, 95, 62, 15, 12], '35394f3f');
                console.log(aa)
            }

            get_o()

            then we get the slide track aa
43. h9s9 and rp: we want to find out where is the location of assignment of o, go back to line5211/52112
    -- o[$_CAIAj(768)]: $_CAIAj(768) = 'rp' = X(i[$_CAIAj(197)] + i[$_CAIAj(172)][$_CAHJe(195)](0, 32) + o[$_CAHJe(747)]);
44. then we put the o in function get_o()

hover on V[$_CAHJe(372)],then go to return {
              "encrypt": function (t, e, n) {}
45. search for "encrypt", it belongs to "$_IEn": function (c) and belongs to var V
46. create window._l = V, print window._l to check Function: encrypt
47. substitute window._l for V[$_CAHJe(372)]:
    function get_w() {
    var u = window._u['$_CCDf']()
    var o = get_o()
    var l = window._l['encrypt'](JSON.stringify(o), _sj())
    console.log(l)
    }
    #####DONT FORGET THE get_w()######
48. then we get the Array(560) [
  150,  92,  90,   7,  32,  16, 243,  90, 207,  78,  58, 216,
  201,   9,  30,   6,  23,  74, 113, 197,  44,  27, 225, 101,
   31,  53,  87, 229,  11,   4,  72, 136, 202,  74,  27,  47,
  199, 244,  60, 137, 245, 128,  53, 105, 158, 250, 205,  51,
  244,  20, 250, 227, 107, 180,  50,  75, 133,  40, 123,  16,
   32, 158,   8,  41, 123, 201, 181, 136, 116, 195, 195,  64,
  209, 255,   5,  49, 167,  76, 172, 215, 132, 154,  70,  14,
  242, 111, 130, 137, 144,  52, 117,  56,   4,  43, 232,  32,
  252, 121,  87, 136,
  ... 460 more items, the same result with the output of
  V[$_CAHJe(372)](gt[$_CAHJe(239)](o), r[$_CAIAj(761)]()) on the console

49. then we go to h:
    lets output the m[$_CAIAj(783)](l) first, we hover on it then get in.
50.  "$_FEE": search for it and it belongs to m, we export it with window._m = m, and we put it in the function get_w, with param=l:
   function get_w() {
    var u = window._u['$_CCDf']()
    var o = get_o()
    var l = window._l['encrypt'](JSON.stringify(o), _sj())
    console.log(l)
    h = window._m['$_FEE'](l)
    console.log(h)
}, then we get the h algo.
51. We get the result w:
    function get_w() {
    var u = window._u['$_CCDf']()
    var o = get_o()
    var l = window._l['encrypt'](JSON.stringify(o), _sj())
    h = window._m['$_FEE'](l)
    w = h + u
    console.log(w)
}

53. Fix the picture


















