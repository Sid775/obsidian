[[Java]]
[[K8s]]

Request памяти должен быть равен limit так как k8s не может вернуть в idle память которая была однажды выделена поде, потому что в случае с Java выделенная хипу память чисто теоретически может быть возвращена операционной системе но т.к. это очень затратная операция - можно на это не рассчитывать.
Следовательно - лучше поставить реквест равный лимиту и упасть сразу при попытке развернуть приложение чем словить OOM в рантайме.