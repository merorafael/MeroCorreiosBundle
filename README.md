MeroCorreiosBundle
==================

[![SensioLabsInsight](https://insight.sensiolabs.com/projects/67c1e408-3d2f-434d-87c9-f4b2e6b333dd/mini.png)](https://insight.sensiolabs.com/projects/67c1e408-3d2f-434d-87c9-f4b2e6b333dd)
[![Latest Stable Version](https://poser.pugx.org/mero/correios-bundle/v/stable.svg)](https://packagist.org/packages/mero/correios-bundle) 
[![Total Downloads](https://poser.pugx.org/mero/correios-bundle/downloads.svg)](https://packagist.org/packages/mero/correios-bundle) 
[![License](https://poser.pugx.org/mero/correios-bundle/license.svg)](https://packagist.org/packages/mero/correios-bundle)

Symfony Bundle with Correios integration

Requirements
------------

- PHP 5.4.9 or above
- SOAP extension
- Symfony 2.8 or above

Instalation with composer
-------------------------

1. Open your project directory;
2. Run `composer require mero/correios-bundle` to add MeroCorreiosBundle in your project vendor;
3. Open **my/project/dir/app/AppKernel.php**;
4. Add `Mero\Bundle\CorreiosBundle\MeroCorreiosBundle()`.

Correios Client
---------------

This bundle is only alias to use [MeroCorreios](https://github.com/merorafael/php-correios).
   
| Service              | MeroCorreios Class                                                                            |
| -------------------- | --------------------------------------------------------------------------------------------- |
| mero_correios.client | [Client](https://github.com/merorafael/php-correios/blob/master/src/Mero/Correios/Client.php) |
 

#### Usage example

```php
namespace Acme\Bundle\ApiBundle\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symfony\Component\HttpFoundation\Request;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;

/**
 * @Route("/correios")
 */
class CorreiosController extends Controller
{
    /**
     * @Route("/{zipCode}/address", name="search_zipcode")
     */
    public function searchAction(string $zipCode)
    {
        $client = $this->get('mero_correios.client'); // Return the Mero\Correios\Client
        try {
            $address = $client->findAddressByZipCode($zipCode);

            return new JsonResponse([
                'zip_code' => $zipCode,
                'address' => $address->getAddress(),
                'neighborhood' => $address->getNeighborhood(),
                'city' => $address->getCity(),
                'state' => $address->getState(),
            ]);
        } catch (AddressNotFoundException $e) {
            return new JsonResponse([
                'message' => $e->getMessage(),
            ], 404);
        } catch (InvalidZipCodeException $e) {
            return new JsonResponse([
                'message' => $e->getMessage(),
            ], 404);
        }
    }

}
```
