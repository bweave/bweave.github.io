---
layout:     post
title:      Snippet All the Things
date:       2015-09-06
author:     Brian Weaver
summary:    Create snippets in SublimeText to speed up development.
excerpt: Snippets in SublimeText "... are smart templates that will insert text for you and adapt it to their context." They're super powerful and will save you tons of boilerplate-code authoring time.
category:   developer-efficiency
thumbnail:  file-text
tags:
 - snippets
 - sublimetext
 - laravel
---

<p class="lead"><strong>TLDR;</strong></p>

[Snippets in SublimeText](http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/extensibility/snippets.html) "... are smart templates that will insert text for you and adapt it to their context." They're *super* powerful and will save you tons of boilerplate-code authoring time.

----

You've probably heard of snippets before. Most text editors worth their salt offer some form of saveable, reusable text bits. In SublimeText however, they've got *super-powers*! Multiple placeholder points and regex transformations give you *massive* control over your snippets. We use [Laravel](http://laravel.com) for most projects at work. As such, I'm constantly writing the same old boilerplate code for CRUD operations in my Controllers.

> *Ain't nobody got time for dat!*

Since all good developers are *lazy*, this seemed like the perfect opportunity to put together some snippets to speed up my workflow.

### Controller-Index.sublime-snippet

<p><small>Note: SublimeText uses snippet filenames in its auto-complete pop-up. Super handy!</small></p>

```xml
<snippet>
    <content><![CDATA[
\$${1}s = ${1/(^[a-z])/\U\1/gi}::all();

return View::make('${1}s.index', [
    '${1}s' => \$${1}s,${0}
]);
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>cindex</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>source.php</scope>
</snippet>
```

### Output

```php
$posts = Post::all();

return View::make('posts.index', [
    'posts' => $posts,
]);
```

### The Breakdown

I use this snippet in my ```index()``` methods. Customize to your needs, of course. It's simple:

- Fetch all of a Model's records
- Return a view with the data

Now, anytime you're authoring an ```index()``` method in a Laravel controller, simply type **cindex** (you know, **c**ontroller **index** ;-)) followed by the **Tab** key, type in your model name, ie. **post**, and *voila*, ```index()``` method done!

You may have noticed that gnarly looking regex in the snippet. That's the part that handles capitalization where necessary. It's super handy because all we have to type is "post" and everything is taken care of. We can move on with our lives!

Here are the steps to create a snippet, in case you're unfamiliar:

- Click *Tools* -> *New Snippet* in the menubar.
- Replace the entire contents of the new file with the code above.
- Save your new snippet with the ```.sublime-snippet``` extension.

### Going Further

Here's an example of a ```destroy()``` method that provides a JSON response instead of returning a view. It uses multiple placeholder points and nested placeholders to allow you to customize your messages back to the client. I saved this one as **Controller Destroy JSON.sublime-snippet** so that I can quickly choose between it and it's view returning counterpart in the auto-complete pop-up as I type. And in case it wasn't obvious, just **Tab** from placeholder point to placeholder point when using the snippet.

```xml
<snippet>
    <content><![CDATA[
try {
    \$${1} = ${1/(^[a-z])/\U\1/gi}::findOrFail(\$id);
} catch (ModelNotFoundException \$e) {
    return Response::json([
        'code' => 404,
        'message' => "${2:Uh-oh, we couldn't find the ${1/(^[a-z])/\U\1/gi} you were looking for.}",
        'error' => \$e->getMessage(),
    ], 404);
}

if (!\$${1}->delete()) {
    return Response::json([
        'code' => 500,
        'message' => "${3:Uh-oh, something went wrong deleting the ${1/(^[a-z])/\U\1/gi}!}",
    ], 500);
}

return Response::json([
    'code' => 200,
    'message' => "${4:${1/(^[a-z])/\U\1/gi} deleted successfully!}",
], 200);
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>cdestroyjson</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>source.php</scope>
</snippet>
```

### Output

```php
try {
    $post = Post::findOrFail($id);
} catch (ModelNotFoundException $e) {
    return Response::json([
        'code' => 404,
        'message' => "Uh-oh, we couldn't find the Post you were looking for.",
        'error' => $e->getMessage(),
    ], 404);
}

if (!$post->delete()) {
    return Response::json([
        'code' => 500,
        'message' => "Uh-oh, something went wrong deleting the Post!",
    ], 500);
}

return Response::json([
    'code' => 200,
    'message' => "Post deleted successfully!",
], 200);
```
### Conclusion

Stop wasting time writing the same code over, and over! Put it in a snippet and do something better with your time!
