

  

```javascript
function observe(data) {
        let observer = {}
        let dep = new Dep()
        for (let key in data) {
            
            Object.defineProperty(observer, key, {
                set(newValue) {
                    //一旦更改则触发所有watcher
                    dep.notify(newValue)
                    console.log('this is set')
                    data[key] = newValue
                },
                get() {
                    console.log('this is get')
                    console.log(Dep.target)
                    
                    if (Dep.target) {
                        dep.addSub(Dep.target)
                    }
                    return data[key]
                }
            })
        }
        return observer
    }
    // ---------------------------Dep作用是收集Watcher 调度中心
    class Dep {
        constructor() {
            this.subs = []
        }
        addSub(sub) {
            this.subs.push(sub)
        }
        notify(newValue) {
            this.subs.forEach((sub) => {
                sub.updata(newValue)
            })
        }
    }
    // -------------------------Watcher作用是执行回调   订阅者
    class Watcher {
        constructor(cb) {
            this.callback = cb
        }
        updata(newValue) {
            this.callback(newValue)
        }
    }
    // --------------------------添加watcher
    function watching(obj, key) {
        let cb = (newVaule) => {
            obj[key] = newVaule
        }
        //想了想 还是设置静态方便一些
        Dep.target = new Watcher(cb)

        return obj
    }
    let data = {
        'a': 1
    }
    data = observe(data)
    let watch = watching({}, 'a')
    watch.a = data.a
    data.a = '222'
    console.log(watch.a)
```

