## Ссылки на полезные материалы

Официальный Vue.js style guide: 
[https://ru.vuejs.org/v2/style-guide/index.html]

Видеодоклад с доступным объяснением цикла событий JS и асинхронности 
[https://www.youtube.com/watch?v=8cV4ZvHXQL4]

## -1. Знания, рожденные многовековым опытом

1. Не пишем инлайн стили
2. Не пишем js-логику в шаблоне разметки
3. Разделяем логические блоки одной пустой строкой (блок инициализации переменных,
 блок исполнения, блок условного исполнения и т.д.)
4. Не пишем !important без крайней необходимости

## 0. Линтер
.eslintrc (в корне проекта)
```json
{
  "env": {
    "node": true
  },
  "parserOptions": {
    "parser": "babel-eslint"
  },
  "extends": [
    "@vue/airbnb",
    "plugin:vue/recommended"
  ],
  "rules": {
    "no-param-reassign": "off",
    "no-multi-spaces": "off",
    "vue/no-multi-spaces": "off",
    "vue/max-attributes-per-line": 3,
    "import/no-unresolved": {
      "node": {
        "paths": ["@"]
      }
    }
  }
}
```

settings.json (для VS Code) должен включать:
```json
"eslint.validate": [
  "javascript",
  "javascriptreact",
  {
    "language": "vue",
    "autoFix": false
  }
]
```

## 1. Файловая структура фронтенда
Здесь описание файловой структуры фронта

## 2. Структура кодовой базы компонента
Здесь стараемся угодить линтеру. Порядок расположения блоков должен быть следующим:
```
/* base libraries */
import axios from 'axios';
import { mapGetters, mapActions } from 'vuex';

/* constants */
import { defaultErrorMessage } from '@/constants';

/* mixins */
import systems from '@/mixins/systems';
import filters from '@/mixins/filters';
import rules from '@/mixins/rules';

/* custom classes */
import Validator from '@/utils/Validator';

/* child components */
import ManageRulesDialog from '@/components/groups/viewGroup/ManageRulesDialog.vue';
import AddFiltersDialog from '@/components/groups/viewGroup/AddFiltersDialog.vue';
import ViewFiltersDialog from '@/components/groups/viewGroup/ViewFiltersDialog.vue';
import ManageAttributesDialog from '@/components/groups/viewGroup/ManageAttributesDialog.vue';
import RemoveGroupButton from '@/components/groups/RemoveGroupButton.vue';

/* layout components */
import BlockDivider from '@/components/common/BlockDivider.vue';
import AppFooter from '@/layout/Footer.vue';

export default {
  components: {
    ...
  },
  filters: {
    ...
  },
  mixins: [
    ...
  ],
  props: {
    ...
  },
  data() {
    return {
      ...
    };
  },
  computed: {
    ...
  },
  watch: {
    ...
  },
  beforeCreate() {
    ...
  },
  created() {
    ...
  },
  beforeMount() {
    ...
  },
  mounted() {
    ...
  },
  beforeUpdate() {
    ...
  },
  updated() {
    ...
  },
  beforeDestroy() {
    ...
  },
  destroyed() {
    ...
  },
  methods: {
    ...
  },
}
```
## 3. Именование
Основной стиль именования каких-либо сущностей - CamelCase.
Экземпляров - camelCase.


### 3.1 Переменные
Имена переменных должны быть существительными, начинающимися с маленькой буквы. 
Название переменной должно ясно сообщать о ее предназначении. 
Однако, не следует забывать и о лаконичности.

Плохой пример:
```
fetchedData,
fetchedFromServerData,
fetchedFromServerGroups,
state
```

Хороший пример:
```
fetchedGroups,
dialog: {
  state: false,
}
```
Относящиеся к одной сущности переменные рекомендуется помещать в один объект.

Плохой пример:
```
isLoading: false,
model: '',
items: [],
...
```
Хороший пример:
```
programType: {
  isLoading: false,
  model: '',
  items: [],
},
...
```
Пренебречь правилом можно, если сущность описана в отдельном компоненте, 
где все данные относятся исключительно к ней.

### 3.2 Вычисляемые свойства (computed)
Правила именования вычисляемых свойств аналогичны правилам именования переменных.

### 3.3 Методы (methods)
Наименования методов и прочих функций должны начинаться с глагола и описывать выполняемое действие.

Плохой пример:
```
groups() {}, // не начинается с глагола => не отождествляется с совершением действия
fetchServerData() {}, // плохо описывает предназначение
lettersOrder() {}, // не начинается с глагола => не отождествляется с совершением действия
calculate() {}, // плохо описывает предназначение
```
Хороший пример:
```
fetchStudentGroups() {},
randomizeLettersOrder() {},
calculateFontSize() {},
```
3.4 Миксины (mixins)

3.5 Компоненты
Наименования компонентов должны начинаться с заглавной буквы и быть исполнены в CamelCase. Название компонента должно давать ясное представление о том, какую сущность он собой представляет.
Плохой пример:
```
make_group // snake_case
makeGroup // camelCase, но с маленькой буквы
MakeGroup // CamelCase, но не отождествляется с представляемой сущностью, хоть и описывает предоставляемый функционал
```
Хороший пример:
```
MakeGroupDialog
ConfirmDialog
QuestionItem
PickUsersDialog
```
Обращение к компонентам в шаблоне происходит следующим образом:
```
tooltip-text(
  :state="tooltipsStates['groupName']"
  :text="tooltipsText['groupName']"
  @close="tooltipsStates['groupName'] = false"
)
dialog-main-title(title="Управление правами")
group-filters-table(
  v-for="(item, key, index) in filters"
  :table-data="item"
  :entity-name="key"
  @item-remove="removeTableItem"
  @entity-remove="removeEntity"
)
```
