
# django 주요기능정리
<br><br>
## 이미지 업로드 방법
<br><br>

**model.py**

	image = models.ImageField(blank=True, null=True)


**pip install pillow**


**root 위치에서 media 폴더생성**


**settings.py**

	MEDIA_URL = '/media/'
	MEDIA_ROOT = os.path.join(BASE_DIR, 'media')


**urls.py**

	from django.conf.urls.static import static
	from django.conf import settings
	urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
		

**template** 

	{% if project.image %}
		<img src="{{ project.image.url }}">
	{% endif %}


<br><br>
<hr/>
<br><br>


## ForeignKey(on_delete=) 옵션들
<br><br>

종류 | 동작 | 
---|:---:|
`CASCADE` | 연결된 객체가 지워지면 해당 하위 객체도 같이 삭제
`PROTECT` | 하위 객체가 남아 있다면 연결된 객체가 지워지지 않음
`SET_NULL` | 연결된 객체만 삭제하고 필드 값을 null 로 설정
`SET_DEDAULT` | 연결된 객체만 삭제하고 필드 값을 설정된 기본 값으로 변경
`SET()` | 연결된 객체만 삭제하고 지정한 값으로 변경
`DO_NOTHING` | 아무일도 하지 않음
<br>


<br><br>
<hr/>
<br><br>

## 페이징 처리
<br><br>

제네릭 뷰를 쓸 경우(paginate_by 만 넣어주면 된다)
	
	class PostList(generic.ListView):
	queryset = Post.objects.filter(status=1).order_by('-created_at')
    	template_name = 'blog/index.html'
    	paginate_by = 4
	
	{% if is_paginated %}
	 <nav aria-label="Page navigation conatiner"></nav>
	  <ul class="pagination justify-content-center">
	    {% if page_obj.has_previous %}
	    <li><a href="?page={{ page_obj.previous_page_number }}" class="page-link">&laquo; PREV </a></li>
	    {% endif %}
	    {% if page_obj.has_next %}
	    <li><a href="?page={{ page_obj.next_page_number }}" class="page-link"> NEXT &raquo;</a></li>

	    {% endif %}
	  </ul>
	 </nav>
	{% endif %}
	

함수형으로 쓸 경우
	
	from django.core.paginator import Paginator, PageNotAnInteger, EmptyPage
	
	def PostList(request):
	object_list = Post.objects.filter(status=1).order_by('-created_on')
    	paginator = Paginator(object_list, 3)  # 3 posts in each page
    	page = request.GET.get('page')
	
	try:
        post_list = paginator.page(page)
    	except PageNotAnInteger:
            # If page is not an integer deliver the first page
        	post_list = paginator.page(1)
    	except EmptyPage:
        	# If page is out of range deliver last page of results
        	post_list = paginator.page(paginator.num_pages)
    	return render(request, 'index.html', {'page': page, 'post_list': post_list})
	
	{% if post_list.has_other_pages %}
	  <nav aria-label="Page navigation conatiner"></nav>
	  <ul class="pagination justify-content-center">
	    {% if post_list.has_previous %}
	    <li><a href="?page={{ post_list.previous_page_number }}" class="page-link">&laquo; PREV </a></li>
	    {% endif %}
	    {% if post_list.has_next %}
	    <li><a href="?page={{ post_list.next_page_number }}" class="page-link"> NEXT &raquo;</a></li>
	   {% endif %}
	  </ul>
	  </nav>
	</div>
	{% endif %}
	
	
