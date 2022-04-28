---
title: 【Ruby】小白學Rails：門口阿桑Routes
tags: 文章發布,ROR
description: RubyOnRails裡的MVC架構是甚麼
date: 2022/04/28
---
<br/>
<center><img src="https://i.imgur.com/9ntpDwL.jpg
"></center>
<center>Photo by <a href="https://unsplash.com/@anniespratt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Annie Spratt</a> on <a href="https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a></center>

<br/>
>前情提要：此為初入新手村的超級菜鳥，探索Ruby on Rails的世界，僅供參考~ 敬請指教修正:bow::bow::bow:

## 門口阿桑Routes你聽過嗎

Route 是整個網站對外公開的網站路徑對照表，當使用者連上你的網站的時候，Rails 會解析使用者所輸入的網址及參數，再根據解析的結果，去找到該負責處理的單位（哪個 Controller 跟 Action）。

>Route: 中文翻譯為「路由」，類似網址路徑，但它並不是網址（URL）本身，而是定義「網址」的「連線方式」以及「行為」的組合。
>1. 要去哪裡（顯示的網址）
>2. 去的方式（HTTP動詞）
>3. 指引到相對應的控制中心（Controller#Action）  
>如果不小心設定了兩條相同的routes時，會以**前者為主，後者被覆蓋**

### R(route)+MVC
在MVC架構裡面，Route是第一線處理request的人，很重要唷!!!Route 是告知這個需求要去找哪個 Controller 的哪個 Action。
但即使 Route 有這條路徑，也不表示那個 Controller 檔案會先存在；就算 Controller 存在，也不表示 Action 存在。

![](https://i.imgur.com/Znrm69D.png)
<center><font color="#aaa">資料來源：五倍紅寶石-為你自己學 Ruby on Rails</font></center>

<br/>

### 指定路由
指定網址與連線方式，並引導到特定 controller 中的 action。
```ruby
#routes.rb
get "/candidates", to: "candidates#index"
```
1. 網址列顯示的網址："/candidates"
2. 連線方式HTTP動詞：get
3. Controller與Action：到 candidates 的 controller，執行 index 這個 action 的內容
```ruby
#condidates_controller.rb
class CandidatesController < ApplicationController
    def index
        #符合條件後執行的內容
    end
end
```

<br/>

### Route的八條路徑與七個action

Routes 都是在 routes.rb 檔案裡定義。
符合 :mag_right:RESTful 風格的路由產生器，只要下一條指令就可以產生8條路徑與7個 action，提供 HTTP 動詞、URL、Controller 動作，這三者的對應關係。按照<mark>慣例</mark>，每個動作會對應到資料庫特定的 <mark>CRUD</mark> 操作。假設路由檔案裡有一條路由宣告為：

> `resources :photos` 可以自動產生與photo有關的八條路徑

| Helper | HTTP動詞 | /路徑 | Controller＃Action |
| -------- | -------- | -------- | ------- |
| photos_path | GET | /photos | photos#index |
|             | POST | /photos | photos#create |
| new_photo_path | GET | /photos/new | photos#new |
|edit_photo_path(:id) | GET | /photos/:id/edit | photos#edit |
| photo_path(:id) | GET | /photos/:id | photos#show | 
|                 | PATCH | /photos/id | photos#update |
|                 | PUT | /photos/:id | photos#update |
|           | DELETE | /photos/:id | photos#destroy |

從終端機我們可以使用 `rails routes -c photos` 這個指令檢查 photos 路由的狀態。（helper如果空白則同上，可以搭配:mag_right:linK_to使用）

> :bulb: **PATCH 和 PUT 的差別**：  
>兩者都是修改 update，但 patch的目的通常指用來修改部分欄位，沒有修改到的欄位值會被保留下來；而 put 則是修改所有欄位，沒有被修改到的欄位值全部會被改為nil。  
>PATCH 用來修改部分資料  
>PUT   用來替換資料(reolace)

<br/>

![](https://i.imgur.com/HwErDSG.png)
<center><font color="#aaa">筆記：by myself</font></center>

<br/>


在 Rails 中我們可以使用 resources 或 resource 搭配 only: 或 except: 自動幫我們產生一系列的路由。

- 複數的Resources 有id 【Resource Routing】  
`resources :[table_names], :[table_names]`

    - 通常用在管理員可以檢視、新增、修改、刪除會員資料
    - 複數的 resources 搭配複數的 table_names 使用

```ruby
# 複數的 resources [:index, :show, :new, :create, :edit, :update, :destroy]
Rails.application.routes.draw do
    resources :photos #快速產生一系列的 CRUD routes 設定
    resources :photos, only: [:index, :show] #只需要顯示資料
    resources :photos, except: [:new, :create, :edit, :update, :destroy] # 不需要新增、修改、刪除
end
```

- 單數的Resource 無id 【Singular Resoruce】  
`resource :[table_name], :[table_name]`

    - 不需要參照id的時候可以使用
    - 通常用在使用者可以檢視、新增、修改、刪除單筆資訊
    - 單數的 resource 搭配單數的 table_name 使用

```ruby
# 單數的 resource  [:show, :new, :create, :edit, :update, :destroy]
Rails.application.routes.draw do
  resource :profile
end
```

<br/>

#### member與collection
不受限於七個預設產生的資源式路由。還可以新增更多集合路由、成員路由。
如果覺得內建的 Resources 產生的 CRUD 不夠用，我們也可以使用 collection 或 member 的方法。

- 使用 collection 方法做出來的路徑不會有：id
- 使用 member 方式產生的路徑，會帶有 :id 在裡面，這個 :id 會傳到 Controller 裡變成 params 這個變數的一部份。

<br/>

---
>The Rails router recognizes URLs and dispatches them to a controller's action, or to a Rack application. It can also generate paths and URLs, avoiding the need to hardcode strings in your views.
>[Rails Guide官方手冊](https://guides.rubyonrails.org/routing.html)

<br/>
以上，就是門口阿桑 Routes 的粗淺介紹。當你的 Routes 對了～再來就是 controller 以及 action 。最後會倒到相對應的 view 與 model。

<br/>
<br/>

---
<spoiler> **參考資料**
- [Model、View、Controller 三分天下](https://railsbook.tw/chapters/10-mvc)(為你自己學 Ruby on Rails 高見龍)
- [Routes](https://railsbook.tw/chapters/11-routes)(為你自己學 Ruby on Rails 高見龍)
- [Rails 路由：深入淺出](https://rails.ruby.tw/routing.html)
- [[Rails] routes 的使用](https://pjchender.dev/ruby-on-rails/ruby-on-rails-routes/)
</spoiler>