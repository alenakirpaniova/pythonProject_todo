***Проект - Мой список дел***

***Список дел — это список, который вы можете сохранить, чтобы поместить все задачи,
которые вам нужно выполнить в данный день.***

!http://127.0.0.1:8000/todoapp/

Если Вы раньше Django не использовали, то необходимо позаботиться о начальной настройке. А именно, 
необходимо автоматически сгенерировать определенный код, который устанавливает Django project — 
набор настроек для конкретного экземпляра Django, включающий в себя конфигурацию базы данных, 
специфичные для Django опции специфичные настройки для приложения.
1. Установите Django
Убедитесь, что в вашей системе установлен Django. Если у вас его нет, откройте командную строку (или cmd) и 
установите Django с помощью следующей команды: pip install django.
2. Откройте командную строку или командную строку. Если вы хотите создать проект в определенной папке,
перейдите в эту папку с помощью команды cd. 
3. Запустите проект, введя следующую команду: django-admin startproject todoproject.
Затем вы увидите, что создается новая папка под названием todoproject.
Теперь зайдите в папку todoproject, введя следующее в командной строке cd todoproject. 
4.Запустим сервер с помощью следующей команды python manage.py runserver.
URL-адрес будет сгенерирован в командной строке, что-то вроде 'http://127.0.0.1:8000/'.
Скопируйте этот URL-адрес и вставьте его на новую вкладку в браузере. Теперь вы увидите свой проект Django,
работающий в вашем веб-браузере.

Посмотрим на результат выполнения команды startproject:
в нем храняться файлы и каталоги необходимы для:
- todoproject - Внешний корневой каталог, это контейнер для вашего проекта. Его имя не имеет значения для Джанго; 
Вы можете переименовать его на что угодно.
- manage.py: утилита, позволяющая взаимодействовать с проектом различными способами.
Вы можете прочитать все подробности о manage.py в django-admin и manage.py.
- Внутренний каталог todoapp - это Python модуль вашего проекта. Его название вы будете использовать для импорта 
чего-либо из этого модуля.
- todoapp/__init__.py: пустой файл, который сообщает Python, что этот каталог должен рассматриваться как пакет Python’а.
Больше о пакетах можно прочитать в официальной документации Python.
- settings.py - Конфигурация и настройки проекта Django. В Настройки Django рассказано все о том, как работают настройки.
- urls.py - указание URL проекта на Django, можно сказать, что это «оглавление» вашего проекта.
Прочитайте больше информации о URL в Диспетчер URL.
- asgi.py: точка входа для ASGI-совместимых веб-серверов для обслуживания вашего проекта.
Смотрите Как развертывать с помощью ASGI для получения более подробной информации.
- wsgi.py: Точка входа для WSGI совместимых веб-серверов для работы с проектом.
Смотрите Как развертывать с помощью WSGI для уточнения деталей работы.

5Создадим приложение:
5.1. Откройте командную строку и перейдите в папку проекта с помощью команды 'cd'.
5.2. python manage.py startapp todoapp
5.3. Теперь откройте среду IDE. Перейдите в settings.py файл и укажите имя приложения в списке INSTALLED_APPS.
внесем туда наше приложение.
Приложение готово!
6. Создадим шаблон для нашего приложения списка дел.
Для этого введите следующую команду: mkdir templates.
Эта команда создаст новую папку с именем 'templates' внутри каталога проекта.
Откройте его в среде IDE и вручную создайте новый HTML-файл с именем 'todolist.html'.
Теперь давайте добавим заголовок для нашего приложения списка дел с помощью тега h1.
<h1>My To Do List</h1> .
Теперь перейдите в views.py файл и добавьте представление для шаблона.
from django.shortcuts import render
def todoappView(request):
    return render(request, 'todolist.html')
Далее нам нужно связать это представление с URL-адресом. Итак, зайдите в urls.py и внесите следующие изменения 
from django.contrib import admin
from django.urls import path
from todoapp.views import todoappView, addTodoView
from todoapp.views import todoappView, addTodoView, deleteTodoView
urlpatterns = [
    path('admin/', admin.site.urls),
    path('todoapp/', todoappView),
    path('addTodoItem/', addTodoView),
    path('deleteTodoItem/<int:i>/', deleteTodoView),
]
Теперь давайте перейдем к settings.py файлу и внесем изменения в список TEMPLATES. Найдите раздел DIRS и замените 
эту строку следующим кодом:'DIRS': [os.path.join(BASE_DIR, 'templates')]. Если os выдает ошибку, то нужно просто
импортировать os.path
Теперь запустите сервер и перейдите к URL/todoapp.
Вы увидите страницу с заголовком «My To Do App».
7. Создание модели. Мы создадим модель, представляющую каждый элемент списка дел, который мы создадим и сохраним
в базе данных.перейдите в models.py и добавьте следующий код.
from django.db import models
class TodoListItem(models.Model):
    content = models.TextField() 
Мы будем использовать эту модель для взаимодействия с базой данных.
Теперь нам нужно сделать миграцию. Мы выполняем миграцию в Django, когда нам нужно изменить конфигурацию базы данных.
Откройте командную строку и введите следующую команду:
python manage.py makemigrations.
После выполнения этой команды введите также следующую команду: 
python manage.py migrate
8. Создание списка дел.
8.1. Попробуем извлечь элементы todo в представление. Откройте views.py файл и импортируйте модель.
from .models import TodoListItem 
Мы создадим новую переменную для хранения всех элементов todo. Затем мы вернем этот список в файл шаблона.
Здесь мы возвращаем список «all_todo_items» как «all_items» через словарь.
8.2. Теперь давайте перейдем к файлу шаблонов и создадим список, чтобы показать все элементы todo.
Откройте todolist.html и добавьте следующий код.
<ul>
    {% for i in all_items %} 
    <li>{{i.content}}
    </li>
    {% endfor %}
</ul> 
Здесь мы используем цикл for для итерации по списку элементов, которые у нас есть в переменной all_items.
Теперь, чуть ниже заголовка, давайте создадим форум, чтобы пользователи могли добавлять элементы todo в список.
У нас есть два входных тега на форуме. Текстовое поле, в которое пользователь может ввести элемент, 
и кнопка для отправки элемента.
8.3. Теперь давайте перейдем к views.py, чтобы создать представление для обработки этого запроса записи.
Код, который мы напишем нужно нам нужно импортировать его в верхней части этого файла, прежде чем мы действительно 
его используем. from django.http import HttpResponseRedirect 
8.4. Теперь перейдите к urls.py и создайте новый путь для представления, которое мы только что создали.
Кроме того, не забудьте импортировать addTodoView в файл urls.py. Итак, внесите следующее изменение:
from todoapp.views import todoappView, addTodoView 
from django.shortcuts import render
def todoappView(request):
    return render(request, 'todolist.html')
from .models import TodoListItem
def todoappView(request):
    all_todo_items = TodoListItem.objects.all()
    return render(request, 'todolist.html',
    {'all_items':all_todo_items})
from django.http import HttpResponseRedirect
def addTodoView(request):
    x = request.POST['content']
    new_item = TodoListItem(content = x)
    new_item.save()
    return HttpResponseRedirect('/todoapp/')
Теперь обновите страницу в браузере. Вы сможете добавлять новые элементы в список.
9. Добавим кнопку удаления. 
9.1. Перейдем к файлу todolist.html и создадим кнопки удаления для каждого элемента списка дел.
Добавьте следующую кнопку между тегами <li> сразу после содержимого.
<form action="/deleteTodoItem/{{i.id}}/" method = "post">{% csrf_token %}
            <input type="submit" value="Delete">
</form>  
9.2. Теперь давайте перейдем к urls.py и добавим новый путь для запросов на удаление.
Кроме того, не забудьте импортировать deleteTodoView в этот файл.
Теперь давайте перейдем к views.py и создадим новую функцию просмотра для запросов на удаление.
9.3. Создадим в папке views.py новую функцию просмотра для запросов на удаление.
def deleteTodoView(request, i):
    y = TodoListItem.objects.get(id= i)
    y.delete()
    return HttpResponseRedirect('/todoapp/').
Наше базовое приложение списка дел готово. Запусти сервер с помощью python manage.py runserver 
и увидем  http://127.0.0.1:8000. Заапустим сервер и перейдем к URL/todoapp.
Теперь мы можем добавлять наши задачи. При выполнение поставленных задач
можем также их удалять из списка.
***Краткая инструкция по работе с сайтом***
1. Запустить программу.
2. Добавить задачу в окно и нажать кнопку add (добавить).
3. Удалить с помощью кнопки del (удалить).
Если вы столкнулись с какими-либо ошибками, попробуйте погуглить ошибку или найти ответы StackOverflow.
Также Вы можете обратиться за помощью на сайт https://pythonistaplanet.com/to-do-list-app-using-django/
и https://github.com/alenakirpaniova?tab=repositories.
