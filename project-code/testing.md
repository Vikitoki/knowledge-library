# Тестирование

## Это факты, разработчик!

1. Тесты — это не долго, не дорого. Это хороший способ экономить на средней дистанции в сколь нибудь большом и среднем проекте. Ваш проект живёт больше трёх месяцев? Вы понимаете, что вам становится сложно ментально держать его модель в голове? Из отдела QA слышны ругательства по поводу значительного увеличения времени на ручное тестирование? Значит пошли первые запахи и пора освежиться тестами!

2. Мы не избавляемся от ручного тестирования – компьютеры это всего лишь (пока что) машины. Они могут гарантировать, что конкретные узлы системы (или их совокупность) находятся в рабочем состоянии. Но то, что работа всей системы правильно отображает требования заказчика, может проверить только человек.

3. Возможно тесты вам не нужны когда:

   - Вы проверяете небольшую бизнес логику (на коленочке), которая может и не залететь в продакшен
   - Ваша девушка кричит: "Дорогой, я в стартапе"
   - Ваш кусок фронтенда является ни чем иным, как "первой дозой бесплатно", а вся фишка приложения - огромный, как мой репозиторий, API, выполняющий основную работу.

## E2E или сказка о потерянном времени

Мальчики и девочки любят E2E тесты. Все любят E2E тесты. Заполнил формочку, нажал галочку, получил результат. Однако такой вид тестов отражает максимально внешнее поведение системы, аналогично пользовательскому опыту. Разница лишь в том, что ответ приходит быстрее. И меньше разочарованных клиентов стучится к вам в поддержку. Поэтому когда ваш ответ на действия в формочке (зелёная галочка) сменится на что-то другое (зелёная какашка), вам останется пожелать удачи для установления, а что же точно сломало эту систему. Ну а бизнесу можно сразу соболезновать - ведь если сломалась, например, формочка для принятия платежей, то... уф, даже страшно представить!

Но если результаты работы E2E тестов быстрее чем пользовательский опыт - это не означает, что эти типы тестов в принципе быстрые. Средний тест, проверяющий работу простой формочки логинизации, составляет от 5 секунд (в среднем). А теперь представим, что у нас 1000 таких тестов. Интересно, как быстро вы начнёте ненавидеть девопсов, когда такие тесты, будут запускаться на каждый чих в мастер (спойлер: быстрее чем ваш кофе остынет).

А ведь чем меньше тесты запускаются, тем меньше ценности они имеют! Отсюда правило - E2E пишут только на ключевое поведение системы и кол-во такого вида тестирования в проекте должно быть как можно меньше.

## Интеграционное тестирование. Ускоряемся!

Но как же нам быть, если мы хотим понять, что происходит и почему всё пошло по пи... ну вобщем, не туда, куда следовало. На помощь спешат интеграционные тесты. Их философия понятна даже ребёнку! Пирамидка состоит из кирпичиков. Чтобы понять почему пирамидка рухнула, нужно посмотреть как связаны между собой кирпичики. Говоря по-взрослому, такой вид тестирования позволяет связать несколько узлов в небольшую систему и протестировать их взаимодействие. Если тест пройден, мы можем быть с вами уверены, что наша маленькая пирамидка работает так, как задумано. В чём же премущество по-сравнению с E2E?

Теперь мы получаем супер быстрый отклик о том, что наши узлы взаимодействуют так как нужно. Машина может прогонять эти тесты в большом количистве и уведомлять нас если что-то пошло не так в течении нескольких секунд! Победа? Ну за малым. Ведь если скажем, мы отыскали кусок пирамидки, из-за которого произошло обрушение, то как понять какой же всё-таки из кирпичиков стал непригодным? На помощь приходит unit-тестирование!

## Unit-тесты. Туда, сюда, обратно

Наконец-то мы можем осуществлять контроль качества на самом его нижнем слое! Контроль того как, из чего сделан наш кирпичик - определит стабильность нашей системы. Благодаря данному типу тестирования мы можем наблюдать за внешним поведением каждого отдельного компонента и делать заключения о качестве его реализации. Звучит круто и вкусо.

У Unit-тестов есть две важные особенности. Первая - это скорость и простота написания таких тестов! Как же приятно обмазываться шаблонами и чувствовать как растёт уверенность в твоей работе - незабываемое ощущение. А второе - покрытие. В мире тестирования существуют метрики, которые, как правило, предоставляются библиотеками. Вы воочию можете налюдать за тем как встаёт солнце, как горит огонь и как растёт количество протестированных компонентов на вашем проекте.

Но вот, в один прекрасный момент вам дарят новый конструктор, где кирпичики имеют разные отверстия непонятной формы. В попытках, вы пытаетесь собрать из них что-то стоящее, но слишком скоро приходит подруга-разочарование. Вот бы кто-нибудь дал вам инстуркцию с подробным описанием, что и в какое отверстие надо... вставить (перед попыткой пошутить на эту тему, пожалуйста, проконсультируйтесь со специалистом). И о чудо, она существует - это типизация!

## Типизация. "Гляди Пяточёк...Входит...Ой. И выходит!"

Данный инструмент позволяет нам наглядно отслеживать, что условный компонент принимает на вход и что получается на выходе. Вещи, которые идут на вход чаще всего называют пропсами (props). В различных языках програмирования для этого используются специальные сущности. Имя им интерфейсы. У нашего любимого JavaScript было тяжёлое детство, да и сам язык является динамически типизируемым. Поэтому реализацию интерфейсов не завезли. И не завезут. Зато языки, которые компилируются в JavaScript предоставляют нам такую функциональность. Самым ярким и мейнстримным из них является TypeScript. Вот как выглядит описание такого интерфейса:

```javascript

type TWomanComponentProps = {
   knowHowToCook: boolean
}

// 5. Компонент
export const WomanComponent: React.FC<TWifeComponentProps> = ({ knowHowToCook }) => {
   if(knowHowToCook) {
      return (
              <p>
                 Выходи родная за меня<br>
                 Буду сыт и счатслив я всегда<br>
                 За твой борщ отдам я всё на свете<br>
                 За котлеты, курочку, спагетти
              </p>
   )
   } else {
         return (
                 <p>
                    Я на тебе никогда не женюсь<br>
                    Я лучше съем перед загсом свой паспорт<br>
                    ...
                 </p>
      )
      }
      }

```

На улице крики! Производители кирпичиков собрались на стихийный митинг и скандируют:
"Зачем нам ваша типизация, когда мы можем писать unit-тесты и убеждаться в том, что пропсы соответствуют действительности".
Казалось бы, правда на их стороне! Но как говорится: любой код - плохой код. Если мы можем не плодить сущности, без надобности,
то так и стоит поступать (не всегда это правда - контръинтуитивные вещи иногда делают шаг вперёд -
React с функциональными компонентами тому доказательство)! Но в нашем случае всё строго и логично, поэтому:

```html
<p>
  Типизация нужна<br />
  Типизация важна<br />
  Каждый должен быть на своём месте!<br />
  <br />
  Написал unit-тестик на пропсы<br />
  Всё зря!<br />
  Использовал типизацию<br />
  Стало в разы прелестней!<br />
</p>
```

## Unit-тесты. Ценности и задачи.

Ну что возвращаемся к нашим менеджеру. А лучше повыше - к бизнесу. Бизнес приходит и ставит требование.
Менеджер воодушевлённый новыми идеями приходит к нам и излагает суть бизнес требования.
Как профессионалы мы конечно же декомпозируем задачу.
Простыми словами: чтобы задумка работала, нам надо внести изменения 1 в этот компонент, изменения 2 в этот компонент и далее по цепочке.
Какую же ценность привносят unit-тесты на этом этапе:

1. Тесты позволяют нам сказать, что наши бизнес требования были выполнены корректно с точки зрения функциональности и отображения.

2. Тесты "связывают" требования и написанный нами код, раскрывая идеи, которые мы превнесли в архитектуру модуля, и позволяют новым
   разработчикам понять что же всё-таки делает данный узел. Если на вашем проекте имеются unit-тесты и вам выпала "не лёгкая", связанная с
   рефакторингом кода, то хороший подход для старта - открывайте тесты, закреплённые к конкретному модулю и читайте их.

3. Тесты помогают нам более трезво взглянуть, по оканчанию, на нашу работу и ответить себе на фундаментальный вопрос,
   который задаёт себе каждый разработчик: "А не фигню ли я написал?". Ведь наша "декомпозиция головного мозга"
   (требование 1, требование 2, ... требование n) чаще всего формируется у нас в голове бесзознательно и потоково.
   Именно поэтому тесты привносят в наш код ключевую инженерную составляющую - ретроспективу.

4. Тесты позволяют напрямую задокументировать поведение сущностей модуля, а также его наиболее важное или неочивидное (или то и другое)
   поведение, оставив этот "отпечаток" в вечности!

На улице снова слышны крики! На это раз здесь собрались разработчики-старообрядцы, которые настроены очень скептически.
Они кричат: "Ря, зачем нам изучать инженерную технологию unit-тестирования, когда мы можем написать документацию
и просто заполнять её по мере продвижения проекта, ря". На что мы зададим простой вопрос, который заставит разойтись наиболее умных из них
с местной болотной площади: "Как вы собираетесь гарантировать, что поведение описанное словами в документации соответствует поведению в модулях проекта".

Проект растёт, разработчик получает всё больше зависимостей.
Внеся изменения в словесную документацию разработчик рискует неявно интерпретировать бизнес ценности внутри модуля , и что самое страшное
даже не заметить этого. При пропорциональном росте времени мы получаем такой же рост
  непредумышленных убийств логики документации. А значит не далёк момент, когда к вам с красными глазами в личку постучится тимлид и скажет:
  "Андрюха, у нас баги по всему проекту, возможно криминал, по коням".
  Поздравляем! По достижении этого момента, вы получаете гарантированный подарок - поталогический страх к любому рефакторингу кода.

Делаем вывод - писать unit-тесты важно и необходимо. Ценность тестов вырастет, если вы будете писать их неотрывно от задачи
_(конечно же предварительно попросив продукт менеджера выделить на это время)_
Это обязательно снизит вашу ментальную нагрузку (вам не придётся возвращаться к этому через месяц/два/год/никогда)
и позволит, как можно раньше, сделать акцент на неочевидных вещах, которые могут быть забыты/уничтожены/перекочевать куда-то ещё.
