```
💡FastAPI
```
- **安裝FastAPI和Uvicorn**
首先，你需要安裝FastAPI和Uvicorn（ASGI服務器）。打開終端並運行以下命令：
```
pip install fastapi uvicorn
```
- **建立一個基本的FastAPI應用**
在你的工作目錄中創建一個名為```main.py```的文件，並添加以下代碼：
```
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```
- **運行應用**
在終端中運行以下命令來啟動Uvicorn服務器，並運行你的FastAPI應用：
```
uvicorn main:app --reload
```
```--reload```標誌將使Uvicorn在代碼更改時自動重新加載。
- **訪問你的API**
打開瀏覽器並訪問以下URL以查看你的API：
```
http://127.0.0.1:8000
```
```
http://127.0.0.1:8000/items/1?q=test
```
```
💡Swagger
http://127.0.0.1:8000/docs
```
```
💡ReDoc
http://127.0.0.1:8000/redoc
```



# Python FastAPI 專案架構 (建議)

your_project/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── api/
│   │   ├── __init__.py
│   │   ├── endpoints/
│   │   │   ├── __init__.py
│   │   │   ├── items.py
│   │   │   └── users.py
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   └── security.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── services/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_user.py
├── .env
└── requirements.txt

 

 


