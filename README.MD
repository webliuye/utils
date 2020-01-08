# 这是一段处理数据长度和单位换算的方法
### 单位针对KB做了特殊处理1024换算，其他均为10000.针对小数点取两位，针对取两位后尾数0时去0处理
```
 function LongNumberToShort (num, unit) {
  if (!num) {
    return {
      num,
      unit
    }
  }
  let moneyUnits = ['', '万', '亿', '万亿']
  let dividend = 10000
  let divideLength = 5
  if (unit === 'KB') { // 针对字节的转换
    moneyUnits = ['KB', 'MB', 'GB', 'TB']
    dividend = 1024
    divideLength = 4
  }

  let curentNum = num
  let curentUnit = moneyUnits[0]
  for (var i = 0; i < moneyUnits.length; i++) {
    curentUnit = moneyUnits[i]
    if (strNumSize(curentNum) < divideLength) {
      break
    }
    curentNum = curentNum / dividend
  }
  const m = {
    num: 0,
    unit: '',
    str: ''
  }
  m.num = curentNum
  const numStrArr = curentNum.toString().split('.')
  if (numStrArr[1] && numStrArr[1].length > 2) { // 小数后面大于2才省略
    m.num = curentNum.toFixed(2)
    const str = String(m.num)
    const beforePoint = str.split('.')[0]
    const afterPoint = str.split('.')[1]
    if (afterPoint[1] === '0') { // 针对尾号为0的省略
      if (afterPoint[0] === '0') {
        m.num = +beforePoint
      } else {
        m.num = +(beforePoint + '.' + afterPoint[0])
      }
    }
  }
  if (unit !== 'KB') {
    unit = unit || ''
    m.unit = curentUnit + unit
  } else {
    m.unit = curentUnit
  }
  m.str = m.num + m.unit
  return m
}
```
#### 使用方法返回一个包含三个属性的对象。值(num)和单位(unit)的拼接字符串(str)
```
  const {str ,num ,unit } = LongNumberToShort(458640,'张')

```
### 返回值是 45.86万张 ；45.86 ， 万张 

## 水平有限欢迎大佬指正