Django позволяет поместить параметр непосредственно в URL адресе (вместо того, чтобы явно прописывать ?q=...). 

___
```
# На примере билборда
URL: board/<rubric-key>/

# In {application}.urls
urlpatterns = [
	path('', main),
	path('<int:rubric_id>/', by_rubric) # идентифицируется int в качестве рубрики
]

# In {application}.views
def by_rubric(request, rubric_id): # параметр передается в контроллер
	[...]
```

___
Relations: [[Django Python]] 
Tags: #code
References: 
Query: 