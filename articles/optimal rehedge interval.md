# Оптимальный интервал для рехеджа портфеля

_Небольшое отступление. После некоторых моих предыдущих постов, я получал сообщения о том, что не все было понятно. Я постараюсь писать более понятным языком для новичков, по-этому опытных людей прошу набраться терпения, и я буду только рад любым замечаниям и уточнениям._

В данной статье мы попробуем разобраться, как влияет дискретность хеджирования и стоимость транзакций на дельта-хеджирование (ДХ). О том, что такое ДХ и его механизм работы, можно почитать в моих предыдущих постах или любых других постах, но лучше всего в книгах.

ДХ по Блэку-Шоулзу (БШ), предполагает непрырывное хеджирование. Это означает, что ребалансировка портфеля должна происходить на бесконечно малых промежутках времени. И именно в таких условиях, ДХ по БШ обеспечивает хеджирование портфеля в полной мере, т.е. позволяет свести риски убытков к минимому.

А что происходит на практике? В реальной жизни у нас нет возможности бесконечно часто ребалансировать портфель: 
- Как бы мы того не хотели, мы можем ребаланировать портфель только через определенные промежутки времени (дискретно). 
- Даже если мы будем ребалансировать очень часто - это дорого с точки зрения транзакционных издержек.

Так и появилось мнение, что в реальной жизни ДХ не возможен. Но это тоже не совсем правда. Давайте на этом остановимся и посмотрим подробнее. 

Попробуем смоделировать ДХ на реальных данных. Для примера я взял данных с московской биржи, минутки для контракта SiH9 с 2019-01-01 по 2019-03-21, т.е. это время жизни этого фьючерса. 
http://iss.moex.com/iss/engines/futures/markets/forts/boards/RFUD/securities/SiH9/candles.jsonfrom=2019-01-01&till=2019-03-21&interval=1&start=0

Данные по стоимости опционов также взяты с московской биржи.
call https://iss.moex.com/iss/history/engines/futures/markets/options/securities/Si69500BC9/trades.json?from=2019-01-01
put https://iss.moex.com/iss/history/engines/futures/markets/options/securities/Si69500BO9/trades.json?from=2019-01-01

Продавать мы будем "стрэдл" в первый торговый день, около центрального страйка. Для красоты картины продавать будем по 10 опционов Put и Call, чтобы было что хеджировать.

Для начала смоделируем ДХ каждую минуту без транзакционных издержек и посмотрим на результат. Это будет наша точка отсчета, т.к. для моделирования более частого ДХ у нас не хватает данных. Ниже представлен график стоимости портфеля на каждом шаге.

[график 1 портфель с ДХ каждую минуту без транзакционных издержек]

Что влияет на дельта-хеджирование? Это дельта нашей позиции. Эта дельта выводится из формулы БШ и зависит в целом от нашей позиции и волатильности базового актива, по которой была продана позиция (кому интересно, как выводится дельта, можно найти в моих предыдущих постах).

Идеально, когда дельта нашей позиции близка к нулю. Это означает, что наши риски сведены к минимому. Когда дельта далека от нуля, мы стремимся продать или купить базовой актив (БА), чтобы уменьшить дельту. Это и есть хеджирование. 

Мы также знаем, что стоимость опциона равна стоимости его замещения. По-этому на рынке опционы часто стоят дороже теоретической цены, т.к. продавец учитывает различные издержки по замещению. И по-этому у разных продавцов разные цены, потому что все умеют замещать по-разному.

Но как в таком случае делать рехедж позиции, если классический ДХ по БШ бесконечно дорог? Существует несколько моделей:
- Модель Hoggard–Whalley–Wilmott (1992) - представляет собой не линейное дифференциальное уравнение, которое модифицирует сигму на основе транзакционных издержек, и ДХ предполагается делать по этой новой сигме.
- 

Но разве это единственный путь снизить риски?

Что если есть путь уменьшить дельту другим способом? Стоит вспомнить про гамму. Если дельта - это скорость изменения стоимости опциона в зависимости от цены БА, то гамма - это скорость изменения дельты в зависимости от цены БА. Чем меньше гамма, тем реже меняется дельта, тем соответственно реже ДХ и меньше расходых на рехедж.

Так к чему же надо стремиться? Стремиться нужно к снижению гаммы, путем корректирования опционной позиции.

Здесь стоит отметить, что решение уравнения по уменьшению гаммы существует только для проданной позиции. Не существует конечного решения для купленной позиции. 



Стоимость опциона равна стоимости его замещения.
Дельта хедж - это самый простой, но и самый дорогой способ замещения.

как выбрать оптимальный интервал для рехеджа?

всегда ли нужен рехедж?

дельтахедж по БШ на реальных данных
    
    привести пример ДХ на реальных данных, используя часовики

дельтахедж по БШ на реальных данных включая стоимость транзакций

    привести пример добавив транзакции и показать как просела прибыль

дискретный ДХ по БШ включая стоимость транзакций

    привести размышления о дискретности и стоимости транзакций
    показать, что можно подкорректировать сигму для ДХ
    попробовать что-нибудь сделать