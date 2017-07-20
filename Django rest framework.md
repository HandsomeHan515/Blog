### 处理嵌套对象
```
// serializers.py
class UserSerializer(serializers.ModelSerializer):
    username = serializers.CharField(max_length=300)
    email = serializers.EmailField()
    
class CommentSerializer(serializers.ModelSerializer):
    user = UserSerialzer()
    content = serializers.CharField(max_length=1024)
    time= serializers.DateTimeField()
```
    如果嵌套表示可以选择接受该None值，应该将该required=False标志传递给嵌套的序列化程序。
```
    user = UserSerializer(required=False)
```
    如果嵌套表示应该是项目列表，则应将该many=True标志传递给嵌套的序列化。
```
    edits = EditSerializer(many=True)
```

#### 编写.create()的嵌套表示方法
```
class UserSerializer(serializers.ModelSerializer):
    profile = ProfileSerializer()
    
    class Meta:
        model = User
        fields = ('username', 'email', 'profile')
        
    def create(self, validated_data):
        profile_data = validated_data.pop('profile')
        user = User.objects.create(**validated_data)
        Profile.objects.create(user=user, **profile_data)
        return user
            
```

#### 编写.update()的嵌套表示方法
```
def update(self, instance, validate_data):
    profile_data = validated_data.pop('profile')
    profile = instance.profile
    instance.username = validated_data.get('username', instance.username)
    instance.email = validated_data.get('email', instance.email)
    instance.save()
    
    profile.title = profile_data.get('title', profile.title)
    profile.description = profile_data.get('description', profile.description)
    profile.save()
    return instance
```
