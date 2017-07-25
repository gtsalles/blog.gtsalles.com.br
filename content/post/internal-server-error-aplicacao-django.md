+++
date = "2013-05-28T19:13:00-03:00"
title = "Internal Server Error no Deploy de Aplicação Django"
slug = "internal-server-error-no-deploy-de-aplicacao-django"
categories = ["python",	"django"]
tags = ["django", "allowed hosts", "erro 500", "deploy", "beerblogging"]

+++

Recentemente desenvolvi um projeto em Django e depois do deploy, quando o `DEBUG` foi setado para `False`, o site começou gerar o erro 500.

Perambulando por aí descobri que como medida se segurança, as versões mais recentes do Django introduziram uma configuração chamada allowed hosts pra prevenir ["envenenamento de cache"](http://pt.wikipedia.org/wiki/DNS_cache_poisoning). Essa configuração é uma lista dos hosts/domínios os quais esse site pode servir.

Para resolver o problema, basta incluir no seu `settings.py` a configuração `ALLOWED_HOSTS` contendo uma lista com os seus endereços de domínio e/ou IP's do seu servidor.

`ALLOWED_HOSTS = ['exemplo.com.br', '11.22.33.44', '127.0.0.1']`

Espero ter ajudado.

Agradecimentos a [Gordon Luk](http://getluky.net/2013/02/21/django-debugfalse-and-allowed_hosts/) e [Henrique Bastos](https://twitter.com/henriquebastos).
