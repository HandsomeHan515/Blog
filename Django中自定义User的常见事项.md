# 使用AbstractBaseUser自定义User需注意的事项
## 必须设置的值
### USERNAME_FIELD
    必须设置，设置认证标识，设置成标识的字段unique=True;
```
class Users(AbstractBaseUser):
    username = CharField(max_length=128, unique=True)
    ...
    USERNAME_FIELD = 'username'
```
### REQUIRED_FIELDS
    必须设置，通过createsuperuser管理命令创建一个新用户时，用于提示的一个字段名称列表。
```
class Users(AbstractBaseUser):
    nickname = CharField(max_length=256)
    ...
    REQUIRED_FIELDS = ['nickname']
```
### is_active
    必须定义。一个布尔属性，标识用户是否是“active”的，AbstractBaseUser默认是true。
### get_full_name()
    必须定义，long格式的用户标识
```
def get_full_name(self):
    return self.username
```
### get_short_name()
    必须定义，short格式的用户标识
```
def get_short_name(self):
    return self.username
```
## 可以使用的方法
### get_username()
    返回USERNAME_FIELDS的值
### is_staff
    是否允许user访问到admin界面
```
@property
def is_staff(self):
    return True
```
### has_perm(perm, obj)
    用户是否拥有perm权限
```
def has_perm(self, perm, obj=None):
    return True
```
### has_module_perms(app_label)
    用户是否拥有app中访问models的权限。
```
def has_module_perms(self, app_label):
    return True
```
    
    需要注册你自定义的用户模型到admin。如果你的自定义用户模型扩展于django.contrib.auth.models.AbscustomauthtractUser，你可以用django的 django.contrib.auth.admin.UserAdmin 类。如果你的用户模型扩展于 AbstractBaseUser，你需要自定义一个ModelAdmin类。他可能继承于默认的django.contrib.auth.admin.UserAdmin。然而，你也需要覆写一些django.contrib.auth.models.AbstractUser 字段的定义不在你自定义用户模型中的。
    
## 为User模型自定义一个管理器
    如果自定义的User中含有++username, email, is_staff, is_active, is_superuser, last_login, and date_joined++这些默认User中含有的字段，你就不需要自己定义一个管理器。除此之外，如果有其他自定义的字段，必须为User模型自定义一个管理器。它继承自BaseUserManager并提供两个额外的方法。
    
### create_user(username_field, password=None, other_fields)**
    接受username field和required字段来创建用户。
```
def create_user(self, username, nickname=None, password=None):
    if not username:
        raise ValueError("Users must have username")

    user = self.model(
        username=username,
        nickname=nickname,
    )

    user.set_password(password)
    user.save(using=self._db)
    return user
```
### create_superuser(username_field, password, other_fields)**
    接受username field和required字段来创建用户。password是必须的。
```
def create_superuser(self, username, nickname, password):
    user = self.create_user(
        username=username,
        password=password,
        nickname=nickname,
    )
    user.is_admin = True
    user.save(using=self._db)
    return user
```
## 自定义用户与内置身份验证表单
    Django内置的forms和views和相关联的user model有一些先决条件。如果你的user model没有遵循同样的条件，则需要定义一个替代的form，通过form成为身份验证views配置的一部分。

### UserCreationForm
    依赖于User Model. 扩展User时必须重写。

### UserChangeForm
    依赖于User Model. 扩展User时必须重写。

### AuthenticationForm
    Works with any subclass of AbstractBaseUser, and will adapt to use the field defined in USERNAME_FIELD.

### PasswordResetForm
    Assumes that the user model has a field named email that can be used to identify the user and a boolean field named is_active to prevent password resets for inactive users.

### SetPasswordForm
    Works with 任何AbstractBaseUser子类

### PasswordChangeForm
Works with 任何AbstractBaseUser子类

### AdminPasswordChangeForm
Works with 任何AbstractBaseUser子类

## 一个完整的例子
### models.py
```
from django.db import models
from django.contrib.auth.models import (
    BaseUserManager, AbstractBaseUser
)


class MyUserManager(BaseUserManager):
    def create_user(self, email, date_of_birth, password=None):
        """
        Creates and saves a User with the given email, date of
        birth and password.
        """
        if not email:
            raise ValueError('Users must have an email address')

        user = self.model(
            email=self.normalize_email(email),
            date_of_birth=date_of_birth,
        )

        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, date_of_birth, password):
        """
        Creates and saves a superuser with the given email, date of
        birth and password.
        """
        user = self.create_user(email,
            password=password,
            date_of_birth=date_of_birth
        )
        user.is_admin = True
        user.save(using=self._db)
        return user


class MyUser(AbstractBaseUser):
    email = models.EmailField(
        verbose_name='email address',
        max_length=255,
        unique=True,
    )
    date_of_birth = models.DateField()
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)

    objects = MyUserManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['date_of_birth']

    def get_full_name(self):
        # The user is identified by their email address
        return self.email

    def get_short_name(self):
        # The user is identified by their email address
        return self.email

    def __str__(self):              # __unicode__ on Python 2
        return self.email

    def has_perm(self, perm, obj=None):
        "Does the user have a specific permission?"
        # Simplest possible answer: Yes, always
        return True

    def has_module_perms(self, app_label):
        "Does the user have permissions to view the app `app_label`?"
        # Simplest possible answer: Yes, always
        return True

    @property
    def is_staff(self):
        "Is the user a member of staff?"
        # Simplest possible answer: All admins are staff
        return self.is_admin
```
### admin.py
```
from django import forms
from django.contrib import admin
from django.contrib.auth.models import Group
from django.contrib.auth.admin import UserAdmin
from django.contrib.auth.forms import ReadOnlyPasswordHashField

from customauth.models import MyUser


class UserCreationForm(forms.ModelForm):
    """A form for creating new users. Includes all the required
    fields, plus a repeated password."""
    password1 = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Password confirmation', widget=forms.PasswordInput)

    class Meta:
        model = MyUser
        fields = ('email', 'date_of_birth')

    def clean_password2(self):
        # Check that the two password entries match
        password1 = self.cleaned_data.get("password1")
        password2 = self.cleaned_data.get("password2")
        if password1 and password2 and password1 != password2:
            raise forms.ValidationError("Passwords don't match")
        return password2

    def save(self, commit=True):
        # Save the provided password in hashed format
        user = super(UserCreationForm, self).save(commit=False)
        user.set_password(self.cleaned_data["password1"])
        if commit:
            user.save()
        return user


class UserChangeForm(forms.ModelForm):
    """A form for updating users. Includes all the fields on
    the user, but replaces the password field with admin's
    password hash display field.
    """
    password = ReadOnlyPasswordHashField()

    class Meta:
        model = MyUser
        fields = ('email', 'password', 'date_of_birth', 'is_active', 'is_admin')

    def clean_password(self):
        # Regardless of what the user provides, return the initial value.
        # This is done here, rather than on the field, because the
        # field does not have access to the initial value
        return self.initial["password"]


class MyUserAdmin(UserAdmin):
    # The forms to add and change user instances
    form = UserChangeForm
    add_form = UserCreationForm

    # The fields to be used in displaying the User model.
    # These override the definitions on the base UserAdmin
    # that reference specific fields on auth.User.
    list_display = ('email', 'date_of_birth', 'is_admin')
    list_filter = ('is_admin',)
    fieldsets = (
        (None, {'fields': ('email', 'password')}),
        ('Personal info', {'fields': ('date_of_birth',)}),
        ('Permissions', {'fields': ('is_admin',)}),
    )
    # add_fieldsets is not a standard ModelAdmin attribute. UserAdmin
    # overrides get_fieldsets to use this attribute when creating a user.
    add_fieldsets = (
        (None, {
            'classes': ('wide',),
            'fields': ('email', 'date_of_birth', 'password1', 'password2')}
        ),
    )
    search_fields = ('email',)
    ordering = ('email',)
    filter_horizontal = ()

# Now register the new UserAdmin...
admin.site.register(MyUser, MyUserAdmin)
# ... and, since we're not using Django's built-in permissions,
# unregister the Group model from admin.
admin.site.unregister(Group)
```
### Last
    在settings里面设置自定义的模型指定为项目的默认用户模型
```
AUTH_USER_MODEL = 'users.Account'
```