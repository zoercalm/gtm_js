# [Cheat sheet](https://www.markdownguide.org/cheat-sheet/)

### для метрики отправка переменных через GTM: 

`<script>
// Получаем значения переменных из GTM
var pagePath = '{{Page Path}}'; // Переменная GTM
var cleanRating = '{{cjs - clean rating}}'; // Переменная GTM

// Создаем объект параметров для Яндекс Метрики
window.yaParams = {
Рейтинг статей: {}
};

// Динамически добавляем значения переменных
window.yaParams["Рейтинг статей"][pagePath] = [
cleanRating
];

// Отправляем данные в Яндекс Метрику
ym(96865152, 'params', window.yaParams || {});
</script>`



### DOM CLICK VIDEO PLAY TRACKING - stopfever 2024-07-11:
`<script>
function addClickEventListeners() {
document.querySelectorAll('[id^="play_"]').forEach(function(playButton) {
playButton.addEventListener('click', function(e) {
console.log("gtm_js_debug = Play button clicked:", playButton);
var playId = playButton.getAttribute('id');
dataLayer.push({
event': 'video_play',
eventCategory': 'Video',
click_video': playId
});
console.log("gtm_js_debug = DataLayer event pushed", {
event': 'video_play',
eventCategory': 'Video',
click_video': playId
});
});
});
}

// Запуск функции после загрузки DOM
addClickEventListeners();
</script>`
---

### отправка формы с отслеживанием ее полей:
#### Contact Form pages

`<script>
document.querySelectorAll('form').forEach(function(form) {
console.log("gtm_js_debug = Form found");

form.addEventListener('submit', function(event) {
console.log("gtm_js_debug = Form submit event triggered");

// get form values
var email = form.querySelector('#email-yui_3_17_2_1_1550475430662_7366-field') ? form.querySelector('#email-yui_3_17_2_1_1550475430662_7366-field').value : '';
var nameField1 = form.querySelector('#text-yui_3_17_2_1_1551402143999_115623-field');
var nameField2 = form.querySelector('#text-yui_3_17_2_1_1550549034643_750405-field');
var name = nameField1 ? nameField1.value : (nameField2 ? nameField2.value : '');
var formId = form.getAttribute('id') || 'undefined';

console.log("gtm_js_debug = Email: " + email);
console.log("gtm_js_debug = Name: " + name);
console.log("gtm_js_debug = Form ID: " + formId);

// save values to sessionStorage
sessionStorage.setItem('formEmail', email);
sessionStorage.setItem('formName', name);
sessionStorage.setItem('formId', formId);


// save values to GTM
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
event': 'form_send_click',
formEmail': email,
formName': name,
formId': formId
});

});
});
</script>`
---

### DL - Form Submitted
`<script>
console.log("gtm_js_debug = Thank You page loaded");

// Извлеките данные из sessionStorage
var formEmail = sessionStorage.getItem('formEmail');
var formName = sessionStorage.getItem('formName');
var formId = sessionStorage.getItem('formId');

console.log("gtm_js_debug = Retrieved formEmail from sessionStorage: " + formEmail);
console.log("gtm_js_debug = Retrieved formName from sessionStorage: " + formName);
console.log("gtm_js_debug = Retrieved formId from sessionStorage: " + formId);
---
// Отправьте данные в GTM
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
event': 'form_sent_successfully',
formEmail': formEmail,
formName': formName,
formId': formId
});

  ---
console.log("gtm_js_debug = DataLayer event pushed on Thank You page", {
event': 'form_sent_successfully',
formEmail': formEmail,
formName': formName,
formId': formId
});
</script>`

### Отслеживание popup с классами, начинающимися с **"popup-"** (например popup-linex и popup-forte)
`.popup[class*="popup-"]`
![image](https://github.com/user-attachments/assets/0483332b-cb44-4978-ba06-80328694fc37)



### отслеживание DOM некоторых вложенных классов:

![image](https://github.com/user-attachments/assets/81651986-361f-4f00-9762-2cbf6fc24df9)

`<script>
// Функция для отслеживания кликов по элементам .popup-goto внутри .popup.popup-linex и .popup.popup-forte
function trackPopupGotoClicks() {
    var popupElements = document.querySelectorAll('.popup.popup-linex .popup-goto, .popup.popup-forte .popup-goto');
    popupElements.forEach(function(popupGotoLink) {
        popupGotoLink.addEventListener('click', function () {
            window.dataLayer = window.dataLayer || [];
            window.dataLayer.push({
                'event': 'pharmacy_click',
                'eventCategory': 'Pharmacy Link Click'
            });
        });
    });
}

// Ждем полной загрузки страницы
document.addEventListener('DOMContentLoaded', trackPopupGotoClicks);
</script>`
