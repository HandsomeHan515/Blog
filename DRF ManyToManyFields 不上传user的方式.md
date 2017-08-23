#  DRF ManyToManyFields 创建时不需上传User
  
    在view中添加该代码
```
def perform_create(self, serializer):
    serializer.save(user=self.request.user)
```
