# JSON.stringify #

重写JSON.stringify，可以指定打印的层数(默认20层),可以避免因为无限递归导致的JSON.stringify报错问题

例如

```js
var obj = {
  name:"abcd",
  arr:["a","b",{
    c:"d",
    e:true
  }]
};
obj._obj = obj;

JSON.stringify(obj);
//Uncaught TypeError: Converting circular structure to JSON

//当重写JSON.stringify后
{
 "name": "abcd",
 "arr": [
  "a",
  "b",
  {
   "c": "d",
   "e": true
  }
 ],
 "_obj": {
  "name": "abcd",
  "arr": [
   "a",
   "b",
   {
    "c": "d",
    "e": true
   }
  ],
  "_obj": {
   "name": "abcd",
   "arr": [
    "a",
    "b",
    {
     "c": "d",
     "e": true
    }
   ],
   "_obj": {
    "name": "abcd",
    "arr": [
     "a",
     "b",
     {
      "c": "d",
      "e": true
     }
    ],
    "_obj": {
     "name": "abcd",
     "arr": [
      "a",
      "b",
      {
       "c": "d",
       "e": true
      }
     ],
     "_obj": {
      "name": "abcd",
      "arr": [
       "a",
       "b",
       {
        "c": "d",
        "e": true
       }
      ],
      "_obj": {
       "name": "abcd",
       "arr": [
        "a",
        "b",
        {
         "c": "d",
         "e": true
        }
       ],
       "_obj": {
        "name": "abcd",
        "arr": [
         "a",
         "b",
         {
          "c": "d",
          "e": true
         }
        ],
        "_obj": {
         "name": "abcd",
         "arr": [
          "a",
          "b",
          {
           "c": "d",
           "e": true
          }
         ],
         "_obj": {
          "name": "abcd",
          "arr": [
           "a",
           "b",
           {
            "c": "d",
            "e": true
           }
          ],
          "_obj": {
           "name": "abcd",
           "arr": [
            "a",
            "b",
            {
             "c": "d",
             "e": true
            }
           ],
           "_obj": {
            "name": "abcd",
            "arr": [
             "a",
             "b",
             {
              "c": "d",
              "e": true
             }
            ],
            "_obj": {
             "name": "abcd",
             "arr": [
              "a",
              "b",
              {
               "c": "d",
               "e": true
              }
             ],
             "_obj": {
              "name": "abcd",
              "arr": [
               "a",
               "b",
               {
                "c": "d",
                "e": true
               }
              ],
              "_obj": {
               "name": "abcd",
               "arr": [
                "a",
                "b",
                {
                 "c": "d",
                 "e": true
                }
               ],
               "_obj": {
                "name": "abcd",
                "arr": [
                 "a",
                 "b",
                 {
                  "c": "d",
                  "e": true
                 }
                ],
                "_obj": {
                 "name": "abcd",
                 "arr": [
                  "a",
                  "b",
                  {
                   "c": "d",
                   "e": true
                  }
                 ],
                 "_obj": {
                  "name": "abcd",
                  "arr": [
                   "a",
                   "b",
                   {
                    "c": "d",
                    "e": true
                   }
                  ],
                  "_obj": {
                   "name": "abcd",
                   "arr": [
                    "a",
                    "b",
                    "[object Object]"
                   ],
                   "_obj": {
                    "name": "abcd",
                    "arr": "[object Array]",
                    "_obj": "[object Object]"
                   }
                  }
                 }
                }
               }
              }
             }
            }
           }
          }
         }
        }
       }
      }
     }
    }
   }
  }
 }
}

```

**使用方式**

在浏览器控制台中粘贴下面的代码然后回车

```js
CustomStringify();
function CustomStringify(){
  const __stringify = JSON.stringify;
  JSON.stringify = stringfiy;
  function stringfiy(value,replacer=function(k,v){return v},space,deep=20){
    let target = generageTarget(value);
    if(shouldIterative(value)){
      for(let j in value){
        compress(target,j,value[j],1);
      }
    }
    return __stringify(target,replacer,space);
    function compress(target,i,value,currentLevel){
      if(currentLevel<deep){
        if(shouldIterative(value)){
          target[i] = generageTarget(value);
          for(let j in value){
            compress(target[i],j,value[j],currentLevel+1)
          }
        }else{
          target[i] = value;
        }
      }else{
        if(shouldIterative(value)){
          target[i] = Object.prototype.toString.call(value);
        }else{
          target[i] = value;
        }
      }
    }
    function generageTarget(value){
      if(isArray(value)){
        return [];
      }else if(isObject(value)){
        return {};
      }else{
        return value;
      }
    }
    function shouldIterative(value){
      return isArray(value)||isObject(value);
    }
    function isArray(value){
      return Object.prototype.toString.call(value)=== "[object Array]";
    }
    function isObject(value){
      return Object.prototype.toString.call(value) ==="[object Object]";
    }
  }
}
```

然后再次使用JSON.stringify的时候就会按照重写的JSON.stringify来执行了

JSON.stringify的参数传入跟原生JSON.stringify基本相同,除了可以额外的传入一个参数来指定最大输出的层级以外,例如

```js
JSON.stringify(obj,undefined,' ',2)
```
