<h3>Общие правила форматирования</h3>

* Используем кодировку UTF-8. <meta charset="utf-8">
* В качестве отступа используем четыре пробела
* Lowercase для HTML элементов (тэгов), атрибутов и их значений, CSS селекторов, свойств и их значений.
* Не оставляем пробелов в конце строки
* Помечаем недоделки TODO: описание. Можно указать исполнителя, дописав логин в скобках TODO(aloginova): go home.
* Для перехода на новую строку используем CRLF (не LF). В vs можно настроить справа внизу.

<h3>Правила для HTML</h3>

* По возможности используем семантическую разметку - то есть используем элементы по их назначению: button для кнопки, p для параграфа, h* - для заголовков.
* Для осмысленных изображений добавляем описание в alt. Для декоративных - не нужно.
* Не используем инлайн-стилей, стилизующих тегов типа 'u', 'strong' и т.п. В HTML реализуем только структуру, для стилей - CSS.
* Двойные кавычки
* Нет пробелов перед и после "="
* Блочные элементы, списки и таблицы сносим на новую строку. Добавляем отступы для дочерних элементов. Инлайновые элементы можно писать в той же строке, если позволяет длина.
* Закрываем блочные элементы, списки, таблицы на новой строке том же уровне вложенности (имеется ввиду количество отступов), на котором открываем, либо на той же строке - при небольшой длине.

Надо:

```html
<div>
  <div>
    <p>Blabla <span>ololo</span></p>
  </div>
</div>
```

Не надо:

```html
<div>
  <div>
    <p>Blabla</p></div> 
    </div>
```

* Не закрываем (escape) void элементы ('< br >', а не '< br />', '< input >', '< img >', '< meta >', '< base >', '< link >')

<h3>Правила для CSS</h3>

* Форматирование: классы отделяются пустой строкой, пробел перед "{", после ":", новая строка после "{", ";"

Надо:

```css
.custom-class {
    color: #f456be;
    border: 1px solid #123455;
}
```

Не надо:

```css
.custom-class{
    color:#f7678f;
    border:1px solid #000123
}
.second-class
{ 
    color: #098123; width: 100%;
}
```


* Одинарные кавычки
* БЭМ - наименование классов
* Не используем вложенность, пока это возможно. Допустима вложенность при задании стилей для дочерних элементов по ховеру на родителе, по добавлению бэм-модификатора, для медиа-кваери.
* Не используем Id для задания стилей.
* Не используем селекторы тегов


<h3>Порядок свойств</h3>
Упорядочить свойства по смыслу:
* Сперва наиболее важные и общие свойства: display, position, width, height
* Затем позиционирование: z-index, top, left, padding, margin
* Все остальные сгруппированные свойства:

Надо:

```css
.block {
  display: block
  width: 100%
  color: red
  font-size: 60px
  line-height: 1.2
  background-image: url('')
  background-position: 50% 50%
  transition: color ease 0.5s
}
```

Не надо:

```css
.block {
  transition: color ease 0.5s
  color: red
  line-height: 1.2
  width: 100%
  background-image: url('')
  display: block
  font-size: 60px
  background-position: 50% 50%
}
```

<h3>Препроцессоры</h3>
Во многих препроцессорах есть возможность записывать названия классов при помощи наследования от родительского класса, используя знак &. 
Использование такой визуальной вложенности снижает читаемость и делает невозможным поиск по селектору (’.house__door’ в поиске не будет найден). Поэтому:

* Исключаем вложенность через знак & для случаев внутренних элементов и модификаторов:

Не надо:

```css
.house
  height: 100%
  &_big
    height: 300%
  &__window
    color: transparent
    &_close
      color: brown
  &__door
    background: url('/assets/window.jpg')
```

Надо:

```css
.house
  height: 100%

.house_big
  height: 300%

.house__window
  color: transparent

.house__window_close
  color: brown

.house__door
  background: url('/assets/door.jpg')
```


* Исключениями являются псевдоэлементы и псевдоклассы - при условии, что вложенность одноуровневая (изменяются свойства только этого элемента): 

Не надо:

```css
.house
  color: brown
  &:hover
    .house__window  // другой класс
      color: transparent
```

Надо:

```css
.house
  color: brown
  &:hover         // тот же класс
    color: red

.house:hover .house__window
  color: transparent

.door
  height: 100%
  &::after        // не теряется контекст псевдоэлемента
    background: url('/assets/budka-sobaki.jpg')
```

<h3>Stylus</h3>
Для препроцессора Stylus используем следующий стиль:

* Не используем фигурные скобочки
* Не ставим точку с запятой в конце свойства;

Не надо:

```css
.house {
  display: block;
  width: 100px;
}
.window {
  width: 20px;
  background: url('/assets/window.jpg');
}
```

Надо:

```css
.house
  display: block
  width: 100px

.window
  width: 20px
  background: url('/assets/window.jpg')
```


<h3>И еще раз о БЭМ</h3>

БЭМ = Блок Элемент Модификатор.

Блок - функционально независимый элемент, который может быть повторно использован.
Название блока отвечает на вопрос "что это?", но не "какой?".
Блок НЕ должен влиять на свое окружение, он не задает собственное позиционирование и внешние отступы.
То есть у блока нет таких свойств, как position, top, left, bottom, right, margin, float.
Так же по возможности стоит избегать задания width - задавать только там, где это действительно необходимо по смыслу. Например, если делаем блок для логотипа, то фиксируем ему размеры, но если делаем блок для вывода новости с заголовком, то width предполагаем равной 100%.
Допустима любая вложенность блоков в HTML-разметке.

Элемент - составная часть блока, которая не может быть использована в отрыве от него.
Название так же отвечает на вопрос "что?". Может задавать собственное позиционирование (и активно этим занимается).
В HTML возможна любая вложенность элементов, блоки также можно вкладывать в элементы. 
У элемента всегда есть блок-родитель. Но не у каждого блока должны быть элементы.

Не надо:

```html
<div class="some-block">
  <div class="oh-come-on__elem-one"></div> // нет блока oh-come-on - значит не должно быть и его элементов
  <div class="oh-come-on__elem-two"></div>
</div>
<div class="some-block__i-am-lost"></div> // элементы блока должны быть вложены в него в дом-дереве
```

Модификатор - определяет внешний вид, состояние, поведение.
Модификаторы бывают булевые и ключ-значение. Например булевый \_disabled, ключ-значение \_color\_red. 
Модификатор всегда используется вместе с блоком или элементом.

Не надо:

```html
<div class="my-block_disabled">
```

Надо:

```html
<div class="my-block my-block_disabled">
```  

Для позиционирования блока внутри другого блока (т.к. блоку задавать позиционирование нельзя) 
можно использовать миксы - сочетание разных бэм-сущностей на одном дом-узле, а проще говоря, блок и элемент одновременно, например:

```html
<div class="vacuum">
	<div class="horse vacuum__dweller">
		<div class="horse__tail">  и т.д.
```	

<h3>Правила именования.</h3>

- Слова в названии разделяются дефисами: my-sphere-horse
- Элемент отделяется от блока двумя нижними подчеркиваниями: my-sphere-horse\_\_eye, my-sphere-horse\_\_tail
- Модификатор отделяется от блока/элемента одним нижним подчеркиванием: my-sphere-horse\_vacuumable
- Если у модификатора есть значение, то так же одним нижним: my-sphere-horse\_\_eye\_side\_right, my-sphere-horse\_\_eye\_side\_left

<h3>Пояснения и примеры</h3>

Вложенность бэм-сущностей не копирует вложенность дом-элементов.
Но элементы блока всегда вложены в него в дом дереве (вложенность может быть любого порядка).

Пример.
Допустим имеем такую структуру:

```html
<div>
  <h1>Breaking news</h1>
  <div>
    <img src="jdun.png" alt="Ждущий ждун">
    <p>Homunculus loxodontus is waiting for Alexander.</p>
    <p>And so the Slowpoke does.</p>
    <p>But does anybody wait for Dmitry?</p>
  </div>
</div>
```

Можно для нее создать такую бэм-структуру:

```html
<div class="news">
  <h1 class="news__title">Breaking news</h1>
  <div class="news__article">
    <img src="jdun.png" alt="Ждущий ждун" class="news__illustration">
    <p class="news__topic news__topic_primary">Homunculus loxodontus is waiting for Alexander.</p>
    <p class="news__topic">And so the Slowpoke does.</p>
    <p class="news__topic">But does anybody wait for Dmitry?</p>
  </div>
</div>
```

Так же можно сделать так: выделить article в отдельный блок (в случае, если предполагается, что эта часть может быть переиспользована, либо просто если так будет логичнее и удобнее организовать структуру) и использовать микс (блок и элемент на одной сущности - чтобы спозиционировать article внутри news).

```html
<div class="news">
  <h1 class="news__title">Breaking news</h1>
  <div class="news__article article">
    <img src="jdun.png" alt="Ждущий ждун" class="article__illustration">
    <p class="article__topic article__topic_primary">Homunculus loxodontus is waiting for Alexander.</p>
    <p class="article__topic">And so the Slowpoke does.</p>
    <p class="article__topic">But does anybody wait for Dmitry?</p>
  </div>
</div>
```

А вот так делать НЕ НАДО: создавать элементы элементов, например так:

```html
<div class="news">
  <h1 class="news__title">We don't do it like this!</h1>
  <div class="news__article">
    <img src="jdun.png" alt="Ждущий ждун" class="news__article__illustration">
    <p class="news__article__topic news__article__topic_primary">Homunculus loxodontus is waiting for Alexander.</p>
    <p class="news__article__topic">And so the Slowpoke does.</p>
    <p class="news__article__topic">But does anybody wait for Dmitry?</p>
  </div>
</div>
```

Также НЕ НАДО создавать блоков без необходимости.

