# Akcje globalne w EasyAdminie

W pliku głównego configa EasyAdmina dodajemy parametr z listą akcji globalnych. Oprócz tego linkujemy ten parametr do konfiguracji Twiga. Będziemy też modyfikować szablon listy, więc trzeba upewnić się, że w sekcji EasyAdmina zadeklarowaliśmy używanie własnego szablonu listy.
```yaml
# src/AdminBundle/Resources/config/admin.yml
parameters:
    global_actions:
        - exportCsv

twig:                                           # patrz ../Twig/Zmienne-globalne.md
    globals:
        global_actions: '%global_actions%'      # link do parametru 'global_actions'

easy_admin:
    design:
        templates:
            list: '@Admin/easy_admin/list.html.twig'
```

Oryginalny EasyAdminowy szablon `list.html.twig` ma już blok do akcji globalnych, który wykorzystywany jest do wyświetlania pola wyszukiwania i przycisku dla akcji `new`. Rozszerzymy go **_(*1)_** by wyświetlał przycisk **_(*4)_** dla każdej akcji globalnej **_(*2)_** zdefiniowanej wyżej w kluczu `twig/globals`. Aby przycisk akcji wyświetlał się tylko dla tych encji, dla których wpiszemy nazwę akcji w pliku configa EA encji, dodajemy sprawdzanie w **_(*3)_**.

W **_(*5)_** tworzymy linka do akcji, która oczywiście musi być zdefiniowana w kontrolerze. W funkcji `path` podajemy nazwę routa a `_request_parameters` daje dostęp do zmiennych z zapytania w URL-u (od ?), do których musimy dodać naszą akcję.

```twig
{# src/Admin/Resources/views/easy_admin/list.html.twig #}
{% extends '@EasyAdmin/default/list.html.twig' %}

{% block global_actions %} {# *1 #}
    {{ parent() }}
    {% for global_action in global_actions %} {# *2 #}
        {% if easyadmin_action_is_enabled_for_list_view(global_action, _entity_config.name) %} {# *3 #}
            <div class="button-action"> {# *4 #}
                <a class="btn btn-primary" href="{{ path('easyadmin', _request_parameters|merge({ action: global_action })) }}"> {# *5 #}
                    {{ ('app.action.' ~ global_action)|trans }} {# *6 #}
                </a>
            </div>
        {% endif %}
    {% endfor %}
{% endblock global_actions %}
```

W **_(*6)_** jako tekstu na przycisku używamy etykiet tłumaczenia, które dodamy w pliku z tłumaczeniami.

```yaml
# src/Admin/Resources/translations/messages.pl.yml
app.action.exportCsv: Eksport CSV
```

Jednak jeśli po prostu dodamy w pliku configa encji klucz `list/actions` z nazwą akcji to pojawi się ona dla każdego wiersza listy. Aby tego uniknąć rozszerzamy w szablonie listy
blok `item_actions`
```twig
{# src/Admin/Resources/views/easy_admin/list.html.twig #}
{% block item_actions %}
    {% set _list_item_actions = _list_item_actions|filter_admin_actions(item) %}
    {{ parent() }}
{% endblock %}
```

Jest tu użyta funkcja `filter_admin_actions()`, którą dopiero musimy zdefiniować.
```php
<?php
namespace AdminBundle\Twig;

use HelperBundle\Tool\Shortcuts as Sf;
use HelperBundle\Tool\Store;

class FilterActionsExtension extends \Twig_Extension
{
    public function getFilters()
    {
        return [
            new \Twig_SimpleFilter(
                'filter_admin_actions',
                [$this, 'filterActions']
            )
        ];
    }

    public function filterActions(array $itemActions, $item)
    {
        if (method_exists($item, 'filterActions')) {
            $itemActions = $item->filterActions($itemActions, $item);
        }
        return array_intersect_key(
            array_diff_key($itemActions, array_flip(Sf::param('global_actions'))),
            array_flip(Store::get('allowedActions'))
        );
    }
}
?>
```

Rozszerzenie Twiga trzeba zarejestrować jako usługę
```yaml
services:
    admin.twig_extension.filter_admin_actions:
        class:      AdminBundle\Twig\FilterActionsExtension
        public:     false
        tags:
            - { name: twig.extension }
```

Na koniec trzeba w configu EA dla encji ustawić akcje globalne w widoku listy. W kluczu `easy_admin/entities/<Entity>/list/actions` należy podać *wszystkie* akcje, jakie mają być dostępne w tym widoku - również standardowe i te, które wyświetlają się w każdej pozycji.
```yaml
easy_admin:
    entities:
        RequestLog:
            list:
                actions: ['show', 'exportCsv']
```
