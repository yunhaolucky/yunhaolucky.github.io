Django Mannul
### Detail page for each object of a class
Suppose we have already created the class("apple")
1. In `view.py`, create `detail` method
  ```python
  from django.shortcuts import render,get_object_or_404
  from apples.model import Apple
  def detail(request,object_id):
	apple = get_object_or_404(Apple, pk=task_id)
	context = {'apple':apple}
	return render(request,'apple/detail.html',context)
  ```
2. in `apple/url.py`, add following line
  ```python
  url(r'^(?P<apple_id>\d+)/$',views.detail,name ='detail')
  ```
3. then we can display item in `template\apple\detail.html`
  ```html
<h1>{{ apple }}</h1>
<a href="{%url 'namespace_url_app:detail' apple.id %}">{{apple.name}}</a>
  ```
