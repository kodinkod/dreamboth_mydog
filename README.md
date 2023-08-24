# dreamboth_name

 - [Описание статьи](## что это такое ?) 
    - что это такое ?(## что это такое ?)
    - Почему над этим работают?(## Почему над этим работают?)
    - [Как формулируется задача?]([## Как формулируется задача?](https://github.com/kodinkod/dreamboth_name/blob/main/README.md#как-формулируется-задача))
    - В чем ее основная идея?(## В чем ее основная идея?)
    - В чем ее новаторство?(## В чем ее новаторство?)
    - Какие получились результаты?(## Какие получились результаты?)
 - Guide
   - quickstart ()
   - train
   - interference 

## что это такое ? 
 В [DreamBoth(paper)](https://arxiv.org/pdf/2208.12242.pdf "link")  предлагается метод дообучения дифузионных tex2img моделей на маленьком датасете (до 30 изображений одного класса, однотипного объекта) путем внедрения уникальных токенов, которые привязываются к классу. Это позволяет генерировать изображения с объектом из обучающей выборки в разных сценах, сохраняя важные детали объекта. Так, вы можете обучить дифузионную модель на изображениях своей любимой собачки и генерировать картинки, где ваша собака гуляет по парижу и пьёт капучино! 

 ## Почему над этим работают?
   Большие модели преобразования текста в изображение совершили значительный скачок в эволюции искусственного интеллекта, обеспечив высококачественный и разнообразный синтез изображений из заданной текстовой подсказки. Однако этим моделям не хватает способности имитировать внешний вид объектов в заданном наборе ссылок и синтезировать их новые представления в различных контекстах.
   
   Метод DreamBoth позволяет справляется с <b>language drift (языковой дрейф)</b> - это явление, при котором язык или его использование со временем меняется или сдвигается в своем значении, смысле или терминологии. Из-за чего даже тонко настроенные, когда-то хорошо работающие  tex2img  модели начинают работать некорректно. Чтобы решитью эту проблему в статье используют class-specific prior preservation-loss, который поощеряет разнообразие и противодествует языковому дрейфу.
   
   Также этот метод  решает задачу генерации объекта в различных сценах и позах, стилях не теряя при этом ключяевых особенностей объекта; Важно, что метод позволит инетгрировать объект в сцены, которых даже не было в обучающей выборке. Решение этой задачи может пригодится для генерации карточек товаров на онлайн-маркетах!
  
## Как формулируется задача?

## В чем ее основная идея?
## В чем ее новаторство?
## Какие получились результаты?

## quickstart

## Результаты обучения 


## train

## interference 

## paper 
