# ~ Jquery Toast Plugin Documentation ~

본 문서는 Jquery Toast 의 한글 Documentation 입니다.

저 나름대로 정리해봤습니다. 겹치는 내용이 있을 수 있습니다.

기본 사용법
$.toast("Test toast message");

var options = {}

$.toast(options);

기본값

~~~
$.toast.options = {
    text: '',                   // String || Array / 문자열의 경우 그대로 $().html()로 출력, 배열의 경우 ul,li에 $().html()하여 출력
    heading: '',                // String / h1 태그에 담겨 출력. text와 별개로 출력됨
    showHideTransition: 'fade', // 'fade' || 'slide' || else / jquery의 fade, slide를 메서드를 활용하여 애니메이션
    allowToastClose: true,      //  close 버튼을 함께 출력. 반환값을 이용한 close 할때는 true 설정 필요
    hideAfter: 3000,            // Number / 몇 ms 후에 사라질지 설정
    loader: true,               // 토스트 메세지 위에 progress bar을 출력할 것인지 설정
    loaderBg: '#9EC600',        // 토스트 메세지 위에 progress bar의 색깔을 설정
    stack: 5,                   // 토스트 메세지가 최대 몇개까지 쌓일지 설정 (이상의 토스트 메세지가 생성될 경우 나중의 것은 자동 삭제)
    position: 'bottom-left',    // 'bottom-left' || 'bottom-right' || 'top-right' || 'top-left' || 'bottom-center' || 'top-center' || 'mid-center'
                                // 토스트 메세지의 생성 위치 설정. .jq-toast-wrap 의 css 속성을 변경하여 추가로 조작 가능
    bgColor: false,             // String / 토스트 메세지의 배경색 설정. css의 background-color 속성에 들어갈수 있는 값이면 됨.
    textColor: false,           // String / 토스트 메세지의 글자색 설정. css의 color 속성에 들어갈수 있는 값이면 됨.
    textAlign: 'left',          // String / 토스트 메세지의 글 정렬 설정. css의 text-align 속성에 들어갈수 있는 값이면 됨.
    icon: false,                // 'success' || 'error' || 'warning' || 'info' / 토스트 메세지의 사전 설정 사용. 다른 옵션들이 우선시되므로 추가 수정 가능
    beforeShow: function () {},     //beforeShow 이벤트 바인딩
    afterShown: function () {},     //afterShown 이벤트 바인딩
    beforeHide: function () {},     //beforeHide 이벤트 바인딩
    afterHidden: function () {},    //afterHidden 이벤트 바인딩
    onClick: function () {}         //onClick 이벤트 바인딩
};
~~~

내부 작동

## 1. setup

다음의 속성들을 설정합니다

toast.options.text
toast.options.heading
toast.options.bgColor
toast.options.textColor
toast.options.textAlign
toast.options.icon
toast.options.class

## 2. addToDom

.jq-toast-wrap이 없으면 추가

.jq-toast-wrap에 setup된 toast 메세지 추가

toast.options.stack = Number 토스트 메세지의 최대 개수 설정. 최대 개수를 초과하면 가장 처음에 만들어졌던 메세지는 자동으로 삭제됩니다.

## 3. position

toast.options.position 토스트 메세지의 위치를 지정합니다.

## 4.bindToast

토스트 메세지에 이벤트를 바인딩합니다

토스트 메세지 옵션의 이벤트를 실제 이벤트와 이어주는 역할을 합니다.

## 4-1 processLoader 작동

processLoader은 토스트 메세지 상단에 주황색(기본값) progress bar

processLoader은 toastEl의 afterShown 이벤트에 호출

this.options.hideAfter에 숫자값이 들어있거나(this.options.hideAfter는 (숫자)ms 이후에 자동으로 닫히는 속성) options.loader이 true이면 동작

## 5. animate

먼저 beforeShow 이벤트를 fire 합니다.

this.options.showHideTransition.toLowerCase()에 따라 fade 혹은 slide, show 합니다.

그 뒤에 afterShown 이벤트를 fire 합니다.

this.hideAfter에 숫자값이 있어 자동으로 사라지는 경우에, 

this.options.hideAfter 값만큼 setTimeout하여 

먼저 beforeHide 이벤트를 fire합니다.

this.options.showHideTransition.toLowerCase()에 따라 fade 혹은 slide, hide 합니다.

그 뒤에 afterHide 이벤트를 fire합니다.

beforeHide에 숫자값이 없는경우는 bindToast에서 bind한 onclick 이벤트에 beforeHide와 afterHidden 이벤트가 fire됩니다.

## 6. return

$.toast() 는 객체를 반환합니다.

객체는 reset, update, close 메서드를 가지고 있습니다.

$.toast().reset(what)
$.toast().update(options)
$.toast().close()

reset의 경우 인자로 all을 넣으면 전체 토스트 메세지를 삭제합니다. 그 외의 모든 경우는 자기 자신만을 삭제합니다.

update의 경우 setup과 bind 과정을 다시합니다. 따라서 hideAfter 시간을 고치는건 의미가 없습니다.

close의 경우 allowToastClose가 false로 되어있으면 close()메서드를 사용해도 꺼지지 않습니다.
