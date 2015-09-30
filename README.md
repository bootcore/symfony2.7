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
#### POST And GET Request
```php
use Sensio\Bundle\FrameworkExtraBundle\Configuration\Method;

/**
 * @Method("GET")
 */
public function indexAction()
{
    // ...
}
```
#### use ParamConverter automatic get data
```php
use Sensio\Bundle\FrameworkExtraBundle\Configuration\ParamConverter;

/**
 * @ParamConverter("post", class="AppBundle:Post")
 */
public function indexAction(Post $post)
{
    $title = $post->getTitle();

    // ...
}
```
#### Response
```php
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\RedirectResponse;
```
### Doctine 更新数据
```php
$em->persist($objectData);
$em->flush();
```
### 添加数据
```php
$Table = new Table();
$Table->setAttribute('value');

$em = $this->getDoctrine->getManager();
$em->persist($Table);
$em->flush();
```
#### 使用ParamConverter自动查询URL参数所对应的Entity
```php
/**
 * @Route("/detail/{id}")
 * @ParamConverter("post", class="AppBundle:Post")
 */
public function detailAction(Post $post)
{
    $title = $post->getTitle();

    // ...
}
```
#### Doctrine ORM 生命周期管理
```php
/**
 * ...
 * @ORM\HasLifecycleCallbacks();
 */
class Demo {
    /**
     * @ORM\PrePersitt()
     * @param \DateTime $createTime
     * @return Table
     */
    public function setCreateTime()
    {
        $this->createTime = new \DateTime();

        return $this;
    }
}
```
#### 执行SQL语句
```php
$this->get('database_connetion')->fetchAll("select * from ...");
```
#### dump数据
```php
use Doctrine\Common\Util\Debug;
Debug::dump($data);
```
#### symlink
php app/console assets:install --symlink --relative

#### 自定义配置
```php
$this->container->getParameter('config_name');
```
#### Form example
```php
$User = new User();
$form = $this->createFormBuilder($User)
    ->add('email')
    ->add('password')
    ->add('age')
    ->add('submit', 'submit')
    ->getForm();
return array('form' => $form->createView());
```
#### twig
```twig
{{ form(form) }}
```
#### 获取form表单
```php
$form->handleRequest($this->getRequest());
if ($form->isValid()) {
    $em = $this->getDoctrine->getManager();
    echo $User->getEmail();
    $em->persist($User);
    $em->flush();
}
```
#### 禁用html5提示
```php
$form = $this->createFormBuilder($User)
    ->add('email')
    ->add('password')
    ->add('age')
    ->add('submit', 'submit', array("attr" => array("formnovalidate" => "formnovalidate")))
    ->getForm();
```
#### Entity 表单验证
```php
use Symfony\Component\Validator\Constants as Assert;
/**
 * ...
 * @Assert\Length(min=6, max=30)
 */
protected $password;
```
#### 构造表单的时候进行验证
```php
$form = $this->createFormBuilder($User)
    ->add('email')
    ->add('password')
    ->add('age', 'integer', array("constants" => new GreaterThanOrEqual(18)))
    ->add('submit', 'submit')
    ->getForm();
```
#### 设置表单的提交地址、方法
```php
$form = $this->createFormBuilder($User)
    ->setAction($this->generateUrl('url'))
    ->setMethod('GET')
    ->add('email')
    ->add('password')
    ->add('age')
    ->add('submit', 'submit')
    ->getForm();
```
#### 文件上传
```php
$form = $this->createFormBuilder($User)
    ->add('file', 'file')
    ->add('submit', 'submit')
    ->getForm();
$form->handleRequest($this->getRequest());
if ($form->isValid()) {
    $form->get('file')->getData()->move($dir, $filename);
    $User->setThumb($filename);
}
```
#### symfony config
Reference>>framework
### 禁用表单默认的csrf
config.yml
csrf_protection: enabled|field_name
enabled: true|false