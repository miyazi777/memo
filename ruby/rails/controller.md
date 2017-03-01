
## redirect_to

redirect_to 遷移先URL, パラメータ

```
redirect_to action: "show", id: 5
redirect_to post
redirect_to "http://www.rubyonrails.org"
redirect_to "/images/screenshot.jpg"
redirect_to articles_url
redirect_to proc { edit_post_url(@post) }
```

## rendar

rendar 遷移先, パラメータ

```
render action: "show"
```
