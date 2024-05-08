# 一、Student-Information-Form
本次案例，我们尽量减少dom操作，采取操作数据的形式 增加和删除都是针对于数组的操作，然后根据数组数据渲染页面
# 二、学生信息表
## 核心思路：
### ①： 声明一个空的数组
const arr = []
### ②： 点击录入模块
(1). 首先取消表单默认提交事件
`e.preventDefault()`
(2). 创建新的对象，里面存储 表单获取过来的数据
```javascript
 const obj = {
        stuId: arr.length + 1,
        uname: uname.value,
        age: age.value,
        gender: gender.value,
        salary: salary.value,
        city: city.value
      }
```
(3). 追加给数组
`arr.push(obj)`
(4). 渲染数据。 遍历数组， 动态生成tr， 里面填写对应td数据， 并追加给 tbody
```javascript
for (let i = 0; i < arr.length; i++) {
        // 生成 tr 
        const tr = document.createElement('tr')
        tr.innerHTML = `
          <td>${arr[i].stuId}</td>
          <td>${arr[i].uname}</td>
          <td>${arr[i].age}</td>
          <td>${arr[i].gender}</td>
          <td>${arr[i].salary}</td>
          <td>${arr[i].city}</td>
          <td>
            <a href="javascript:">删除</a>
          </td>
        `
        // 追加元素  父元素.appendChild(子元素)
        tbody.appendChild(tr)
      }
```
(5). 重置表单
`info.reset()`
(6). 注意防止多次生成多条数据，先清空 tbody
`tbody.innerHTML = ''`

### ③： 点击删除模块
(1). 采用事件委托形式，给 tbody 注册点击事件
` tbody.addEventListener('click', function (e) {}`
(2). 点击链接，要删除的是对应数组里面的这个数据，而不是删除dom节点，如何找到这个数据？
`if (e.target.tagName === 'A') {}`
(3). 前面渲染数据的时候，动态给a链接添加 **自定义属性** data-id=“0”,这样点击当前对象就知道索引号了
`<td><a href="javascript:" data-id=${i}>删除</a></td>`
(4). 根据索引号，利用 splice 删除这条数据
`arr.splice(e.target.dataset.id, 1)`
(5). 重新渲染
`render()`
### ④： 点击新增需要验证表单
(1). 获取所有需要填写的表单， 他们共同特点都有 name属性（**属性选择器**）
`const items = document.querySelectorAll('[name]')`
(2). 遍历这些表单，如果有一个值为空，则return 返回提示输入为空中断程序
```javascript
      for (let i = 0; i < items.length; i++) {
        if (items[i].value === '') {
          return alert('输入内容不能为空')
        }
      }
```
(3). 注意书写的位置，应该放到**新增数据的前面， 阻止默认行为的后面**
### 示例图：
[![student](https://img.17carat.cn/2024/04/student.png "student")](https://img.17carat.cn/2024/04/student.png "student")
