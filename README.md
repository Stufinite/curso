# 課搜（curso）

*`curso`* 是西班牙文的課程，諧音為中文的 *`課搜`*（課程搜尋）  
因此得名

*`課搜`* 使用中興資工 [`普及資料與智慧運算實驗室`](http://web.nchu.edu.tw/~yfan/) 所開發的`KCM、KEM`等文字探勘模型作為輔助工具  
當使用者所搜尋的名稱在資料庫查無符合資料時  
系統會找出 `近似` 於使用者所查詢的 `關鍵字`   
再次查詢資料庫  並且回傳`最符合的相關課程給使用者`

## API usage and Results

api domain：*`http://www.campass.com.tw/`*  
請在api domain後面接上正確的url pattern以及query string  
詳細的參數以及結果請參閱下面介紹

### parameter

* `keyword`：the word you want to query.
* `school`：Course of school you want to search. Below are schools which is available.
  * [已經完成的學校清單](https://docs.google.com/spreadsheets/d/1shRsbpbYUQtol0Q1Gbgdd3xn4dQy0MHkqDfLIUlKPIQ/edit#gid=270187308)


### API usage and Results

API使用方式（下面所寫的是api的URL pattern）<br>
Usage of API (pattern written below is URL pattern)：

1. 簡稱、課程名稱、老師、課號搜尋：  
取得該校課程的課程代碼

  - 範例 (Example)：
    - `http://127.0.0.1:8000/curso/get/search/?keyword=台灣&school=NUTC`：

        ```
        ["D19080", "D16468"]
        ```

2. 複數關鍵字查詢：

  - 範例 (Example)：`http://127.0.0.1:8000/curso/get/search/?keyword=文化+臺灣&school=NUTC`

    ```
    ["D16432"]
    ```

3. 紀錄課程名稱縮寫字：提供 `課程縮寫字`、`課程課程`。  
透過紀錄使用者查詢的行為，紀錄課程縮寫字的語料

  - 範例 (Example)：`http://127.0.0.1:8000/curso/post/incWeight/?keyword=發心&fullTitle=發展心理學`
  - result：

    ```
    {"receive Weight success": 1}
    ```
  - mongo的結果：
    ```
    { "_id" : ObjectId("5985b407f8208ec456aee30e"), "key" : "發心", "value" : { "發展心理學" : 5, "發育心理學" : 3 } }
    ```

## Prerequisities

1. service：need `mongodb`：

  - Linux：`sudo apt-get install mongodb`

## Installing

1. `pip install curso`

## Running & Testing

## Run

1. `settings.py`裏面需要新增curso這個app：
  * add this:
  ```
    INSTALLED_APPS=[
        ...
        ...
        ...
        'curso',
    ]
  ```

2. `urls.py`需要新增下列代碼  把所有search開頭的request都導向到curso這個app：
  * add this:
  ```
  import curso.urls
  urlpatterns += [
      url(r'^search/', include(curso.urls))
  ]
  ```

3. `python manage.py runserver`：即可進入頁面測試curso是否安裝成功。

### Break down into end to end tests

目前還沒寫測試...

## Built With

- djangoApiDec==1.2,
- jieba==0.38,
- pymongo==3.4.0,
- PyPrind==2.9.9,
- requests==2.12.3,
- simplejson==3.10.0,

## Contributors

- **張泰瑋** [david](https://github.com/david30907d)

## License

This package use `MIT` License.

## Acknowledgments

感謝`范耀中`老師的指導
