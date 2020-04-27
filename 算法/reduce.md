 

#  数组翻转

```javascript
let a = [1,2,3,55,6] 

1. 直接用数组的 reverse() 方法
a.reverse()

2.用reduceRight 
function Rerverse(arr) {
    return arr.reduceRight((total,item)=>{
        total.push(item)
        return total
    },[])
}

3.运用unshift
   function Rerverse(arr) {
            let res = []
            arr.forEach((item) => {
                res.unshift(item)
            })
            return res
        }
4.数组元素交换
	    function Rerverse(arr) {
            let len = arr.length
            for (let i = 0; i < len / 2; i++) {
                [arr[i], arr[len - i - 1]] = [arr[len - 1 - i], arr[i]]
            }
            return arr
        }
```



# 数组扁平

```javascript
let arr = [[1],[2],3,[4,5,6]]

1.用reduce
        function flat(arr) {
            return arr.reduce((total, item) => {
                //如果是数组则 递归调用
                return total.concat(Array.isArray(item) ? flat(item) : item)
            }, [])
        }

2.利用栈
function flat(arr) {
            let res = []
            //浅拷贝
            let stack = [...arr]
            while (stack.length > 0) {
                let val = stack.pop()
                if (Array.isArray(val)) {
                    stack.push(...val)
                } else {
                    res.unshift(val)
                }
            }
            return res
        }

3.利用some来判断
  function flat(arr) {
            while (arr.some((item) => {
                return Array.isArray(item)
            })) {
                console.log(arr)
                arr = [].concat(...arr)
            }
            return arr
        }
```

# 数组去重

 

```javascript
 1.用reduce
function uniq(arr) {
            return arr.reduce((total, item) => {
                if (!total.includes(item)) {
                    total.push(item)
                    return total
                } else {
                    return total
                }
            }, [])
        }

2. 用set
 arr = Array.from(new Set(arr))

3.利用对象属性去重

 
```

