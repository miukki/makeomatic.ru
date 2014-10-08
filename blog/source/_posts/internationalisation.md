title: Интернационализация в AngularJS
subtitle: Интернационализация с использованием angular-translate
date: 2014-25-07
author: Анна Аминева
gravatarMail: annafedotovaa@gmail.com
cover: http://makeomatic.ru/blog/images/dist.io.png
coverWidth: 623
coverHeight: 523
url: https://makeomatic.ru/blog/2014/07/25/About_Distributio/
tags: [AngularJS, Javascript]
---

Интернационализация охватывает много вопросов. В этой небольшой статье я расскажу о том, как начать работать с angular-translate. Его возможности, о которых мы с вами узнаем:
* предоставление нескольких языков с помошью json файлов (языковая таблица)
* асинхронная загрузка языков с помощью расширения angular-translate - StaticFilesLoader
* подключение запасного языка, если перевод слова на текущем не был найден.

Ссылку на весь код вы найдете в конце статьи.

###Настройка angular-translate

```js
angular.module("myapp", ['pascalprecht.translate'])
.config(function ($translateProvider) {
    //язык по умолчанию
    $translateProvider.preferredLanguage('en');
    //запасной язык, если запись не была найдена на текущем языке
    $translateProvider.fallbackLanguage('es');
    //загружаем записи языков из файлов
    $translateProvider.useStaticFilesLoader({
        prefix: '', //относительный путь, например: /languages/
        suffix: '.json' //расширение файлов
    });
})
```

###Использование angular-translate

Ниже вы можете увидеть использование директивы в качестве аттрибута и фильтра. В контроллере вы можете использовать сервис `$translate`.
```js
<h1 translate="title"></h1>
<h1>{{title | translate}}</h1>
```

###Выбор языка 

А тут приведен код выпадающего меню для выбора языка и контроллера:
```js
<!-- html код -->
<select ng-model="selectedLanguage" ng-change="changeLanguage()">
  <option value="en" translate="global_language_en"></option>
  <option value="es" translate="global_language_es"></option>
</select>

//контроллер
.controller("Controller", function($scope, $translate) {
    //переменная для хранения выбранного языка
    $scope.selectedLanguage = $translate.proposedLanguage(); //по умолчанию

    $scope.changeLanguage = function () {
      //значение, передаваемое в метод use, должно быть обозначением локали, например: en-UK, en и т.д.
      $translate.use($scope.selectedLanguage);
    };
});
```

По мотивам Gerard Sans “Angular internationalisation”

[Источники](http://angular-translate.github.io/docs/#/guide)


