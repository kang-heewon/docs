# 릴리즈 노트

- [버전 관리 체계](#versioning-scheme)
- [지원 정책](#support-policy)
- [라라벨 5.7](#laravel-5.7)

<a name="versioning-scheme"></a>
## 버전 관리 체계

라라벨의 버전 관리 체계는 다음의 컨벤션을 유지합니다: `paradigm.major.minor`. 메이저 프레임워크 릴리즈는 6 개월마다 (2월, 8월) 릴리즈되며, 마이너 릴리스는 매주 여러번 릴리즈 될 수 있습니다. 마이너 릴리즈에는 이전 버전의 호환성을 깨뜨리는 변경 사항이 **없어야** 합니다.

애플리케이션에서 라라벨 프레임워크, 라라벨의 컴포넌트 또는 패키지를 참조할 때에, 라라벨의 메이저 릴리즈가 이전 버전과 호환성을 유지하지 못하는 변경사항을 포함하고 있기 때문에 항상 `5.7.*` 와 같이 참조하도록 해야 합니다. 변경사항에 대해서는 하루 안에 새로운 릴리즈를 업데이트 할 수 있도록 노력하고 있습니다.

패러다임 변경 릴리즈는 몇년간 구분되어 있으며, 프레임워크의 아키텍처와 컨벤션에 대한 근본적인 변경을 나타냅니다. 현재까지는 개발중인 버전에 대한 변경 발표는 없습니다.

<a name="support-policy"></a>
## 지원 정책

라라벨 5.5과 같은 LTS 릴리즈 동안에는, 2년간의 버그 픽스와 3년동안의 보안 패치가 지원됩니다. 이러한 릴리즈는 장기간에 걸친 지원과 유지보수를 제공합니다. 일반적인 릴리즈에서는 버그 픽스는 6개월, 보안 패치는 1년동안 제공됩니다.

| 버전 | 릴리즈 | 버그픽스 지원기간| 보안 패치 지원기간|
| --- | --- | --- | --- |
| 5.0 | 2015 년 2 월 4 일 | 2015 년 8 월 4 일 | 2016 년 2 월 4 일 |
| 5.1 (LTS) | 2015 년 6 월 9 일 | 2017 년 6 월 9 일 | 2018 년 6 월 9 일 |
| 5.2 | 2015 년 12 월 21 일 | 2016 년 6 월 21 일 | 2016 년 12 월 21 일 |
| 5.3 | 2016 년 8 월 23 일 | 2017 년 2 월 23 일 | 2017 년 8 월 23 일 |
| 5.4 | 2017 년 1 월 24 일 | 2017 년 7 월 24 일 | 2018 년 1 월 24 일 |
| 5.5 (LTS) | 2017 년 8 월 30 일 | 2019 년 8 월 30 일  | 2020 년 8 월 30 일 |
| 5.6 | 2018 년 2 월 7 일 | 2018 년 8 월 7 일 | 2019 년 2 월 7 일 |
| 5.7 | 2018년 9월 4일 | 2019년 3월 4일 | 2019년 9월 4일 |

<a name="laravel-5.7"></a>
## 라라벨 5.7

Laravel 5.7은 [라라벨 노바](https://nova.laravel.com)를 도입하고, 인증 스캐폴딩에 이메일 검증 (옵션), 인증 게이트 및 Policy의 게스트 사용자 지원, 콘솔 테스트 개선, Symfony `dump-server` 통합, 현지화 가능한 알림, 다양한 버그 수정 및 유용성 개선 등이 포함됩니다.

### Laravel Nova

[라라벨 노바](https://nova.laravel.com)는 Laravel을 위한 최상의 관리자 대시보드입니다. 물론 Nova의 핵심 기능은 Eloquent를 사용하여 기본 데이터베이스 레코드를 관리 할 수 있는 것입니다. 또한 Nova는 필터, 렌즈, 작업, 대기중인 작업, 메트릭, 권한 부여, 커스텀 도구, 커스텀 카드, 커스텀 필드 등을 지원합니다.

Laravel Nova에 대한 자세한 내용은 [Nova 웹 사이트](https://nova.laravel.com)를 확인하십시오.

### 이메일 검증

Laravel 5.7에서는 프레임워크에 포함 된 인증 스캐폴딩에 이메일 검증 옵션을 도입했습니다. 이 기능을 추가하기 위해 `email_verified_at` 타임스탬프 컬럼이 프레임워크에 포함 된 기본 `users` 테이블 마이그레이션에 추가되었습니다.

새로 가입한 사용자에게 자신의 이메일을 검증하게하려면, `User` 모델은 `MustVerifyEmail` 인터페이스를 추가해야합니다 :

    <?php

    namespace App;

    use Illuminate\Notifications\Notifiable;
    use Illuminate\Contracts\Auth\MustVerifyEmail;
    use Illuminate\Foundation\Auth\User as Authenticatable;

    class User extends Authenticatable implements MustVerifyEmail
    {
        // ...
    }

`User` 모델에 `MustVerifyEmail` 인터페이스를 추가하면, 새로 가입한 사용자는 검증 링크가 포함 된 이메일을 받게됩니다. 이 링크를 클릭하면 Laravel에서 자동으로 데이터베이스에 확인 시간을 기록하고 사용자를 원하는 위치로 리디렉션합니다.


기본 어플리케이션의 HTTP 커널에 `verified` 미들웨어가 추가되었습니다. 이 미들웨어는 확인된 사용자만 허용하는 경로에 추가 할 수 있습니다.

    'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,

> {tip} 이메일 검증에 대해 더 자세히 알아 보려면 [전체 문서](/docs/{{version}}/verification)를 확인하세요.

### 게스트 사용자의 Gates / Policies

이전 버전에서는 권한 부여 Gate 및 Policy가 인증되지 않은 게스트 사용자에게 `false` 를 자동으로 반환했습니다. 그러나 이제는 "옵션(optional)"유형의 타입힌트를 선언하거나 사용자 인수에 대한 `null` 기본값을 제공하여 권한 부여 검사를 통과 할 수 있습니다.

    Gate::define('update-post', function (?User $user, Post $post) {
        // ...
    });

### Symfony Dump Server

Laravel 5.7은 Symfony의 `dump-server` 커맨드와 [Marcel Pociot 패키지](https://github.com/beyondcode/laravel-dump-server)을 통합합니다. 시작하려면 `dump-server` Artisan 커맨드를 실행하세오 :

    php artisan dump-server

서버가 시작되면 `dump` 에 대한 모든 요청은 브라우저 대신 `dump-server` 콘솔 창에 표시되어 HTTP 응답 출력을 mangling 하지 않고 값을 확인 할 수 있습니다.

### 알림 현지화

이제 Laravel을 사용하면 현재 언어가 아닌 언어로 알림을 보낼 수 있으며 알림이 대기중인 경우에도 이 언어를 기억할 수 있습니다.

이를 위해 `Illuminate\Notifications\Notification` 클래스가 원하는 언어를 설정하는 `locale` 메소드를 제공합니다. 알림의 양식이 생성 될 때 어플리케이션은 이 언어로 변경되고 양식의 생성이 완료되면 이전 언어로 돌아갑니다.

    $user->notify((new InvoicePaid($invoice))->locale('es'));

알림 가능한 여러 항목의 현지화는 `Notification` 파사드를 통해 이루어질 수도 있습니다.

    Notification::locale('es')->send($users, new InvoicePaid($invoice));

### 콘솔 테스팅

Laravel 5.7에서는 `expectsQuestion` 메소드를 사용하여 콘솔 명령에 대한 사용자 입력을 쉽게 "모킹" 할 수 있습니다. 또한 `assertExitCode` 및 `expectsOutput` 메소드를 사용하여 콘솔 명령으로 출력 할 것으로 예상되는 종료 코드와 텍스트를 지정할 수 있습니다. 예를 들어 다음과 같은 console 명령을 생각해보십시오 :


    Artisan::command('question', function () {
        $name = $this->ask('What is your name?');

        $language = $this->choice('Which language do you program in?', [
            'PHP',
            'Ruby',
            'Python',
        ]);

        $this->line('Your name is '.$name.' and you program in '.$language.'.');
    });

이 명령을 다음 테스트와 같이 `expectsQuestion`,`expectsOutput` 및`assertExitCode` 메소드를 사용하여 테스트 할 수 있습니다 :

    /**
     * Test a console command.
     *
     * @return void
     */
    public function test_console_command()
    {
        $this->artisan('question')
             ->expectsQuestion('What is your name?', 'Taylor Otwell')
             ->expectsQuestion('Which language do you program in?', 'PHP')
             ->expectsOutput('Your name is Taylor Otwell and you program in PHP.')
             ->assertExitCode(0);
    }

### URL 생성기 및 호출 가능한 구문

Laravel의 URL 생성기는 컨트롤러 동작에 대한 URL을 생성 할 때 문자열만 받아들이는 대신 "호출 가능한(callable)" 구문을 허용합니다.

    action([UserController::class, 'index']);

### Paginator Links

Laravel 5.7에서는 paginator가 URL "창"의 각 사이드에 표시하는 추가 링크의 수를 변경 할 수 있습니다. 기본적으로 세 개의 링크는 기본 paginator 링크의 각 사이드에 표시됩니다. 그러나 `onEachSide` 메소드를 사용하여 이 숫자를 변경 할 수 있습니다 :


    {{ $paginator->onEachSide(5)->links() }}

### 파일시스템의 Read / Write 스트림

Laravel의 통합 파일시스템은 이제 `readStream` 과 `writeStream` 메소드를 제공합니다 :

    Storage::disk('s3')->writeStream(
        'remote-file.zip',
        Storage::disk('local')->readStream('local-file.zip')
    );
