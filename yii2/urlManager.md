# UrlManager

```php
<?php
namespace backend\modules\helpdesk\controllers\katherine;

use backend\components\NoAuthBaseController;

/**
 * Controller class for helpdesk debug
 **/
class TestController extends NoAuthBaseController
{
    public function actionHello()
    {
        return 'bingo';
    }
}

```

`http://ajax.wm.com/5b69315c9cd0b3447c314882/api/helpdesk/katherine/test/hello`

Match

```php
'<accountId:[0-9a-f]{24}>/api/<module:\w+>/<submodule222:\w+>/<controller:[\w-]+>/<action:[\w-]+>' => '<module>/<submodule222>/<controller>/<action>'
```

### Stack

- base/Application - `run()`
    - `getRequest()`; It return the request components defined in main.php
    - `handleRequest()`
- web/Application - `handleRequest()`
    - `$route = $request->resolve()`
    - web/Request - resolve()
        - `$result = $app->getUrlManager()->parseRequest()`, if no matched, it will throw `Page not found` exception; In this case, it got UrlRule/parseRequest() response, see below
        - web/UrlManager - `parseRequest()`
            - foreach $rules and get `submodule222` rule structure
            - `return $rule->parseRequest()`
        - web/UrlRule - `parseRequest()`
            - generate below structure and return:

                ```
                array(2) {
                  [0]=>
                  string(29) "helpdesk/katherine/test/hello"
                  [1]=>
                  array(1) {
                    ["accountId"]=>
                    string(24) "5b69315c9cd0b3447c314882"
                  }
                }
                ```

- Module.php
    - `runAction("helpdesk/katherine/test/hello")`
    - `createController("katherine/test/hello")`
    - `createControllerById("katherine/test")` generate `"backend\modules\helpdesk\controllers\katherine\TestController"`
    - `class_exists` auto load class file
    - class name is also should be matched


TRICK:

If you want to see full stack, you can define `namespace` to `{namespace}\arbitrary_string`

```json
{
    "name": "Unknown Class",
    "message": "Unable to find 'backend\\modules\\helpdesk\\controllers\\katherine\\TestController' in file: /srv/omnisocials-backend/src/backend/modules/helpdesk/controllers/katherine/TestController.php. Namespace missing?",
    "code": 0,
    "type": "yii\\base\\UnknownClassException",
    "file": "/srv/omnisocials-backend/src/vendor/yiisoft/yii2/BaseYii.php",
    "line": 296,
    "stack-trace": [
        "#0 [internal function]: yii\\BaseYii::autoload('backend\\\\modules...')",
        "#1 [internal function]: spl_autoload_call('backend\\\\modules...')",
        "#2 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/base/Module.php(640): class_exists('backend\\\\modules...')",
        "#3 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/base/Module.php(597): yii\\base\\Module->createControllerByID('katherine/test')",
        "#4 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/base/Module.php(589): yii\\base\\Module->createController('hello')",
        "#5 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/base/Module.php(522): yii\\base\\Module->createController('katherine/test/...')",
        "#6 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/web/Application.php(105): yii\\base\\Module->runAction('helpdesk/kather...', Array)",
        "#7 /srv/omnisocials-backend/src/vendor/yiisoft/yii2/base/Application.php(386): yii\\web\\Application->handleRequest(Object(yii\\web\\Request))",
        "#8 /srv/omnisocials-backend/src/backend/web/index.php(18): yii\\base\\Application->run()",
        "#9 {main}"
    ]
}
```

