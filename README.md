 Калькулятор на HTTP сервере Golang

1. данный проект может считать арифмитические выражения,переданные через HTTP-запрос, с использованием операций:
# + сложение
# - вычитание
# * умножение
# / деление
# (1+1) использование скобок


2. в  калькуляторе работает приоритет операций:
1 умножение и деление
2 сложение и вычитание


3. Запуск проекта
 1 установите [Golang](https://go.dev/dl/)*
 2 сохраните [проект](https://github.com/WO1N123/YandexLic123.git)*
 3 откройте терминал и пропишите команду для запуска сервера

go run ./cmd/main.go

4. если вы получили сообщение:

(Дата запуска самого сервера) сервер запущен

значит сервер запустился корректно

после того, как вы запустили сервер, создайте новый терминал(или он сам откроется)( все терминалы в CTRL+SHIFT+5), что отправить запрос
 
 
5. после того, как вы открыли новый терминал, вы должны прописать:

Invoke-WebRequest -Method 'POST' -Uri 'http://localhost:8080/api/v1/calculate' -ContentType 'application/json' -Body '{"expression":"2+2*2"}' | Select-Object -Expand Content


Статусы сервера

200 - StatusOK 
всё работает корректно и вы получите ответ

422 - StatusUnprocessableEntity 
входные данные не соответствуют требованиям — например, кроме цифр и разрешённых операций присутствует символ английского алфавита. убедитесь, что ваше выражение было написано корректно

405 - StatusMethodNotAllowed
Метод, используемый в запросе, не соответствует для данного сервера. в прошлом important сказано, какой стоит использовать. убедитесь, что используете метод POST

500 - StatusInternalServerError
в случае какой-либо иной ошибки («Что-то пошло не так»)

Пример ответов сервера

200 (StatusOK)


Invoke-WebRequest -Method 'POST' -Uri 'http://localhost:8080/api/v1/calculate' -ContentType 'application/json' -Body '{"expression":"2+2*2"}' | Select-Object -Expand Content

результат

{"result":"6.00"}

422 (StatusUnprocessableEntity) ошибка пользователя в написании выражения

Invoke-WebRequest -Method 'POST' -Uri 'http://localhost:8080/api/v1/calculate' -ContentType 'application/json' -Body '{"expression":"2+2*"}' | Select-Object -Expand Content

результат

Invoke-WebRequest : {"error":"expression is not valid"}
строка:1 знак:1
+ Invoke-WebRequest -Method 'POST' -Uri 'http://localhost:8080/api/v1/c ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

405 (StatusMethodNotAllowed) не правильный метод

Invoke-WebRequest -Method 'GET' -Uri 'http://localhost:8080/api/v1/calculate' -ContentType 'application/json' -Body '{"expression":"2+2*2"}' | Select-Object -Expand Content


результат

Invoke-WebRequest : Невозможно отправить тело содержимого с данным типом предиката.
строка:1 знак:1
+ Invoke-WebRequest -Method 'GET' -Uri 'http://localhost:8080/api/v1/ca ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Invoke-WebRequest], ProtocolViolationException
    + FullyQualifiedErrorId : System.Net.ProtocolViolationException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
