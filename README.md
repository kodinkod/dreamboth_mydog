# dreamboth mydog genereted
<p align="center">
<img src="image/exp1-vangoghe.png"  width="300" /> 
</p>

[run in colab](https://colab.research.google.com/drive/1KAM4otIc21EJNqKrRCr-Bvg_akgvbKs-?usp=share_link)

 - [Описание статьи](#что-это-такое) 
    - [что это такое ?](#что-это-такое-)
    - [Почему над этим работают?](#почему-над-этим-работают)
    - [Как формулируется задача?](#как-формулируется-задача)
    - [В чем ее основная идея?](#в-чем-её-основная-идея)
    - [В чем ее новаторство?](#в-чем-ее-новаторство)
    - [Какие получились результаты?](#какие-получились-результаты)
 - [Обучение на картинках моей собаки](#обучение-на-картинках-моей-собаки)
    - [эксперименты 1-4](#эксперимент-1)
    - [эксперимент 5](#эксперимент-5)
    - [больше примеров](#other)
 - [Guide](#quickstart)
   - [quickstart](#quickstart)
 - [ВЫВОДЫ](#выводы)
 - [веса](#веса)

## что это такое ?
 В [DreamBoth(paper)](https://arxiv.org/pdf/2208.12242.pdf "link")  предлагается метод дообучения дифузионных tex2img моделей на маленьком датасете (до 30 изображений одного класса, однотипного объекта) путем внедрения уникальных токенов, которые привязываются к классу. Это позволяет генерировать изображения с объектом из обучающей выборки в разных сценах, сохраняя важные детали объекта. Так, вы можете обучить дифузионную модель на изображениях своей любимой собачки и генерировать картинки, где ваша собака гуляет по парижу и пьёт капучино! 

 ## Почему над этим работают?
   Большие модели преобразования текста в изображение совершили значительный скачок в эволюции искусственного интеллекта, обеспечив высококачественный и разнообразный синтез изображений из заданной текстовой подсказки. Однако этим моделям не хватает способности имитировать внешний вид объектов в заданном наборе ссылок и синтезировать их новые представления в различных контекстах.
   
   Метод DreamBoth позволяет справляется с <b>language drift (языковой дрейф)</b> - это явление, при котором язык или его использование со временем меняется или сдвигается в своем значении, смысле или терминологии. Из-за чего даже тонко настроенные, когда-то хорошо работающие  tex2img  модели начинают работать некорректно. Чтобы решитью эту проблему в статье используют class-specific prior preservation-loss, который поощеряет разнообразие и противодествует языковому дрейфу.
   
   Также этот метод  решает задачу генерации объекта в различных сценах и позах, стилях не теряя при этом ключяевых особенностей объекта; Важно, что метод позволит инетгрировать объект в сцены, которых даже не было в обучающей выборке. Решение этой задачи может пригодится для генерации карточек товаров на онлайн-маркетах!
  
## Как формулируется задача?
Главная задача состоит в том, чтобы внедрить экземпляр объекта в область вывода модели таким образом, чтобы мы могли запрашивать у модели различные новые изображения объекта. Попутно с этим в результате решаются задачи переноса стиля, переноса изображения на новый фон, смешивание классов в одном объекте (можно сгенерироывть картинку вашей собаки в классе бегемота и получится бегемот с лицом собачки:) А также может решаться задача изменения качеств объекта - допустим замены цвета, материала из которого сделан объект в обучающей выборке. 

## В чем ее основная идея?
Основная идея метода это внедрить в описание объектов обучающей выборки(до 30 изображений) уникальный "редкий_токен", который будет обозначать объект на котором мы обучаемся. Редкий токен в описании связвается с названием класса, (например описание изображения из тренировочной выборки может выглядеть так: <Фото "редкий_токен" собаки>); Это позволяет использовать уже имеющиеся знания модели о классах и научить её новому объекту под названием "редкий_токен" связанным с каким либо классом. Именно знания о классе с которым связан "редкий_токен" позволит генерировать наш объект из обучающей выборки сохраняя его ключевые детали и изменяя сцены и позы в которых находится объект. 

Но не только внедрение "редкого_токена" помогает добиться хороших результатов. class-specific prior preservation-loss, который позволяет бороться с language drift и переобучением.

<p align="center">
<img src="image/class-specific-prior-preservation-loss.png"  width="500" /> 
</p>

_**Замечание**: В статье не рекомендуют использовать в качастве редких токенов случайный набор букв, ведь это может привести к тому, что языковая модель разделит ваш уникальный токен(слово) на буквы(токены) и мы во время обучения будем связывать класс с несколькими буквами, а это не приводит к хорошим результатам. Рекомендуется использовать короткие, не случайные наборы букв._

## В чем ее новаторство?
В статье описывают технику для решения нескольких ранее невыполнимых задач, включая реконтекстуализацию объекта, синтез представлений с ориентацией на текст и художественную визуализацию, сохраняя при этом ключевые особенности объекта. Также предоставлян новый набор данных и протокол оценки для этой новой задачи предметно-ориентированного генерирования.

## Какие получились результаты?
В целом метод представленный в статье позволял генерировать объекты в разных с сценах и некоторые генерации были почти неотличимы от реальных фотографий, однако иногда могут возниковать проблемы. 
<p align="center">
<img src="image/figure7.png" width="500" /> 
</p>
<b>А вот какие проблемы могут быть</b>: переобучение(с), синтезация некоректоного контекста(a), Контекст и внешний вид предмета могут перепутаться(b).
<p align="center">
<img src="image/figure9.png" width="500" />
</p>

## Обучение на картинках моей собаки

<p align="center">
<img src="dog/photo_2023-08-26 21.42.26.jpeg" style='float:left;' width="190" /> 
<img src="dog/photo_2023-08-26 21.50.56.jpeg" style='float:left;' width="190" /> 
<img src="dog/photo_2023-08-26 21.56.13 2.jpeg" style='float:left;' width="190" /> 
<img src="dog/photo_2023-08-26 21.42.14.jpeg" style='float:left;' width="190" />
</p>
<b>Постановка эксперимента: </b>

 - 18 фотографий в обучающей выборке
 - фиксируем train_step = 1300
 - фиксируем batch_size  = 1
 - фиксируем learning rate = 3e-4, 1e-4
 - train random seed = 42
 - LORA_SCALE_UNET = 0.4
 - LORA_SCALE_TEXT_ENCODER = 0.4
 - GUIDANCE = 6.4

Замечено, что выбор промта и редкого токена часто оказывают сильнейшее влияние на генерацию. Чтобы подобрать редкий токен было выбрано несколько идей из опыта интернет-комьюнити и все они протестированы (sks, http, hta, oue). Также эксперимент будет интересен тем, что я буду обучать используя простые фотографии своей собаки с телефона!

Сначала я обучал модель, потом делал 4 генерации "A {rare-token} dog" с разным random seed, а затем генерирвал 4 картинки тестовые для проверки возможностей c различным промтом:
 - смена одежды объекта: A {RARE_TOKEN} {CLASS} in red t-shorts.
 - смена стиля: A {RARE_TOKEN} {CLASS} in the style of van gogh.
 - сменя локации: A {RARE_TOKEN} {CLASS} in the forest.
 - смена действия: A {RARE_TOKEN} {CLASS}  driving car.

### Эксперимент 1
   - random seed in generation = 6958252186108282
   - prompt "A sks dog"
   - без train text-encoder

<p align="center">
<img src="image/exp_1.png"  width="500" /> 
</p>

### Эксперимент 1_1
   - random seed in generation = 342405815741279
   - prompt "A sks dog"
   - train text-encoder

<p align="center">
<img src="image/exp_1_1.png"  width="500" /> 
</p>

### Эксперимент 2
   - random seed in generation = 342405815741279
   - prompt "A http dog"
   - без train text-encoder

<p align="center">
<img src="image/exp_2_ 342405815741279.png"  width="500" /> 
</p>

### Эксперимент 2_2
   - random seed in generation = 8
   - prompt "A http dog"
   - train text-encoder

<p align="center">
<img src="image/exp2_1_8.png"  width="500" /> 
</p>

### Эксперимент 3
   - random seed in generation = 44220
   - prompt "A hta dog"
   - без train text-encoder

<p align="center">
<img src="image/exp_3_44220.png"  width="500" /> 
</p>

### Эксперимент 3_2
   - random seed in generation = 24277
   - prompt "A hta dog"
   - train text-encoder

<p align="center">
<img src="image/exp_3_1_24277.png"  width="500" /> 
</p>

### Эксперимент 4
   - random seed in generation = 14
   - prompt "A oue dog" 
   - без train text-encoder

<p align="center">
<img src="image/exp4_14.png"  width="500" /> 
</p>

### Эксперимент 4_2
   - random seed in generation = 0
   - prompt "A oue dog"
   - train text-encoder

<p align="center">
<img src="image/exp_4_2_0.png"  width="500" /> 
</p>


Эксперимент [3](#Эксперимент-3) и [2](#Эксперимент-2) показали себя хорошо, но по разному; В 2 получились красивые картинки, но в машину мою собаку нейросеть не посадила, В 3 были снимки похуже, но собака села в машину; В 6 эксперименте я проверю возможно ли настроить процесс генерации так, чтобы получить качествунную картинку для всех 4х тестовых промтов.
### Эксперимент 5
Теперь я просто попытаюсь сгенерировать качественные картинки и обучить на меньшем датасете; Также буду более детально подходить к генерации каждого теста, больше варьировать seed и другие параметры; 
#### Van Gogh style
 - seed in generation = 6275943528424243
 - LORA_SCALE_UNET = 0.6
 - GUIDANCE = 6.4
 - LORA_SCALE_TEXT_ENCODER = 0.6
 - 450 train step(5 images), small_dog, seed=42

<p align="center">
<img src="image/on_small_vangogh.png"  width="500" /> 
</p>
Выглядит и правда похожим на мою собаку и стиль вроде ван-гога.

#### in red t-shorts
 - seed in generation = 6275943528424243
 - LORA_SCALE_UNET = 0.64
 - GUIDANCE = 6.4
 - LORA_SCALE_TEXT_ENCODER = 0.64
 - 450 train step(5 images), small_dog, seed=42

<p align="center">
<img src="image/in_red.png"  width="500" /> 
</p>

#### in the forest
Замечаем, что картинка сгенерированная очень похожа на картинку из обучающей выборки, об этой проблеме также писалось в статье. 

 - seed in generation = 5214883712862421
 - LORA_SCALE_UNET = 0.64
 - GUIDANCE = 6.4
 - LORA_SCALE_TEXT_ENCODER = 0.64
 - 450 train step(5 images), small_dog, seed=42

Слева картинка из датасета, справа сгенерированная! 

<p align="center">
<img src="image/datasets/small_dog/photo_2023-08-26 21.42.24.jpeg.png"   style='float:left;' width="300" /> 
<img src="image/in_forest.png"   style='float:left;' width="300" /> 
</p>

#### driving car
A rare_token dog is sitting at the wheel of the car, her paws on the steering wheel, and she looks into the camera. the whole dog is visible in the frame.
 - seed in generation = 8077461472812382
 - LORA_SCALE_UNET = 0.64
 - GUIDANCE = 6.4
 - LORA_SCALE_TEXT_ENCODER = 0.64
 - 450 train step(5 images), small_dog, seed=42

<p align="center">
<img src="image/in_car.png"   width="500" /> 
</p>


#### other:
<p align="center">
<img src="image/exp.png"   width="500" /> 
</p>

<p align="center">
<img src="image/exp.png"   width="500" /> 
</p>


## quickstart
Запускаем main.ipynb и следуем инструкциям
датасеты: в папке datasets, если хотите повторить мои исследование, закиньте их к себе на гугл диск или сделайте git clone моего репозитоерия себе в колаб;
[run in colab](https://colab.research.google.com/drive/1KAM4otIc21EJNqKrRCr-Bvg_akgvbKs-?usp=share_link)

## Выводы
Метод Dreamboth хороший способ для того, чтобы перенсти стиль, изменить локацию, свойства, одежду, действия у како-либо объекта без потери узноваимости объекта. Однако этот метод требует настройки и, чтобы действительно получать хорошие результаты нужно кропотливо настраивать параметры (редкое слово!) и подбирать обучающую выборку с качественными фотографиями. Однако, мой эксперимент показывает, что даже с фотогрфиями из галлереи телефона(иногда мутные, специфичные ракурсы, не однотипные) можно добиться результатов, которые были заявленны в статье. Мои эксперименты также подтвердили проблемы метода - переобучение, некоректное понимание контекста. 

В моих экспериментах лучше всего работали редкие слова <b>'oue','hta'</b>. Модель генерации хорошо справлялась с перемещением объекта в другую сцену и сменой стиля. Однкао со сложными сценами (собака ведет машину) модель справляется плохо - нужно хорошо постараться, чтобы заставить сгенерить собаку за рулем(я старался). Также смена одежды собаки сложна(видимо модель чаще видила маленьких собачек в футболках, а не больших) собака в красном всегда была маленького роста. 

### Веса
 - [Веса и результаты](https://disk.yandex.ru/d/SiG1T0mPAMZTTQ)

<p align="center">
<img src="image/bad_car.png" style='float:left;'   width="300" /> 
<img src="image/bad_car2.png"  style='float:left;'  width="300" />
</p>

<p align="center">
A http dog with red ball
<img src="image/red_ball.png"  style='float:left;'  width="300" />
</p>