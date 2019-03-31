#类型转换		
先看一道题[]=![]

过程![]为false,[]=>''=>0,false=>0		

###非字符串到字符串
基本类型undefined,null,number,boolean,
		
	  String(null)//'null'
      String(undefined)//'undefined'
      String(true)//'true'
      String(1.0)//'1'
 1.0.toString()//'1'通过String()封装为字符串对象调用toString()方法	
	
	 // null.toString()//报错 null没有 对应原生函数
     // undefined.toString()	//同上	
原生函数

		Number()
		String()
		Boolean()
###非数字转为数字 

	 Number(true)//1
     Number(false)//0
     Number(undefined)//NaN
     Number(null)//0
     Number('true')//NaN
     Number('12')//12
     Number('')//0
//对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字调用valuOf()不能转为基本类型，调用toString()，没有valuOf() 和toString()会产生错误

     var obj = Object.create(null)
     Number(obj) 报错Uncaught TypeError: Cannot convert object to primitive value at Number (<anonymous>)	
###转为布尔类型
可以转换为false 的类型

	Boolean(undefined)//false
    Boolean(null)//false
    Boolean(false)//false
    Boolean(false)//false
    Boolean(0)//false
    Boolean(NaN)//false
    Boolean('')//false
其他为不能转为false

	Boolean(' ')//true
    Boolean([])//true
    Boolean({})//true
    Boolean('0')//true
    var a = new Boolean( false );
    var b = new Number( 0 );
    var c = new String( "" );
    typeof a //"object"
    Boolean(a)//true
    Boolean(b)//true
    Boolean(c)//true
//a,b,c为对象类型,都为true
##相等比较==
基本类型当比较两边类型不同时,先转为number,再比较

	var a = 42 
    var b ='42'
    a==b//true '42'转为42 
	var a = true
    var b = '42'
	a==b//false  true 转为1 '42'转为42

null == undefined //true 其他值与null,undefined 不相等

对象与非对象的比较,对象会先转为基本类型,再比较基本类型

	var a = '42'
    var b = [42]
    a==b //true [42] =>'42' 

	var a = 'abc'
    var b = new String(a)
	typeof a //string
    typeof b //object
	a==b //true //b会拆封 返回'abc' b.toString()

	var a = null
    var b = Object(a)//{}
    a==b//false
    var c = undefined
    var d = Object(c)//{}
    c==d //false

	var e = NaN 
    var f = Object(e)//new Number(NaN)
    e==f //false NaN ==NaN

再看一组

	"0" == null; // false
    "0" == undefined; // false
    "0" == false; // true 都转为0
    "0" == NaN; // false
    "0" == 0; // true '0'=>0
    "0" == ""; // false 
    false == null; // false
    false == undefined; // false
    false == NaN; // false
    false == 0; // true false =>0
    false == ""; // true 都转为0
    false == []; // true []=>''=>0 false=>0
    false == {}; // false {}=>'[object Object]' false=>0
    "" == null; // false
    "" == undefined; // false
    "" == NaN; // false
    "" == 0; // true ''=>0
    "" == []; // true []=>''=>0 ''=>0
    "" == {}; // false {}=>'[object Object]' ''=>0
    0 == null; // false
    0 == undefined; // false
    0 == NaN; // false
    0 == []; // true []=>''=>0
    0 == {}; // false {}=>'[object Object]'
    //null 只和undefined 相等 NaN和谁都不相等

再来一点

chrome控制台输出

 	{}+[];//0
    []+{};//"[object Object]"
