# DRF get_queryset(related_name)
```
def get_queryset(self):
    return self.request.user.jobs.filter(
        Q(review_status=0) | Q(review_status=1) | Q(review_status=3))
```
> jobs为related_name
