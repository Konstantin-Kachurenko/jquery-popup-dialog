## Описание

Адаптивный всплывающий диалог, расширяющий плагин [$.popup](https://github.com/Konstantin-Kachurenko/jquery-popup). Добавляет к существующему API методы управления содержимым блока с автодетектом обертки `.popup-content` и поддержку html-триггеров `data-dialog="selector"` у кликабельных элементов для автоматической инициализации и открытия диалогов.

### Требования:

jQuery 1.7+, плагин [$.popup](https://github.com/Konstantin-Kachurenko/jquery-popup) 1.x.x

### Совместимость с браузерами:

Internet Explorer 9+, Opera 11.6+, Opera Mobile 11.5+, Safari 5+, Chrome, Firefox, IE Mobile

## Подключение

Для использования плагина подключите скрипт в любом месте страницы, после подключения плагина $.popup.

    <link rel="stylesheet" href="path/to/script/jquery.popup.min.css" >
    <script src="path/to/script/jquery.popup.min.js"></script>
    <script src="path/to/script/jquery.popup.dialog.min.js"></script>

## Примеры

### Базовый пример

Блоки с классом `.popup` по умолчанию невидимы, так что они могут быть расположены в любом месте документа. Плагин сам позаботится об их переносе в конец `<body>`, чтобы избежать проблем со свойствами `z-index`.

	<!-- Базовые стили, обязательные для подключения -->
	<link rel="stylesheet" href="path/to/script/jquery.popup.min.css" >
	
	<!-- Скрипты плагина -->
	<script src="path/to/script/jquery.popup.min.js"></script>
	<script src="path/to/script/jquery.popup.dialog.min.js"></script>
	
	<!-- 
	Темизация всплывающих окон осуществляется
	с помощью классов: .popup, .popup-content и .popup-overlay
	//-->
	<style type="text/css">
	.popup {
	  background-color: rgb( 248, 248, 248 );
	  box-shadow: 0px 2px 2px 0px rgba( 0, 0, 0, .3 );
	}
	.popup-content {
	  padding: 20px 40px 30px 40px;
	}
	.popup-overlay {
	  background-color: rgba( 0, 0, 0, .3 );
	}
	</style>
	
	<!-- 
	Атрибут "data-dialog" содержит CSS-селектор для доступа к диалогу 
	-->
	<button data-dialog="#popup-dialog">
	  Открыть диалог
	</button>
	
	<!--
	Эффекты появления устанавливаются блоку .popup с помощью классов: 
	.effect-fade-scale, .effect-slide-right, .effect-slide-left, 
	.effect-slide-top, .effect-newspaper и .effect-sticky
	Если класс эффекта не задан, анимация будет сведена до 'fade in'.
	
	Любой элемент с классом .popup-close внутри диалога
	будет вызывать его закрытие по клику.
	
	.popup-overlay создается автоматически, если он не найден 
	сразу перед блоком .popup. 
	
	.popup-content является опциональным и не создается автоматически, 
	однако он описан в таблице стилей и его наличие учитывается в работе
	методов .empty() и .put() 
	
	Основное назначение этого блока - создавать прокручиваемую область 
	внутри диалога, если он не поместился на экране польностью.
	//-->
	
	<div class="popup effect-fade-scale" id="popup-dialog">
	  <div class="popup-content">
	    <h3>Всплывающий диалог</h3>
	    <p>
	    Привет, я - плагин для реализации всплывающих диалогов 
	    с красивыми анимациями появления на CSS3, и с минимальным
	    количеством javascript и css кода.
	    </p>
	    <button class="popup-close">закрыть</button>
	  </div>
	</div>

### Использование API

При инициализации плагина, объект `$.dialog` сохраняется в `.data('popup')` у каждого из блоков, попавшего в исходный набор. Этот объект предоставляет управление копией плагина:

	<!-- Форма оформления подписки на новости -->
	<form id="form-subscribe">
	  <input type="email" name="subscribe" placeholder="Email" />
	  <button type="submit">Подписаться на новости</button>
	</form>
	
	<!-- 
	Диалог оповещения об отправке запроса. 
	-->
	<div class="popup effect-fade-scale" id="popup-dialog">
	  <div class="popup-content">
	    <h3>Запрос принят</h3>
	    <p>Спасибо за интерес, проявленный к нашей компании.</p>
	  </div>
	</div>
	
	<script>
	  $(document).ready(function(){
	
	    // Применяем плагин к блоку #popup-dialog
	    var $dialog = $('#popup-dialog').dialog();
	
	    // Включаем обработку отправки формы
	    $('#form-subscribe').submit(function(){
	
	      // Получаем доступ к плагину
	      var plugin = $dialog.data('popup');
	
	      // Отображаем диалог
	      plugin.open();
	
	      // Закрываем диалог автоматически через три секунды
	      plugin.close(3000);
	    });
	  });
	</script>

### Декларативная настройка при инициализации

Все доступные опции инициализации плагина могут быть описаны с помощью data-атрибутов диалога `.popup`:

	<!-- Динамическая загрузка содержимого диалога при показе -->
	
	<style type="text/css">
	#ajax-dialog:empty {
	  background: url(preloader.gif) 50% 50% no-repeat;
	}
	</style>
	
	<div class="popup effect-fade-scale" id="ajax-dialog"
	  data-modal=""
	  data-open="$(this).data('popup').empty().load('ajax.php')"
	></div>
	
	<button data-dialog="#ajax-dialog">
	  Открыть диалог
	</button>

## Параметры инициализации

`Boolean modal = false`

Модальный режим, в котором клик по подложке блока не закрывает его.

`Boolean bubble = true`

"Всплывание" блока вверх по дереву документа. При инициализации с этим параметром, установленным в true, блок будет перемещен в конец \<body\>, не зависимо от того, где он был изначально.

`Function open = Null`

Функция, вызываемая перед отображением всплывающего блока.

`Function close = Null`

Функция, вызываемая перед скрытием всплывающего блока.

`Function realign = Null`

Функция, вызываемая перед установкой свойств `margin-left` и `margin-top` при позиционировании блока.

## API

При инициализации плагина, объект $.dialog сохраняется в .data('popup') у каждого из блоков, попавшего в исходный набор. Этот объект предоставляет управление копией плагина и имеет следующий набор свойств методов:

### Свойства

`jQuery $popup`

Всплывающий блок

`jQuery $overlay`

Подложка всплывающего блока

### Методы

`$.popup config([Object params])`

Расширяет набор параметров инициализации, добавляя из объекта `Object params` новые параметры и перезаписывая уже существующие.

`$.popup open()`

Показывает всплывающий блок и его подложку.

`$.popup overlay()`

Показывает только подложку. После вызова этого метода, сам блок по прежнему может быть отображен с помощью метода `open()`

`$.popup close([Number delay])`

Скрывает всплывающий блок. Если числовой параметр `delay` передан и больше 0, закрытие будет отсрочено на переданное количество микросекунд.

`$.popup realign()`

Позиционирует блок так же, как при показе или изменении размеров окна. Полезен при динамическом наполнении блока.

`jQuery empty()`

Удаляет содердимое диалога, возвращая блок `.popup-content`, либо сам диалог `.popup`, если `.popup-content` не найден.

`jQuery put(mixed content)`

Помещает содержимое в блок `.popup-content`, либо в сам диалог `.popup`, если `.popup-content` не найден. Параметр content может быть как строкой, так и DOM-элементом или объектом jQuery, представляющим элемент.

`jQuery destroy()`

Уничтожает копию плагина, возвращая исходный блок.
