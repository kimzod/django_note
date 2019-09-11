# note


## 이미지 업로드 방법

model

	image = models.ImageField(blank=True, null=True)

pip install pillow

root 위치에서 media 폴더생성

settings.py 

	MEDIA_URL = '/media/'
	MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

urls.py

	from django.conf.urls.static import static
	from django.cofng import settings
	urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
	
template 

	{% if project.image %}
		<img src="{{ project.image.url }}">
	{% endif %}




