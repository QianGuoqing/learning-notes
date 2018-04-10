## mongoose使用方法

`mongoose`是`Node`操作`mongodb`的一种驱动。有了`mongoose`就可以用`Node`很方便的操作`mongodb`了。首先安装好Node环境和MongoDB，然后使用mongoose的操作步骤如下：

## 操作步骤

- 安装`mongoose`
	
	`npm install --save mongoose`
	
- 引入`mongoose`

	`const mongoose = require('mongoose')`
	
- 使用connect方法连接数据库，同时要知道数据库的路径和数据库名

	```javascript
	const DB_URL = 'mongodb://localhost:27017/db_name'
	mongoose.connect(DB_URL)
	```
	
- 判断数据库是否连接成功

	```javascript
	const DB_URL = 'mongodb://localhost:27017/db_name'
	mongoose.connect(DB_URL, err => {
		if (err) {
			console.log(`连接失败: ${err}`)
		} else {
			console.log(`连接成功`)
		}
	})
	```
	
- 创建Schema
	
	Shema是一种表结构，它用来描述表的内容，表有什么字段。它相当于一种模板，无法直接用来操作数据库，但是只有创建了这个结构，之后才能使用特定的方法来操作数据库。
	
	```javascript
	const userSchema = mongoose.Schema({
		username: { type: String, require: true },
		password: { type: String, require: true },
		age: { type: Number }
	})
	// 这个Schema有三个字段，分别是用户名，密码，年龄。
	```
	
- 创建Model
	
	Model是由Schema创建的，可以用来直接操作数据库
	
	```javascript
	const userModel = mongoose.model('userModel', userSchema)
	```

- 增删改查

	根据刚才创建的`model`就能直接对数据库进行增删改查了。
	
	```javascript
	// 增
	userModel.create({
		username: 'qian guoqing',
		password: 'password',
		age: 20
	}, (err, doc) => {
		if (err) {
			console.log(`Error: ${err}`)
		} else {
			console.log(doc) // 输出刚刚插入的数据
		}
	})
	
	// 查
	// 第一个参数是{},表示输出所有数据
	// 相当于 select * from ...
	userModel.find({}, (err, docs) => { 
		if (err) {
			console.log(err)
		} else {
			console.log(docs) // 输出所有数据
		}
	})
	
	// 相当于select * from xxx where username='qian guoqing'
	userModel.find({ username: 'qian guoqing' }, (err, docs) => { 
		if (err) {
			console.log(err)
		} else {
			console.log(docs) // 输出所有数据
		}
	})
	
	// 查询给定条件的第一个数据
	userModel.findOne({username: 'qian guoqing'}, (err, doc) => {
		if (err) {
			console.log(err)
		} else {
			console.log(doc) // 输出第一个满足条件的数据
		}
	})
	
	// 删
	userModel.remove({ 条件 }, err => {
		if (err) {
			console.log(err)
		}
	})
	
	// 改 (update)
	const condition = { name: 'derrick qian' }
	const update = { $set: { age: 25 } } 
	userModel.update(condition, update, err => {
		if (err) {
			console.log(`update error: ${err}`)
		} else {
			console.log(`update success`)
		}
	})
	```