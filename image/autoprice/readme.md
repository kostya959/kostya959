Представьте, что вы выставляете свой автомобиль на продажу на Авито. Вы загружаете фотографии, но не знаете, какую цену поставить, чтобы машина быстро нашла покупателя и при этом не продешевить.
В этой задаче вам предстоит помочь в решении реальной индустриальной проблемы - автоматического прайсинга автомобилей. Такой сервис помогает продавцам корректно оценивать стоимость машины, ускоряя продажу и делая рынок б/у авто более прозрачным.
Перед вами фотографии автомобилей с объявлений Авито за 2024 год - максимум 4 изображений на каждый объект, в основном внешний вид.

Тренировочная выборка: 70 000 объектов и 273 873 фотографии.
Тест: 25 000 автомобилей и 98 075 фотографий.
Задание:
Постройте модель, которая для каждого автомобиля из тестовой выборки предскажет его стоимость.

Данные:
train_dataset.parquet - обучающая выборка, целевая переменная: price_TARGET.
test_dataset.parquet - тестовая выборка.
sample_submission.csv - шаблон отправки (ID,target).
по этой ссылке доступны файлы train_images.zip и test_images.zip - это фотографии автомобилей, например, 100000_0.jpg, где 100000 - это ID объявления.
Разделение выполнено по времени: train - это объявления с января по ноябрь 2024 года, а test - объявления за декабрь 2024 года.
Помимо фотографий автомобилей вам доступны и некоторые атрибуты объектов:
body_type - тип кузова
drive_type - тип привода
engine_type - тип двигателя
doors_number - количество дверей
color - цвет
pts - вид ПТС
close_date - дата закрытия объявления
steering_wheel - руль
crashes_count - число ДТП
owners_count - число владельцев
mileage - пробег
latitude, longitude - координаты локации продажи
equipment - комплектация
Одновыборные опции (строка с одним значением): audiosistema, diski, electropodemniki, fary, salon, upravlenie_klimatom, usilitel_rul
Мультивыбор-поля (список опций; отсутствие - [None]): aktivnaya_bezopasnost_mult, audiosistema_mult, shini_i_diski_mult, electroprivod_mult, fary_mult, multimedia_navigacia_mult, obogrev_mult, pamyat_nastroek_mult, podushki_bezopasnosti_mult, pomosh_pri_vozhdenii_mult, protivoygonnaya_sistema_mult, salon_mult, upravlenie_klimatom_mult.
Метрика:
В качестве метрики качества используется medianAPE (Median Absolute Percentage Error):

m
e
d
i
a
n
A
P
E
=
median
i
(
|
^
y
i
−
y
i
|
y
i
)


где 
y
i
 - правильный ответ, 
^
y
i
 - предсказание модели.