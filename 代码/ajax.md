

 

```javascript
 const ajax = (option) => {
        let xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject()
        let params = (data) => {
            let arr = []
            for (let i in data) {
                arr.push(`${i}=${data[i]}`)
            }
            return arr.join('&')
        }
    }
    let param = params(option.data)
    if (option.method === 'Post') {
        xhr.open('Post', data.url, true)
        xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded')
        xhr.send(param)
    }
    if (option.method === 'Get') {
        xhr.open('Get', data.url + '?' + param, true)
        xhr.send(null);
    }
    xhr.onreadystatechange = function () {
        if (xhr.readtState == 4 && xhr.status == 200) {
            option.success()
        }
    }
```

