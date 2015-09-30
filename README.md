# symfony2.7 Dev notes
#### controller
```php
use Symfony\Bundle\FrameworkBundle\Controller\Controller;
```
### annotation
```php
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Route;
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Template;

/**
 * @Route("/_exam")
 * @Template()
 */
public function _examAction()
{
    // ...
}
```
