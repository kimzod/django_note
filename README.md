

## 이미지 업로드 방법


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



<hr/>



## ForeignKey 옵션들


**CASCADE**
*연결된 객체가 지워지면 해당 하위 객체도 같이 삭제*


**PROTECT**
*하위 객체가 남아 있다면 연결된 객체가 지워지지 않음*


**SET_NULL**
*연결된 객체만 삭제하고 필드 값을 null 로 설정*


**SET_DEDAULT**
*연결된 객체만 삭제하고 필드 값을 설정된 기본 값으로 변경*


**SET()**
*연결된 객체만 삭제하고 지정한 값으로 변경*


**DO_NOTHING**
*아무일도 하지 않음*



	



