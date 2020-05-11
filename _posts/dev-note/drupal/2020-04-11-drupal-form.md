---
title: "Drupal Form API를 사용하여 새로운 페이지 만들기"
excerpt: "포스트 예시"

categories:
    - study
    - drupal
tags:
    - drupal

gallery:
  - url: /assets/images/drupal/rokebi-cash.png
    image_path: /assets/images/drupal/rokebi-cash.png
    alt: "Form 예제 1"
    title: "Form 예제 1"

last_modifieed_at: 2020-04-14T14:00:00+09:00
---
Drupal에서 새로운 URL과 페이지를 만들고 사용자 input을 처리하기 위해서는 Form API를 사용한다.   
예를 들어 다음과 같이 여러개 필드로 구성된 페이지를 만들기 위해서는 아래와 같이 처리한다.  
1. mymodule.route.yml 파일 생성

{% include gallery caption="Form 예제 #1" %}

## 디렉토리 구성
Custom 모듈을 생성한 후 다음과 같이 디렉토리를 구성한다. 
- mymodule
    * mymodule.routing.yml
    * src
        - Form
            * RokebiChangeBalanceForm.php


## mymodule.routing.yml 파일 생성
이 파일은 mymodule 맨 위 디렉토리에 위치하며, 추가될 페이지의 path를 설정한다.

```
rokebi.change_balance_form:
  path: '/admin/op/account/{account}/balance'
  defaults:
    _form: 'Drupal\rokebi\Form\RokebiChangeBalanceForm'
    _title: 'Change Rokebi Cash'
  requirements:
    _permission: 'access content'
```

- `rokebi.change_balance_form`는 추가될 form의 ID
- Form path는 `/admin/op/account/{account}/balance`로 설정
- Form path의 `{account}`는 변수로 해당 소스 코드에 같은 이름(`$account`)으로 전달된다. 
- `_form`은 Form을 처리할 소스 코드 이름으로 `src/Form/RokebiChangeBalanceForm.php` 이다. 
- `_title`은 페이지의 제목
- `_permission`은 페이지 접근 권한


## 소스 코드 
소스 코드는 크게 3부분으로 구성된다. 

### 시작 부분
`namespace`를 선언하고, 코드에서 사용할 다른 모듈을 불러온다.
그리고, 페이지를 구성할 새로운 클래스 `RokebiChangeBalanceForm`을 `FormBase`의 하위 클래스로 선언한다.

```
<?php

namespace Drupal\rokebi\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;
use Drupal\node\Entity\Node;

class RokebiChangeBalanceForm extends FormBase {
```

### Form ID 선언
Form ID를 반환하는 `getFormId()` 함수를 작성한다. 
해당 form ID는 유일한 값이어야 하며, `hook_form_FORM_ID_alter()`와 같이 다른 hook에서 사용될 수 있는 PHP 함수 이름 규정을 따라야 한다. 
즉, 문자, 숫자와 '_'로 구성한다.

```
  public function getFormId() {
    return 'rokebi_change_balance_form';
  }
```

### Form 구성 
Form을 구성하는 `buildForm()` 함수를 작성한다.
routing.yml 파일에서 path 이름에 변수를 사용하지 않은 경우는 2개의 파라미터만 갖는다. 
위 예제와 같이 `{account}` 변수를 사용한 경우, 3번째 파라미터로 전달된다. 
변수가 여러개면 4,5번째 파라미터로 차례로 전달된다.

```
  public function buildForm(array $form, FormStateInterface $form_state, string $account = NULL) {
```

Form API를 사용해서 필드를 구성한다.
사용 가능한 각종 필드에 대한 정보는 Drupal 홈 페이지를 참고한다. https://api.drupal.org/api/drupal/elements/8.2.x

본 에제에서 사용된 필드는 다음과 같다.

#### hidden field
Path로 전달된 `{account}` 값을 hidden 필드에 저장한다. 이 값은 `submitForm()`에 전달되어 사용된다.
```
    $form['account'] = [
      '#type' => 'hidden',
      '#value' => $account
    ];
```

#### textfield
Textfield는 가장 자주 사용된다. 
- `#title`에 제목을 지정한다.
- `#attributes`는 필드의 여러가지 속성을 지정한다. `disable` 값을 설정하면, 해당 필드를 비활성화 할 수 있다.
- `#deault_value`에 기본 값을 설정한다.
```
    $form['balance'] = [
      '#type' => 'textfield',
      '#title' => $this->t('로깨비 캐쉬'),
      '#weight' => 10,
      '#attributes' => ['disabled' => 'disabled'],
      '#default_value' => '0'
    ];
```

#### textfield
Number는 숫자 값을 입력 받을 때 사용한다.
- `#required`를 TRUE로 설정하면, 필수 입력 값이 된다. 
```
    $form['delta'] = [
      '#type' => 'number',
      '#title' => $this->t('변경 금액'),
      '#required' => TRUE,
      '#weight' => 20,
      '#default_value' => 0,
    ];
```

#### select
Select box UI가 필요한 경우 'select' 필드를 사용한다.
- `#options`에 선택 가능한 값을 배열 형태로 지정한다.
```
    $form['reason'] = [
      '#type' => 'select',
      '#title' => $this->t('변경 사유'),
      '#required' => TRUE,
      '#weight' => 30,
      '#options' => self::REASON, 
    ];
```

#### textarea
여러줄의 문자열을 입력 받을 때 사용한다.
- `#rows`에 UI 입력 줄 수를 지정한다.
- `#states`에는 다른 필드의 값이나 상태에 따라서 변하는 UI 상태를 지정할 수 있다. 
예를 들어 다음 예제는 'reason' 필드의 값이 'etc'로 선택된 경우에만 화면에 보여지도록 설정한다.
```

    $form['other_reason'] = [
      '#type' => 'textarea',
      '#title' => $this->t('그외 사유'),
      '#weight' => 100,
      '#rows' => 10,
      '#states' => [
        'visible' => [
          [':input[name="reason"]' => ['value' => 'etc']]
        ]
      ]
    ];
```

#### Submit
Form을 처리하기 위한 버튼을 설정한다.
```
    $form['actions'] = [
      '#type' => 'actions',
      '#weight' => 10,
    ];

    // Add a submit button that handles the submission of the form.
    $form['actions']['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('저장'),
    ];

    return $form;
  }
```

### Validation 처리
Form에 입력된 값이 유효한지 검사하는 `validateForm()`을 작성한다.
```
  public function validateForm(array &$form, FormStateInterface $form_state) {
    parent::validateForm($form, $form_state);

    if ($form_state->getValue('reason') == 'etc' &&
      strlen(trim($form_state->getValue('other_reason'))) < 5) {
      $form_state->setErrorByName('other_reason', $this->t('사유가 너무 짧습니다. 5자 이상 필요.'));
    }
  }
```


### Submit handler
Form 입력 값을 처리하는 `submitForm()`을 작성한다.
```
  public function submitForm(array &$form, FormStateInterface $form_state) {

    if (!$form_state->getErrors()) {

    }
  } 
```
