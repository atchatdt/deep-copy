# deep-copy
```
function getTypeObj(obj) {

    // Lấy về kiểu của obj => toString
    // call thì để thay đổi context
    let str = Object.prototype.toString.call(obj)
    const map = {
        "[object Boolean]": "boolean",
        "[object Number]": "number",
        "[object String]": "string",
        "[object Function]": "function",
        "[object Array]": "array",
        "[object Date]": "date",
        "[object RegExp]": "regExp",
        "[object Undefined]": "undefined",
        "[object Null]": "null",
        "[object Object]": "object",
        "[object Set]": "set",
        "[object Map]": "map"
    }
    return map[str]
}

// Xác định thuộc định dạng nào để chuyển tới function xử lý tương ứng
function deepCopy(obj) {
    let type = getTypeObj(obj)
    let copy;
    switch (type) {
        case "array":
            return copyArray(obj, copy)
        case "function":
            return copyFunction(obj, copy)
        case "object":
            return copyObject(obj, copy)
        case "set":
            return copySet(obj, copy)
        case "map":
            return copyMap(obj, copy)
        default:
            return obj
    }
}

// Copy array
function copyArray(obj, copy = []) {
    for (const [index, value] of obj.entries()) {
        copy[index] = deepCopy(value)
    }
    return copy
}

// Copy function
function copyFunction(obj, copy = []) {
    const fun = eval(obj.toString())
    fun.prototype = obj.prototype
    return fun
}

// Copy object
function copyObject(obj, copy = {}) {
    for (const [key, value] of Object.entries(obj)) {
        copy[key] = deepCopy(value)
    }
    return copy
}

// Copy set
function copySet(obj, copy = new Set([])) {
    for (let item of obj) {
        let result = deepCopy(item)
        copy.add(result)
    }
    return copy
}

// Copy map
function copyMap(obj, copy = new Map([])) {
    for (let [key, value] of obj.entries()) {
        copy[key] = deepCopy(value)
    }
    return copy
}

const obj1 = {
    name: 'kuro',
    job: {
        dev: {
            fe: ['html', 'css', 'js'],
            be: ['Nodejs'],
            fw: ['Reactjs', 'express']
        }
    },
    set: new Set([1, 2, 3, 4, 4, 5, 5, { x: 10 }]),
    a: Symbol(10),
    map: new Map([['a', 1], ['b', 2]])
}
console.log(obj1)
const obj2 = deepCopy(obj1)
console.log(obj2.map)
console.log(obj1.map)

```
