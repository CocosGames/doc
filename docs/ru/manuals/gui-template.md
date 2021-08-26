---
title: GUI-нода Templates
brief: This manual explains the Defold GUI template system that is used to create reusable visual GUI components based on shared templates or 'prefabs'.
---

# GUI-нода Templates

GUI-нода Template предоставляет мощный механизм для создания многократно используемых компонентов GUI на основе общих шаблонов или "префабов". В этом руководстве объясняется, что это за функция и как ее использовать.

GUI-шаблон --- это GUI-сцена, которая инстанцируется, нода за нодой, в другую GUI-сцену. Любые значения свойств нод исходного шаблона могут быть переопределены.

## Создание шаблона

GUI-шаблон --- это обычная GUI-сцена, поэтому он создается так же, как и любая другая GUI-сцена. <kbd>Кликните ПКМ</kbd> в каком-либо расположении в панели *Assets* и выберите <kbd>New... ▸ Gui</kbd>.

![Create template](images/gui-templates/create.png){srcset="images/gui-templates/create@2x.png 2x"}

Создайте шаблон и сохраните его. Следует заметить, что ноды экземпляра будут располагаться относительно начала координат, поэтому целесообразно создать шаблон в позиции 0, 0, 0.

## Создание экземпляров из шаблона

На основе образца можно создать любое количество экземпляров. Создайте или откройте GUI-сцену, в которой нужно разместить шаблон, затем <kbd>кликните ПКМ</kbd> в секции *Nodes* в *Outline* и выберите <kbd>Add ▸ Template</kbd>.

![Create instance](images/gui-templates/create_instance.png){srcset="images/gui-templates/create_instance@2x.png 2x"}

Задайте свойству *Template* файл шаблона GUI-сцены.

Можно добавить любое количество шаблонных экземпляров, и для каждого экземпляра можно переопределить свойства каждой ноды и изменить позицию, цвет, размер, текстуру и так далее.

![Instances](images/gui-templates/instances.png){srcset="images/gui-templates/instances@2x.png 2x"}

Любое свойство, которое вы измените, будет отмечено синим цветом в редакторе. Нажмите кнопку сброса рядом со свойством, чтобы установить его значение в значение из шаблона:

![Properties](images/gui-templates/properties.png){srcset="images/gui-templates/properties@2x.png 2x"}

Любая нода, имеющая переопределенные свойства, также окрашивается в синий цвет в *Outline*:

![Outline](images/gui-templates/outline.png){srcset="images/gui-templates/outline@2x.png 2x"}

Экземпляр шаблона отображается в виде сворачиваемой записи в представлении *Outline*. Однако важно отметить, что этот элемент в Outline *не является нодой*. Экземпляр шаблона также не существует в рантайме, но все ноды, которые являются частью этого экземпляра, существуют.

Ноды, являющиеся частью экземпляра шаблона, автоматически именуются с префиксом и слэшем (`"/"`), прикрепленным к их *Id*. Префикс --- это *Id*, установленный в экземпляре шаблона.

::: important
Переопределение значений нод экземпляра шаблона в *Layouts* в настоящее время не работает в Editor 2. Если необходимо использовать макеты в сочетании с шаблонами, пожалуйста, используйте Editor 1, пока эта проблема не будет решена.

См. https://github.com/defold/editor2-issues/issues/1124
:::

## Изменение шаблонов в рантайме

Скрипты, которые манипулируют или запрашивают ноды, добавленные через механизм шаблонов, должны учитывать только именование нод экземпляра и включать *Id* экземпляра шаблона в качестве префикса имени ноды:

```lua
if gui.pick_node(gui.get_node("button_1/button"), x, y) then
    -- Какие-либо действия...
end
```

Не существует ноды, соответствующей самому экземпляру шаблона. Если требуется корневая нода для экземпляра, добавьте ее в шаблон.

Если скрипт связан со GUI-сценой шаблона, он не является частью дерева нод экземпляра. Можно прикрепить один единственный скрипт к каждой GUI-сцене, чтобы логика скрипта располагалась в GUI-сцене, в которую инстанцируются шаблоны.