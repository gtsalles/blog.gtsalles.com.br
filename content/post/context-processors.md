+++
date = "2014-01-04T22:34:00-03:00"
title = "Context Processors"
slug = "context-processors"
categories = ["python",	"django"]
tags = ["django", "template", "context processors", "beerblogging"]

+++

Template Context Processors (TCP) é uma feature que o Django utiliza pra acessar variáveis em qualquer template. Provavelmente já os usamos diversas vezes sem perceber, seja no ``MEDIA_URL``, ``STATIC_URL``, debug ou mesmo no user, que retorna o usuário atualmente autenticado na sessão ou um objeto AnonymousUser caso não esteja autenticado.

Todos esses exemplos podem ser acessados simplesmente por ``{{ user }}``, ou ``{{ debug }}``, sendo que nenhuma dessas variáves está em nenhuma das views que escrevemos. Isso se dá porque no global_settings do Django, já existem alguns TCP pré-configurados. Por causa deles, é possível acessar essas (e outras) variáveis.

Um exemplo diferente de aplicação seria um situação onde você precisa acessar um objeto em diversos templates diferentes. Uma solução para isso seria a de incluir no contexto de cada view o determinado objeto. Isso foge totalmente ao princípio DRY.

Com TCP, o acesso ao objeto fica centralizado em um único arquivo na sua app, e, a partir daí, pode ser acessado em qualquer template. Mais ou menos isso:

```
# app_name/context_processors.py

from app_name.models import RandomModel

def method_name(request):
    """
    All objects of RandomModel will be available in any template
    just using {{ random }}
    """

    random_stuff = RandomModel.objects.all()
    return {'random': random_stuff}
```

```

# settings.py

from django.conf.global_settings import TEMPLATE_CONTEXT_PROCESSORS

TEMPLATE_CONTEXT_PROCESSORS += (
    'app_name.context_processors.method_name'
)
```

Antes de usar TCP, avalie bem se existe uma real necessidade para a sua aplicação. Pelo fato de ele estar setado no settings.py, ele sempre será executado independente de um template ou outro não o utilizar, isso pode gerar queries desnecessárias e com isso, perda de desempenho na aplicação. Em casos onde só é necessário acessar objetos em apenas alguns templates talvez o uso de [Custom Template Tags](https://docs.djangoproject.com/en/dev/howto/custom-template-tags/) seja mais indicado.

That's all ;)
