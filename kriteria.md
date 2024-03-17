# BOOKSHELF API CRITERIA
## STRUCTURE DATA
```json
{
    id: string,
    name: string,
    year: number,
    author: string,
    summary: string,
    publisher: string,
    pageCount: number,
    readPage: number,
    finished: boolean,
    reading: boolean,
    insertedAt: string,
    updatedAt: string
}
```
### Certain
1. <b>id</b>, should be unique, we can use nanoid version 3.
    ```bash
    npm install nanoid@3
    ```
2. <b>finished</b>, The finished value is obtained from the observation pageCount === readPage.
3. <b>insertedAt</b>, is a property that holds the date the book was entered. we can use 
> new Date().toISOString()  
4. <b>updatedAt</b>, is a property that hold the date when book just entered or updated, <i>for book just entered</i> we can use value of <b>insertedAt</b>


### Certain 
#### Use Port <b>9000</b>
#### Application running using npm start: 
```json
{
    ...
    "scripts": {
        "start": "node src/server.js",
        "start:dev": "nodemon src/server.js"
    }
}
```
#### Add Book   
method: <b>POST</b>   
URL: <b>/books</b>   
Body request: 
```json
{
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
}
```
response error: 
1. Client does not attach name property on body request
    1. status code: <b>400</b>
    2. response body: 
    ``` json 
    {
        "status": "fail",
        "message": "Gagal menambahkan buku. Mohon isi nama buku"
    }
    ```
2. Client attach value of property readPage > pageCount
    1. status code: <b>400</b>
    2. response body: 
    ```json
    {
        "status": "fail",
        "message": "Gagal menambahkan buku. readPage tidak boleh lebih besar dari pageCount"
    }
    ```

response success:
1. status code: <b>201</b>
2. response body: 
```json 
{
    "status": "success",
    "message": "Buku berhasil ditambahkan",
    "data": {
        "bookId": "xxxxx"
    }
}
```
#### List all books
method: <b>GET</b>    
URL: <b>/books</b>   

response success: 
1. status code: <b>200</b>
2. response body: 
```json
{
    "status": "success",
    "data": {
        "books": [
            {
                "id": "xxx",
                "name": "xxx",
                "publisher": "xxx",
            },
            ...
        ]
    }
}
```

response no books: 
```json 
{
    "status": "success",
    "data": {
        "books": []
    }
}
```

#### Get spesific book
method: <b>GET</b>   
URL: <b>/books/{bookId}</b>    

response success:   
1. status code <b>200</b>
2. response body: 
```json 
{
    "status": "success",
    "data": {
        "book": {
            "id": "xxxx",
            "name": "Buku A Revisi",
            "year": 2011,
            "author": "Jane Doe",
            "summary": "Lorem Dolor sit Amet",
            "publisher": "xxx",
            "pageCount": 200,
            "readPage": 26,
            "finished": false,
            "reading": false,
            "insertedAt": "2021-03-05T06:14:28.930Z",
            "updatedAt": "2021-03-05T06:14:30.718Z"
        }
    }
}
```

response error: 
1. status code <b>404</b>
2. response body: 
```json 
{
    "status": "fail",
    "message": "Buku tidak ditemukan"
}
```

#### Update book
method: <b>PUT</b>   
URL: <b>/books/{bookId}</b>   
body request: 
```json
{
    "name": string,
    "year": number,
    "author": string,
    "summary": string,
    "publisher": string,
    "pageCount": number,
    "readPage": number,
    "reading": boolean
}
```
response success: 
1. status code <b>200</b>   
2. response body
```json 
{
    "status": "success",
    "message": "Buku berhasil diperbarui"
}
```

response error: 
1. Client does not attach property name:
    1. status code <b>400</b>   
    2. response body: 
    ```json 
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. Mohon isi nama buku"
    }
    ```

2. Client attach value of readPage > pageCount:
    1. status code <b>400</b>   
    2. response body: 
    ```json
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. readPage tidak boleh lebih besar dari pageCount"
    }
    ```

3. Client attach Id, not found.
    1. status code <b> 400</b>
    2. response body: 
    ```json 
    {
        "status": "fail",
        "message": "Gagal memperbarui buku. Id tidak ditemukan"
    }
    ```

#### Deleted book
method: <b>DELETE</b>   
URL:  <b>/books/{bookId}</b>   

response success: 
1. status code <b>200</b>   
2. response body: 
```json 
{
    "status": "success",
    "message": "Buku berhasil dihapus"
}
```

response error: 
1. status code <b>404</b>   
2. response body: 
```json 
{
    "status": "fail",
    "message": "Buku gagal dihapus. Id tidak ditemukan"
}
```