### Реализуйте возможность указания ссылок на страницы книги в различных интернет-магазинах.

В файле models.py была создана модель Link. Поле ``` book_a ``` показывает, что связь
между моделями ``` Link ``` и ``` Book ``` один ко многим (одной книге может
соответствовать несколько ссылок). Поле ``` link ``` будет хранить валидный
URL-адрес. С помощью метода ``` str ``` будет производиться отображение
поля ``` link ``` на сайте администратора. Класс ``` Meta ``` задает дополнительную
информацию о модели(имя для модели в единственной и множественной форме):

```
class Link(models.Model):
    book_a = models.ForeignKey('Book', on_delete=models.CASCADE, null=True)
    link = models.URLField('Ссылка', max_length=255)

    def __str__(self):
        return self.link

    class Meta:
        ordering = ('link',)
        verbose_name = 'Ссылка на книгу в других магазинах'
        verbose_name_plural = 'Ссылки на книгу в других магазинах'
```

Далее для отображения полей для ввода ссылок в линию был добавлен класс LinkInLine:

```
class LinkInline(admin.TabularInline):
    model = Link
```
Регистрация модели в панели администратора. Поле ``` list_display = ('link',) ```
показывает, какое поле модели ``` Link ``` будет отображаться на сайте администратора:
```
@admin.register(Link)
class LinkAdmin(admin.ModelAdmin):
    list_display = ('link',)
```

Для отображение ссылок на сайте был добавлен следуюущий цикл в файл book_detail.html:
```
{% for l in book.link_set.all %}
  <ul>
  <a href="{{l.link}}">Книга в другом магазине</a>
  </ul>
  {% endfor %}
```
