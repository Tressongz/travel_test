<template>
<div class="header">
  <div class="header_left">
    <a href="#" class="header_burger"><svg class="icon24"><use xlink:href="./img/sprite.svg#burger"></use></svg></a>
    <div class="header_logo active">
      <a class="logo" href="#">T<span>op</span> C<span>lient</span></a>
      <div class="side_toggle">
        <a href="#">
          <svg class="icon24">
            <use xlink:href="./img/sprite.svg#side_toggle"></use>
          </svg>
        </a>
      </div>
    </div>
    <div class="header_cent">
      <a href="#">Справочный Центр<span>Помощь</span></a>
    </div>
    <div class="header_tech">
      <a href="tel:88006008013">8 (800) 600 80 13</a>
      <a href="mailto:hd@clubwings.ru">hd@clubwings.ru</a>
    </div>
  </div>
  <div class="header_right">
    <div class="header_ico">
      <div class="header_currency">
        <a href="#"><svg class="icon24 icon_rus icon_bord"> <use xlink:href="./img/sprite.svg#ico_rub"></use></svg>RU</a>
        <a href="#"><svg class="icon24 icon_fill"> <use xlink:href="./img/sprite.svg#ico_usd"></use></svg>USD</a>
      </div>
      <div class="header_user_mess">
        <a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#user_mess"></use></svg></a>
      </div>
    </div>
    <div class="header_user">
      <a href="#"><span>Ольга Кузьма</span> <img alt="profile_img" src="img/profile_img.png" width="40" height="40"></a>
    </div>
  </div>
</div>
<div class="work_area">
  <div class="sidebar active">
    <div class="group_block">
      <div class="side_logo">
        <a href="#"><img alt="side_logo" src="img/side_logo.svg" width="50" height="50"></a>
        <div class="side_plane">
          <div class="plane_name">Travel plane</div>
          <div class="plane_price">540 000 ₽</div>
        </div>
      </div>
    </div>
    <div class="group_block">
      <ul class="side_nav">
        <li class="active"><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_home" /></svg>Главная</a></li>
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_service" /></svg>Услуги</a></li>
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_order" /></svg>Заказы</a></li>
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_stat" /></svg>Статистика</a></li>
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_company" /></svg>Компании</a></li>
      </ul>
    </div>
    <div class="group_block">
      <ul class="side_nav">
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_news" /></svg>Новости</a></li>
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_options" /></svg>Настройки</a></li>
      </ul>
    </div>
    <div class="group_block">
      <ul class="side_nav">
        <li><a href="#"><svg class="icon24"> <use xlink:href="./img/sprite.svg#ico_exit" /></svg>Выйти</a></li>
      </ul>
    </div>
  </div>
  <div class="wrapper">

    <div style="height: 900px;"></div>

    <div class="modal_side modal_right" style="display: block;">
      <div class="modal_side_wrap">
        <div class="modal_head modal_head_tarif">
          <h5>Правила тарифа</h5>
          <div class="tarif_switch tabs tabs-but">
            <a
                href="#"
                @click="changeFlight(item.PaxSegmentID)"
                v-for="item in json.DataLists.PaxSegmentList.PaxSegment"
                class="swit_tarif1"
                v-bind:class="{ 'active' : flight === item.PaxSegmentID}"
                :key="item.PaxSegmentID"
            >
              {{ item.Dep.IATA_LocationCode }} – {{ item.Arrival.IATA_LocationCode }}
            </a>
          </div>
          <div class="razdel_switch tabs tabs-but">
            <a href="#raz15" class="swit_razdel1 active">Раздел 15</a>
            <a href="#raz16" class="swit_razdel2">Раздел 16</a>
            <a href="#raz21" class="swit_razdel3">Раздел 21</a>
            <a href="#raz22" class="swit_razdel4">Раздел 22</a>
          </div>
          <a href="#" class="btn btn-icon btn-blue_fill modal_close">
            <svg class="icon18"><use xlink:href="./img/sprite.svg#ico_close"></use></svg>
          </a>
        </div>
        <div class="rules_wrap">
          <div
              v-html="newRules"
              class="rules_item"
              style="text-align: justify"
          >
          </div>
        </div>
      </div>
    </div>
    <div class="lay" style="display: block"></div>
  </div>
</div>
</template>

<script setup>
import {onBeforeMount, onMounted} from "vue";
import json from '../data/rules.json'
import {computed, ref, watch} from "vue";

const rules = ref('')
const flight = ref('dff5eac8-1d7d-4d68-bab3-d39e7884a0b2')
const newRules = ref('')
function changeFlight(id) {
  rules.value = json.Rules.find(item => id === item.PaxSegmentRefID).FareRuleText.Remark.RemarkText
  flight.value = id
}

watch(rules, (newRule) => {
  let raz15 = `<b>15. Ограничения по продаже<\/b>`
  let raz16 = `<b>16. Сборы при расторжении (изменении условий) договора перевозки<\/b>`
  let raz21 = `<b>21. Скидки для агентов<\/b>`
  let raz22 = `<b>22. Другие скидки</b>`
  newRule = rules.value.slice(0, rules.value.indexOf(raz15)) + `<span id="raz15"> ${raz15} </span>` + rules.value.slice(rules.value.indexOf(raz15) + raz15.length, rules.value.indexOf(raz16)) +`<span id="raz16"> ${raz16} </span>` + rules.value.slice(rules.value.indexOf(raz16) + raz16.length, rules.value.indexOf(raz21)) +`<span id="raz21"> ${raz21} </span>` + rules.value.slice(rules.value.indexOf(raz21) + raz21.length, rules.value.indexOf(raz22)) +`<span id="raz22"> ${raz22} </span>` + rules.value.slice(rules.value.indexOf(raz22) + raz22.length)
  newRules.value = newRule
})

onMounted(() => {
  rules.value = json.Rules[0].FareRuleText.Remark.RemarkText
})
</script>