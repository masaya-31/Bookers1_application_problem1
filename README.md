# DMM WEBCAMP 課題1【Bookers1を完成させよう(デバッグ形式)】

デバック実施作業

ーーーーーーーーー

※"Webpacker::Manifest::MissingEntryError in…"のエラーが発生した場合、

$ yarn installのコマンド実行。

ーーーーーーーーー

1.サーバー起動時にYou’re on Rails!が表示される

config/routes.rbにhomes/topへのルートパスの記述追加。

root to: "homes#top"

ーーーーーーーーー

2.サーバー起動時にhomes/top画面に遷移せず、LoadErrorとなる

app/controllers/homes_controller.rbの記載変更。

修正前　　　　　　class Controller < ApplicationController

修正後　　　　　　class HomesController < ApplicationController

ーーーーーーーーー

3.homes/top画面のstartのリンクをクリックしても反応しない

app/views/homes/top.html.erbのstartに遷移先の記述が無いため、記述追加。

修正前　　　　　　<%= link_to "start"%>

修正後　　　　　　<%= link_to "start", books_path %>

ーーーーーーーーー

4. books/index画面でNameError undefined local variable or method `book'のエラー発生

app/views/books/index.html.erbのrenderに変数bookに変数@bookを渡すように記述。

修正前　　　　　　<%= render 'form' %>

修正後　　　　　　<%= render 'form', book: @book %>

ーーーーーーーーー

5. books/index画面でNoMethodError undefined method 'title'のエラー発生

db/schema.rbを確認し、booksにtitleカラムが無いため、カラムの追加。

(1)rails g migration AddTitleToBooks title:stringをターミナルで実行

(2)作成されたマイグレーションファイルにタイトルカラムの記述があることを確認。

(3)rails db:migrateをターミナルで実行

(4)db/schema.rbを確認し、titleカラムが作成されているか確認。

ーーーーーーーーー

6. 空白で投稿するとエラーが出る

app/contorllers/books_controllerのcreateメソッドの記述追加。

修正前

       else
       
      render :index
      
    end

修正後

       else
       
      @books = Book.all
      
      render :index
      
    end

空白時にrenderでindexに戻す記載があるが、renderではindexアクションを実行しないため、render前にBook.allでbookテーブルのデータを全取得する必要がある。

ーーーーーーーーー

7. 5の解決後、再度一覧画面を表示するとUrlGenerationErrorが発生

app/views/books/index.html.erbの18行目のEditの遷移先を変更

修正前　　　　　　<%= link_to 'Edit', edit_book_path, %>

修正後　　　　　　<%= link_to 'Edit', edit_book_path(book.id) %>

editの遷移先にidが指定されていないため、どのeditに遷移したら良いかわからなくなっているため、idの指定記述。

ーーーーーーーーー

8. 編集フォームのupdateボタンが発火しない

app/views/edit.html.erbの<%= render 'form', book: @book %>がformタグの中にあるため、formタグの削除。

ーーーーーーーーー

9. 投稿の編集が行えない

app/controllers/books_controller.rbのupdateメソッドに引数が指定されていないため、引数の指定。

修正前　　　　　　if @book.update()

修正後　　　　　　if @book.update(book_params)

ーーーーーーーーー
