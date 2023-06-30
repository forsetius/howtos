# Eksport do CSV

Eksport ma być dostępny tylko dla wybranych encji więc akcję kontrolera zdefiniujemy sobie w traicie, który będziemy mogli dodawać tylko do tych kontrolerów, które tego wymagają.
```php
use HelperBundle\Tool\Shortcuts as Sf;

trait CsvExportTrait
{
    public function exportCsvAction()
    {
        $sortDirection = $this->request->query->get('sortDirection');
        if (empty($sortDirection) || !in_array(strtoupper($sortDirection), ['ASC', 'DESC'])) {
            $sortDirection = 'DESC';
        }

        $queryBuilder = $this->createListQueryBuilder(
            $this->entity['class'],
            $sortDirection,
            $this->request->query->get('sortField'),
            $this->entity['list']['dql_filter']
        );

        return Sf::service('admin.tool.csv_exporter')->getResponseFromQueryBuilder(
            $queryBuilder,
            substr($this->entity['class'], strrpos($this->entity['class'], '\\')+1) .'.csv'
        );
    }
}
```

I dodajemy go do kontrolera dla encji, która będzie udostępniała eksport:
```php
<?php
namespace AdminBundle\Controller;
use AdminBundle\Controller\AdminController;

class RequestLogController extends AdminController
{
    use \AdminBundle\Controller\Traits\CsvExportTrait;
}
?>
```

```php
<?php
namespace AdminBundle\Tool;

use Doctrine\Common\Collections\ArrayCollection;
use Doctrine\ORM\QueryBuilder;
use Symfony\Component\HttpFoundation\StreamedResponse;

class CsvExporter
{
    public function getResponseFromQueryBuilder(QueryBuilder $queryBuilder, $filename)
    {
        $entities = new ArrayCollection($queryBuilder->getQuery()->getResult());
        $response = new StreamedResponse();

        $response->setCallback(function () use ($entities) {
            $values = $entities->current()->dumpValues());
            $handle = fopen('php://output', 'w+');
            // Add header
            fputcsv($handle, array_keys($values, ';');
            while ($values) {
                fputcsv($handle, $entity->dumpValues(), ';');
                $entities->next();
            }
            fclose($handle);
        });
        $response->headers->set('Content-Type', 'text/csv; charset=utf-8');
        $response->headers->set('Content-Disposition', 'attachment; filename="' . $filename . '"');

        return $response;
    }
}
?>
```

Usługa musi być skonfigurowana:
```yaml
services:
    admin.tool.csv_exporter:
        class:      AdminBundle\Tool\CsvExporter
        public:     true
```

Zdefiniujemy sobie odpowiedni interfejs, który będzie implementowany przez encje, które będą obsługiwać zrzut CSV
```php
// src/AdminBundle/Entity/Interfaces/CsvExportInterface
namespace AdminBundle\Entity\Interfaces;

interface CsvExportInterface
{
    public function dumpValues();
}
```

W klasie encji, która ma udostępniać zrzut do CSV implementujemy interfejs, czyli musimy zdefiniować funkcję `dumpValues()`, która musi zwracać tablicę z wartościami skalarnymi, które mogą być używane w pliku CSV (ciągi znaków i liczby).
```php
use AdminBundle\Entity\Interfaces\CsvExportInterface;

class RequestLog extends AbstractEntity implements CsvExportInterface
{
    // ...
    public function dumpValues()
    {
        return [
            'requestTime' => $this->getRequestTime()->format('Y-m-d H:i:s'),
            'httpStatus' => $this->getHttpStatus(),
            'httpStatusText' => $this->getHttpStatusExt()['name'],
            'methodName' => $this->getMethodName(),
            'object' => empty($this->getObject()) ? null : $this->getObject()->getName(),
        ];
    }
    // ...
```
