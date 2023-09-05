https://www.linkedin.com/pulse/service-repository-pattern-implementation-django-your-moji-mich/

# Search

get query from request
https://stackoverflow.com/questions/32431115/what-is-the-meaning-of-q-in-request-get-var

src_Django/posts/views.py
```python
class SearchView(View):
    def get(self, request, *args, **kwargs):
        queryset = Post.objects.all()
        query = request.GET.get('q')
        if query:
            queryset = queryset.filter(
                Q(title__icontains=query) |
                Q(overview__icontains=query)
            ).distinct()
        context = {
            'queryset': queryset
        }
        return render(request, 'search_results.html', context)
```
