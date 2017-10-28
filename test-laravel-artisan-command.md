---
layout: blog
title: 测试laravel commands的方法
date: 2017-10-27 17:21:41
tags:
- tools
- laravel
- test
categories:
- coding
---
## 引言
最近使用到laravel的consolo命令行工具，在编写命令，想写一些测试的时候，发现官方文档中并没有提到`command`的测试方法。花了点时间，翻墙找了资料，实践成功并记录一下，方便更多人。

<!--more-->

## 测试方法
大家都知道Laravel中使用了很多Symfony的成熟组件，Laravel的console组件使用的就是[Symfony/console](https://github.com/symfony/console)。

幸运的是，`Symfony/console` 组件中提供了用于command测试的`CommandTester`, 使用方法如下
```
...
use FooCommand;
use Symfony\Component\Console\Application;
use Symfony\Component\Console\Tester\CommandTester;
...

public function testSample(){
    //创建一个console测试应用平台，用来搭载测试的命令
    $application = new Application();
    
    //创建待测试的command
    $testedCommand = $this->app->make(FooCommand::class);
    //设置命令执行需要的laravel依赖
    $testedCommand->setLaravel(app());

    //添加待测试的command到测试应用上
    //同时command 也绑定 application
    $application->add($testedCommand);

    //实例化命令测试类
    $commandTester = new CommandTester($testedCommand);
    //命令输入流，对应每次交互需要提供的输入内容
    $commandTester->setInputs([
        //...
        ]);
    //执行命令
    $commandTester->execute(['command' => $testedCommand->getName()]);

    //对命令执行结果进行断言测试，主要是依靠正则判断
    //$commandTester->getDisplay() 方法可以获取命令执行后的输出结果
    $this->assertRegExp("/some reg/", $commandTester->getDisplay());

}

```

## 示例
我们现在有一个手动创建新用户的命令`createUser`,作用就是手动创建一个用户。

需要交互式让用户输入`name`,`email`,`password`,`comfirm password`,这些数据。

### 待测试的command

```
<?php

namespace App\Console\Commands;

use App\User;
use Illuminate\Auth\Events\Registered;
use Illuminate\Console\Command;
use Illuminate\Support\Facades\Validator;

class CreateUser extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'createUser';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'create new user for system manually';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $this->line($this->description);

        // 获取输入的数据
        $data = [
            'name' => $this->ask('What\'s your name?'),
            'email' => $this->ask('What\'s your email?'),
            'password' => $this->secret('What\'s your password?'),
            'password_confirmation' => $this->secret('Pleas confirm your password.')
        ];

        // 验证输入内容
        $validator = $this->makeValidator($data);
        if ($validator->fails()) {
            foreach ($validator->errors()->toArray() as $error) {
                foreach ($error as $message) {
                    $this->error($message);
                }
            }
            return;
        }

        // 向用户确认输入信息
        if (!$this->confirm('Confirm your info: ' . PHP_EOL . 'name:' . $data['name'] . PHP_EOL . 'email:' . $data['email'] . PHP_EOL . 'is this correct?')) {
            return;
        }

        // 注册
        $user = $this->create($data);
        event(new Registered($user));
        $this->line('User ' . $user->name . ' successfully registered');
    }

    /**
     * Get a validator for an incoming registration request.
     *
     * @param  array $data
     * @return \Illuminate\Contracts\Validation\Validator
     */
    protected function makeValidator($data)
    {
        return Validator::make($data, [
            'name' => 'required|string|max:255|unique:users',
            'email' => 'required|string|email|max:255|unique:users',
            'password' => 'required|string|min:6|confirmed'
        ]);
    }

    /**
     * Create a new user instance after a valid registration.
     *
     * @param  array $data
     * @return \App\User
     */
    protected function create($data)
    {
        return User::create([
            'name' => $data['name'],
            'email' => $data['email'],
            'password' => bcrypt($data['password'])
        ]);
    }
}

```

### 正确的结果

如果正确输入信息的话，会得到如下输出
```
$ path-to-your-app/app# php artisan createUser
create new user for system manually

 What's your name?:
 > vestin

 What's your email?:
 > correct@abc.com

 What's your password?:
 > 

 Pleas confirm your password.:
 > 

 Confirm your info: 
name:vestin
email:correct@abc.com
is this correct? (yes/no) [no]:
 > yes

User vestin successfully registered
```

### 想要测试的内容

我想要测试两块内容：
- 数据输入验证测试
    - email有效性测试
    - password两次输入是否相同的测试
- 正确创建用户测试


### 编写单元测试
```
<?php

namespace Tests\Unit\command;

use App\Console\Commands\CreateUser;
use Symfony\Component\Console\Application;
use Symfony\Component\Console\Tester\CommandTester;
use Tests\TestCase;
use Illuminate\Foundation\Testing\RefreshDatabase;

class CreateUserTest extends TestCase
{

    use RefreshDatabase;

    /**
     * 测试数据验证
     *
     * @return void
     */
    public function testValidation()
    {
        $application = new Application();

        $testedCommand = $this->app->make(CreateUser::class);
        $testedCommand->setLaravel(app());
        $application->add($testedCommand);

        $commandTester = new CommandTester($testedCommand);
        $commandTester->setInputs(['Vestin', 'badEmail@abc', '123456', '654321']);
        $commandTester->execute(['command' => $testedCommand->getName()]);

        // assert
        $this->assertRegExp("/The email must be a valid email address/", $commandTester->getDisplay());

        $commandTester->setInputs(['vestin', 'correct@abc.com', '123456', '654321']);
        $commandTester->execute(['command' => $testedCommand->getName()]);

        // assert
        $this->assertRegExp("/The password confirmation does not match/", $commandTester->getDisplay());
    }

    /**
     * 测试成功注册用户
     *
     * @return void
     */
    public function testSuccess()
    {
        $application = new Application();

        $testedCommand = $this->app->make(CreateUser::class);
        $testedCommand->setLaravel(app());
        $application->add($testedCommand);

        $commandTester = new CommandTester($testedCommand);
        $commandTester->setInputs(['Vestin', 'correct@abc.com', '123456', '123456', 'y']);
        $commandTester->execute(['command' => $testedCommand->getName()]);

        // assert
        $this->assertRegExp("/User Vestin successfully registered/", $commandTester->getDisplay());
        $this->assertDatabaseHas('users', [
            'email' => 'correct@abc.com',
            'name' => 'Vestin'
        ]);
    }
}

```

### 执行测试
```
$ path-to-your-app/app#  ./vendor/bin/phpunit 
PHPUnit 6.4.3 by Sebastian Bergmann and contributors.

..                                                                  3 / 3 (100%)

Time: 659 ms, Memory: 14.00MB
```


## 参考文档
* http://symfony.com/doc/current/components/console.html
* https://laracasts.com/discuss/channels/testing/how-do-you-test-your-commands-phpunit
* https://themsaid.com/building-testing-interactive-console-20160409
* https://laravelista.com/posts/testing-laravel-commands