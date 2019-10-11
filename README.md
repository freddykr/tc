# Документация по Tolstoy Comment Api
Эта документация расчитана на то, что вы уже прошли регистрацию на сайте [tolstoycomments.com](https://tolstoycomments.com/) и у вас уже доступ к личному кабинету. Так же для работы с документацией потребуется базовое знание основ JavaScript.
Основные вопросы на которые будут даны ответы в этой документации:
1. Как вставить виджет комментариев на сайт.
2. Как разместить счетчик комментариев под статьёй.
3. Как вывести счетчик комментариев на против каждой новости на главной и в разделе новостей странице.
4. Как вставить виджет с самыми обсуждаемыми статьями на сайте.
5. Как кстативить кнопку на сайт с вызовом виджета.

## Введение
Виджет Tolstoy Comment состоит из нескольких компонентов: 
- основной виджет с комментариями;
- счетчика кол-ва комментариев;
- дополнительных мини-виджетов.

## Инициализация
Основной скрипт инициализации загружает api для создания компонентов. При этом он не создает виджета с комментариями и других компонентов. Поэтому можно подключить основной код на все страницы вашего сайта. Место вставки кода особого значения не имеет, однако, если разместить основной скрипт внутри тега \<HEAD\> виджет будет загружаться быстрее.
```html
/// html
<!-- Tolstoy Comments Init -->
<script type="text/javascript">
	!(function(w, d, s, l, x) {
		w[l] = w[l] || [];
		w[l].t = w[l].t || new Date().getTime();
		var f = d.getElementsByTagName(s)[0],
			j = d.createElement(s);
		j.async = !0;
		j.src = "//test.tolstoycomments.com/sitejs/app.js?i=" + l + "&x=" + x + "&t=" + w[l].t;
		f.parentNode.insertBefore(j, f);
	})(window, document, "script", "tolstoycomments", "SITE_ID");
</script>
<!-- /Tolstoy Comments Init -->
```
`SITE_ID` - идентификатор вашего сайта

После инициализации основго кода, можно делать перечу различных параметров в виджет. Старый метод инициализации параметров строится на основе переменной `window.tolstoycomments.config`. Однако, такой подход не очень удобный, поскольку может привести к вставки кода подобного примеру ниже:
```html
/// html
<script type="text/javascript">
	window.tolstoycomments.config = {
		visible: true
	};
</script>
<!-- CUSTOM HTML -->
<script type="text/javascript">
	window.tolstoycomments.config = {
		desktop_class: 'tolstoycomments-feed'
	};
</script>
```
В этом случае одни настройки будут перезаписаны другими настройками и `visible: true` не будет задан. В новой версии передача параметров инициализации происходит вот так:
```html
/// html
<script type="text/javascript">
	window["tolstoycomments"] = window["tolstoycomments"] || [];
	window["tolstoycomments"].push({
		action: "init",
		values: {
			visible: true
		}
	});
</script>
<!-- CUSTOM HTML -->
<script type="text/javascript">
	window["tolstoycomments"] = window["tolstoycomments"] || [];
	window["tolstoycomments"].push({
		action: "init",
		values: {
			desktop_class: "tolstoycomments-feed"
		}
	});
</script>
```
Пример выше будет эквиволентен примеру:
```html
/// html
<script type="text/javascript">
	window["tolstoycomments"] = window["tolstoycomments"] || [];
	window["tolstoycomments"].push({
		action: "init",
		values: {
			visible: true,
			desktop_class: "tolstoycomments-feed"
		}
	});
</script>
```
И все это эквиволентно:
```html
/// html
<script type="text/javascript">
	window.tolstoycomments.config = {
		visible: true,
		desktop_class: 'tolstoycomments-feed'
	};
</script>
```
> Параметры переданные с помощью `action: "init"` всегда будут перезаписывать параметры переданные через `window.tolstoycomments.config`.

## Подключение виджета с комментариями
Чтобы вывести виджет с комментариями на странице сайта нужно разместить следующий код:
```html
/// html
<!-- Tolstoy Comments Widget -->
<div class="tolstoycomments-feed"></div>
<script type="text/javascript">
	window["tolstoycomments"] = window["tolstoycomments"] || [];
	window["tolstoycomments"].push({
		action: "init",
		values: {
			visible: true
		}
	});
</script>
<!-- /Tolstoy Comments Widget -->
```
По умолчанию основной код инициализации загружается с параметром `visible: false`. Чтобы виджет с комментариями отобрахился на странице нужно передать в настройках инициализации `visible: true`. Тег с классом `tolstoycomments-feed` задан в настройках по умолчанию. Название класса можно изменить, задав имя нужного класса в параметре `desktop_class`.
Пример:
```html
/// html
<script type="text/javascript">
	window["tolstoycomments"] = window["tolstoycomments"] || [];
	window["tolstoycomments"].push({
		action: "init",
		values: {
			desktop_class: 'CUSTOM_CLASS_NAME'
		}
	});
</script>
```

## Остались вопросы?
Если у вас есть вопросы, их можно задать, отправив по почте <help@tolstoycomments.com> или написать в виджете на странице лендинга [tolstoycomments.com](https://tolstoycomments.com/)

## Вставка кода на сайт
Образец кода для вставки на сайт можно взять после прохождения регистрации на сайте [tolstoycomments.com](https://tolstoycomments.com/)
## Основное Api
Для доступа к настройкам виджета:
```javascript
/// javascript
window.tolstoycomments.widget
```
### Открыть виджет
```javascript
/// javascript
window.tolstoycomments.widget.open();
```
### Закрыть виджет
```javascript
/// javascript
window.tolstoycomments.widget.close();
```
### Проверка состояние виджета
```javascript
/// javascript
window.tolstoycomments.widget.isopen(); // return type bool
```
### Инициализивать виджет
по умолчанию, при загрузке страницы, вызывается инициализация виджета
```javascript
/// javascript
window.tolstoycomments.widget.init();
```
### Удалить виджет
```javascript
/// javascript
window.tolstoycomments.widget.destroy();
```
### Открыть начальную страницу
Страница будет запущена с настройками переданными в 'window.tolstoycomments.config'. 
В случае, если страница чата совпадает с текущей открытой, обновление не произойдет.
```javascript
/// javascript
window.tolstoycomments.widget.navfirst();
```
### Перейти на страницу чата
В случае, если страница чата совпадает с текущей открытой, обновление не произойдет.
```javascript
/// javascript
window.tolstoycomments.widget.nav({
	url: url, // url: string - не обязательный, по умолчанию: document.location.href
	title: title, // title: string - не обязательный, по умолчанию: document.title
	identity: identity // identity: string - не обязательный, по умолчанию: MD5(document.location.href)
});
```
### Перейти на главную страницу чата
```javascript
/// javascript
window.tolstoycomments.widget.main();
```
### Перейти на страницу авторизации
```javascript
/// javascript
window.tolstoycomments.widget.auth();
```
## Инициализация
### Инициализация на странице всех чатов
По умолчанию код вызова виджета создает чат для текущей веб страницы.
Чтобы загружать виджет на странице всех чатов, нужно разместить следующий код сразу после вставки кода виджета:
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = { main: true };
</script>
```
### Инициализация с указанием произвольных данных страницы
identity: произвольная строка, которая служит идентификатором страницы сайта (это может быть порядковый номер статьи в базе или полная ссылка).

url: ссылка на страницу сайта, которая будет использована в виджете для перехода на текущую страницу сайта. По умолчанию берется значение 'document.location.href'.

title: заголовок страницы сайта. По умолчанию берется заголовок из document.title.
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        identity: 'Любая произвольная строка', 
        url: 'http://youdomen.com/youurl',
        title: 'Заголовок страницы'
    };
</script>
```
### Счетчик комментариев
Чтобы вывести счетчик комментариев, необходимо, чтобы на странице был подключен основной код виджета. В параметрах конфигурации виджета нужно задать имя класс селектора:
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        comment_class: 'tolstoycomments-cc'
    };
</script>
```
Для вывода счетчика комментариев нужно создать html элемент с именем класс, переданным в переменной `comment_class`. Пример:
```html
/// html
<span class="tolstoycomments-cc" data-url="[ПОЛНАЯ ССЫЛКА НА СТАТЬЮ]"></span>
или
<span class="tolstoycomments-cc" data-identity="[Любая произвольная строка]"></span>
```
Пример счетчика с текстом `Нет комментариев` по умолчанию:
```html
/// html
<span class="tolstoycomments-cc" data-url="[ПОЛНАЯ ССЫЛКА НА СТАТЬЮ]">Нет комментариев</span>
или
<span class="tolstoycomments-cc" data-identity="[Любая произвольная строка]"></span>
```
В параметре `data-url` нужно передать полную ссылку на статью, для которой необходимо сделать вывод количества комментариев.
(по умолчанию `comment_class` не задан, и виджет не будет выводить счетчики комментариев)
### Счетчик комментариев без вывода самого виджета
Для вывода счетчика комментариев на главной странице сайта, в списке рубрик или в списке статей можно отключить вывод виджета и оставить только загрузку счетчика комментариев. Для этого нужно задать `visible: false` в настройках конфигурации:
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        visible: false
    };
</script>
```
Так будет выглядеть инициализация виджета с выводом счетчика комментариев, но без вывода самого виджета
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        visible: false,
        comment_class: 'tolstoycomments-cc'
    };
</script>
```
### Счетчик комментариев в собственном стиле
Для создания собственного стиля вывода числа комментариев предусмотрена функция `comment_render`. Пример этой функции по умолчанию:
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        visible: false,
        comment_class: 'tolstoycomments-cc',
        comment_render: function (val) {
            return val.toString();
        }
    };
</script>
```
Функция `comment_render` вызывается после получения числа коментариев с сервера виджета. В переменной `val` передается число комментариев. В переменной `this` внутри функции будет передан html-элемент, в который происходит вывод. Функцию `comment_render` можно вызывать без return, в этом случае вы сами должны описать локигу вывода.
Пример собственного стиля вывода счетчика комментариев:
```html
/// html
/* ... основной код виджета */
<script>
    window.tolstoycomments.config = {
        visible: false,
        comment_class: 'tolstoycomments-cc',
        comment_render: function (val) {
            this.textContent = val + " комментари" + GetNumEnding(val, "й", "я", "ев");
        }
    }
    var GetNumEnding = function (n, a, b, c) {
        var v = parseInt(n % 100);
        if (v >= 11 && v <= 19) {
            return c;
        } else {
            v = parseInt(n % 10);
            if (v == 1) {
                return a;
            } else {
                if (v >= 2 && v <= 4) {
                    return b;
                } else {
                    return c;
                }
            }
        }
    }
</script>
```
Результат:
* 0 комментариев
* 1 комментарий
* 2 комментария
* 5 комментариев
## Комментарии под статьей (стандартный вид)
По умолчанию виджет выводится в том месте, куда был вставлен код. Если код вставки был видоизменен или отформатирован, или перенесен в отдельный файл, виджет вставится отдельным окном. Это происходит, поскольку становится невозможно найти место вставки кода виджета в DOM дереве страницы сайта.
Виджет можно вставить в произвольное место с помощью специального тега с классом:
```html
/// html
/* ... текст статьи */
<div class="tolstoycomments-feed"></div>
```
В параметрах инициализации виджета нужно добавить такое же имя класса, как и в коде выше:
```html
/// html
<script type="text/javascript">
    window.tolstoycomments.config = { 
        desktop_class: 'tolstoycomments-feed'
    };
</script>
```
*Примечание: виджет будет загружаться в указанное место только, если указать соответствующие настройки в панели администрирования в разделе дизайн*
## Полный список всех параметров инициализации виджета
Указан весь список параметров инициализации виджета с параметрами по умолчанию
```html
/// html
<script type="text/javascript">
    window.tolstoycomments.config = { 
	main: false, // если задать true то выведить список чатов
	identity: null, // identity текущей страницы
	url: NormolizeURL(document.location.href), // url текущий страницы, по умолчанию используется встроенная функция
	title: LoadTitle(window), // заголовок текущей страницы, по умолчанию используется встроенная функция LoadTitle
	visible: true, // загружать виджет комментариев
	comment_class: "", // класс элементов для вывода кол-ва комментариев в статьях
	comment_render: val => { // функция рендеринга кол-ва комментариев
		// this - текущий DOM элемент
		return val.toString(); 
	},
	comment_button_text: "Обсудить", // на мобильных устройствах при выборе дизайна с вызовом виджета по кнопке
	desktop_class: "tolstoycomments-feed", // класс элемента в который будет отрендерин виджет при встроенном в сайт дизайне
	success: () => {}, // callback функция для полчения доступа к методов виджета объекта tolstoycomments.widget
	scroll_border_top: 0, // отступ сверху при отображении виджета встроенным в статью
	scroll_border_bottom: 0, // отступ снизу при отображении виджета встроенным в статью
	sso: null,
	mobile: IsMobile() // текущая платформа на которой был загружен виджет, по умолчанию используется встроенная функция
    };
</script>
```
Тексты встроенных функций, используемые в инициализации виджета.
```javascript
/// javascript
function NormolizeURL(url) {
	let u = url,
		h = u.indexOf("#"),
		q = u.indexOf("?");
	if (h != -1) {
		u = u.substr(0, h);
	}
	if (q != -1) {
		u = u.substr(0, q);
	}
	return u;
}
function LoadTitle(win) {
	var titles = win.document.getElementsByTagName("title");
	if (titles.length > 0) {
		return trim(titles[0].innerText).substr(0, 255);
	} else {
		return "";
	}
}
function IsMobile() {
	return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(window.navigator.userAgent);
}
```
## Встроенные события виджета
```javascript
/// javascript
window.tolstoycomments.widget.on(event, callback); // подписываемся на событие
window.tolstoycomments.widget.off(event, callback); // отписываемся от события
```
Поддерживаемые типы событий:
1. `ready` - вызывается после загрузки фрейма и до его показа
1. `domready` - вызывается после первого показа фрейма
1. `open` - событие открытия виджета
1. `close` - событие закрытия виджета
## SSO авторизация
Для авторизации виджета нужно передать токен авторизации в параметре `sso`:
```html
/// html
<script type="text/javascript">
    window.tolstoycomments.config = { 
        sso: 'ТОКЕН ДЛЯ АВТОРИЗАЦИИ'
    };
</script>
```
Ниже представлены примеры формирования токена для авторизации с использованием различных языков программирования.
### Node.js
```node
/// Node.js
var arr = {
	id: "id0", // произвольная уникальная строка (50 знаков максимум) - обязательный параметр
	nick: "Иванов Иван", // имя пользователя (50 знаков максимум) - обязательный параметр
	email: "temp@temp.temp", // почта (250 знаков максимум) - необязательный параметр
	avatar: "https://static.tolstoycomments.com/ui/ac/b1/fa/acb1faad-2fad-441a-b789-da57f5317399.png" // ссылка на аватар - необязательный параметр
};
var key = "КЛЮЧ ДОСТУПА К API ИЗ НАСТРОЕК САЙТА";

var userdata = window.btoa(unescape(encodeURIComponent(JSON.stringify(arr))));
var microtime = Date.now();
var signtext = userdata + key + microtime.toString();
var sign = md5(signtext);
var sso = userdata + " " + sign + " " + microtime;
```
### PHP
```php
/// PHP
<?php
    $arr = array(
        'id' => 'id0',
        'nick' => 'Иванов Иван',
        'email' => 'temp@temp.temp',
        'avatar' => 'https://static.tolstoycomments.com/ui/ac/b1/fa/acb1faad-2fad-441a-b789-da57f5317399.png'
    );
    $key = "КЛЮЧ ДОСТУПА К API ИЗ НАСТРОЕК САЙТА";
    $userdata = base64_encode(json_encode($arr));
    $timestamp = round(microtime(true) * 1000);
    $sign = md5($userdata . $key . $timestamp);
    echo "$userdata $sign $timestamp";
?>
```
### Ruby
```ruby
content = Base64.strict_encode64(
  {
        id: 'id0',
      nick: 'Иванов Иван',
     email: 'temp@temp.temp',
    avatar: 'https://static.tolstoycomments.com/ui/ac/b1/fa/acb1faad-2fad-441a-b789-da57f5317399.png'
  }.to_json
)
timestamp = Time.now.to_i * 1_000
sign      = Digest::MD5.hexdigest("#{content}#{token}#{timestamp}")

[content, sign, timestamp].join(" ")
```
