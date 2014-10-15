### Start a project###

export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python2.7/site-packages
* create a project named plan
```
django-admin startproject plan
```

* creating a app named task
```
python manage.py startapp task
```

* create a `task` model in `/task/model.py`
```python
class Task(models.Model):
	name = models.CharField(max_length=200)
	create_date = models.DateTimeField(auto_now_add=True)
	deadline = models.DateTimeField('deadline')
	duration = models.IntegerField()
	parent = models.ForeignKey('self', null =True)
```
* Migrates to the database
```
 python manage.py makemigrations task
 python manage.py migrate

 ```
 *Play With API
```python
import django
from task.models import Task
Task.objects.all()
from django.utils import timezone
t = Task(name = "task test",deadline = timezone.now(),duration = 1000)
t.save()
t.id
t.create_time
Task.objects.all()
```

* Set foreign key and check task_list
```python
#Add a new task
t.task_set.create(name = "task test",deadline = timezone.now(),duration = 1000)
# Get all's children
t.task_set.all()
```
* Add 3 function method in model
```python
	def __unicode__(self):
		return `self.id` + ':' + self.name
	def is_root_task(self):
		return self.parent == None
	def is_leaf_task(self):
		return self.task_set.count() == 0
```

### Add admin
* add superuser
```
python manage.py createsuperuser
yunhao
yunhao
```
* customerize admin page in `task/admin.py`
```python
from django.contrib import admin
from task.models import Task
# Register your models here.
class Taskinline(admin.TabularInline):
	model = Task
	extra = 3
class TaskAdmin(admin.ModelAdmin):
	fieldsets = [
	('Basic information',	{'fields':['name']}),
	('Date information',	{'fields':['deadline','duration']}),
	('Parents',	{'fields':['parent'],'classes': ['collapse']}),
	]
	inlines = [Taskinline]
	list_display = ('name', 'create_date','is_root_task')
	list_filter = ['create_date']
admin.site.register(Task,TaskAdmin)
```
* also add following to the 'task/model.py'
```python
	is_published_recently.admin_order_field = 'create_date'
	is_published_recently.boolean = True
	is_published_recently.short_description = 'Create in 24 hours?'
```
The current data model is
![hour_count_boxplot](https://docs.google.com/uc?export=view&id=0B47woKFE0zXeVzY4TkhpeU16d0k)

### Set urls and views ###
* create a index url in `/task/urls.py`

```python
from django.conf.urls import patterns, url
from task import views
urlpatterns = patterns('',
    url(r'^$', views.index, name='index'),
)
```
Also import `task/urls` into `urls.py`
```python
urlpatterns = patterns('',
...
url(r'^task/',include('tasks.urls')),
)
```
Route Url contains parameters
Add methods in `views.py`
```python
def detail(request,task_id):
	response = "You're are looking at the details of a task %s."
	return HttpResponse(response % task_id)

def add_task(request,task_id):
	return HttpResponse("You're add substasks on question %s."%task_id)
```
And to capture the corresponding parameter in url, We add following method in urls.py
```python
    url(r'^(?P<task_id>\d+)/$',views.detail,name ='detail'),
    # ex:/task/5/add
    url(r'(?P<task_id>\d+)/add/$',views.add_task,name = 'add_task'),
```
### From view to template ###
 Add following to view.py
```python
'''
from django.template import RequestContext, loader
def show_task(request):
	latest_task_list = Task.objects.order_by('-create_date')[:5]
	template = loader.get_template('task/index.html')
	context = RequestContext(request,{
		'latest_task_list':latest_task_list,
		})
	return HttpResponse(template.render(context))
'''
def show_task(request):
	latest_task_list = Task.objects.order_by('-create_date')[:5]
	context = {'latest_task_list':latest_task_list}
	return render(request, 'task/index.html',context)
'''
from django.http import Http404
def detail(request,task_id):
	response = "You're are looking at the details of a task %s."
	try:
		task = Task.objects.get(pk=task_id)
	except Task.DoesNotExist:
		raise Http404
	context = {'task':task}
	return render(request,'task/detail.html',context)
'''
def detail(request,task_id):
	task = get_object_or_404(Task, id=task_id)
	context = {'task':task}
	return render(request,'task/detail.html',context)
```
