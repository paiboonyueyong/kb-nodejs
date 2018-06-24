# kb-nodejs

# ทำ RESTful API ด้วย Node.js และ Express อย่างง่าย

Express คือ web application framework บน Node.js


Start ...

    go to project path >> D:\Play\Project\2018\School\Node\App\contact-chain-api

Initail npm (-y = use default from npm , no question)

    npm init -y  

Install express

    npm install express --save

create file server.js

create file db.json

```json
    [
      {
        "id": "1",
        "name": "Game of thrones"
      },
      {
        "id": "2",
        "name": "Clash of kings"
      }
    ]
```

Install middleware for parse body message

    npm install body-parser --save

    Middleware 
    Filter request  before arriving at our application. use >> app.use() 

Run application

    node server.js


## Sample source code in file server.js

```javascript
const express = require('express')
const app = express()
const books = require('./db')
const bodyParser = require('body-parser')

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: true }))

app.get('/', (req, res) => {
    res.send('Hello World')
})


//GET /books ขอข้อมูลหนังสือทั้งหมด
app.get('/books', (req, res) => {
    res.json(books)
})

//GET /books/1 ขอข้อมูลหนังสือไอดีที่ 1
app.get('/books/:id', (req, res) => {
    res.json(books.find(book => book.id === req.params.id))
})

//POST /books สร้างหนังสือ
app.post('/books', (req, res) => {
    books.push(req.body)
    res.status(201).json(req.body)
})

//PUT /books/ 1 แก้ไขหนังสือไอดีที่ 1
app.put('/books/:id', (req, res) => {
    const updateIndex = books.findIndex(book => book.id === req.params.id)
    res.json(Object.assign(books[updateIndex], req.body))
})

//DELETE /books/ 1 ลบหนังสือไอดีที่ 1
app.delete('/books/:id', (req, res) => {
    const deletedIndex = books.findIndex(book => book.id === req.params.id)
    books.splice(deletedIndex, 1)
    res.status(204).send()
})

app.listen(13000, () => {
    console.log('Start server at port 13000.')
})

```


## ศึกษาเพิ่มเติม
* การ handle errors เช่น ถ้าเซฟลง database ไม่ได้จะส่ง response ไปบอกอย่างไร
* การ validate request เช่น ต้องส่งฟีลด์อะไรมาบ้าง แต่ละฟีลด์เป็นข้อมูลชนิดไหน
* การจัดโฟลเดอร์และไฟล์ เพราะตอนนี้เราเขียนอยู่ในไฟล์เดียว เมื่อโค้ดโตขึ้นจะแบ่งอย่างไรให้ใช้งานได้สะดวก
* การเขียน Document เพื่อบอกว่า API ของเรามีอะไรให้ใช้ได้บ้าง ต้องส่งอะไรมาบ้าง เพื่อให้ผู้ใช้สามารถนำไปใช้งานได้

Reference : 

    https://medium.com/@aofleejay/%E0%B8%AA%E0%B8%A3%E0%B9%89%E0%B8%B2%E0%B8%87-restful-api-%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-express-express-101-ee37cc4952b4
