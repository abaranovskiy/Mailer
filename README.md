Kohana 3.3 Mailer module
================================
* Author: Alexander Baranovskiy
* Original: https://github.com/themusicman/Mailer


Basic Usage:
-------------------------------
* Customize the congfiguration setting in `/config/mailer.php`

```PHP
    return array
        (
        'production' => array(
            'transport' => 'smtp',
            'options' => array
                (
                'hostname' => 'smtp.mail.com',
                'username' => 'admin@theweapp.com',
                'password' => 'password',
                'encryption' => 'ssl',
                'port' => '465',
            ),
        ),
        'development' => array(
            'transport' => 'smtp'
        )
    );
```


* Create a class that extends the Mailer class and save it to `/classes/mailer/class_name.php` (Minus the `Mailer_`)


```PHP
    <?php defined('SYSPATH') or die('No direct script access.');
    class Mailer_User extends Mailer
    {
        public function before()
        {
            // To set the config by environment
            $this->config = Kohana::$environment;
        }
        public function welcome($args)
        {
            $this->to = array('tom@example.com' => 'Tom');
            $this->bcc = array('admin@theweapp.com' => 'Admin');
            $this->from = array('theteam@theweapp.com' => 'The Team');
            $this->subject = 'Welcome!';
            $this->attachments = array('/path/to/file/file.txt', '/path/to/file/file2.txt');
            $this->data = $args;
        }
    }
```



* Create the view for the email and save it to `/views/mailer/class_name/method_name.php` (Minus the Mailer_)


```html
    <p>Welcome <? = $user['name'];
        ?>,</p>
    <p>We are glad you signed up for our web app.  Hope you enjoy.</p>
    <p>The Team</p>
```

* 4. Use the Mailer_User class in one of your controllers

```PHP
    public function action_submit()
    {
        $data = array(
            'user' => array(
                'name' => 'Tom'
            )
        );
        Mailer::factory('user')->send_welcome();
        //you can also pass arguments to the mailer
        Mailer::factory('user')->send_welcome($data);
        //you can also use instance
        Mailer::instance('user')->send_welcome($data);
        //you can also pass arguments in factory
        Mailer::factory('user', 'welcome', $data);
        //you can also send direct from the controller
        Mailer::instance()
                ->to('du@kanema.com.br')
                ->from('du@kanema.com.br')
                ->subject('Welcome!')
                ->html('Welcome!')
                ->send();
    }
    
```

