# Помпа Accu-Chek Insight

**Это программное обеспечение - часть алгоритма самостоятельно настраиваемой ИПЖ; она не является коммерческим продуктом, но требует, чтобы вы прочитали, узнали и поняли, как ей пользоваться. Она не будет за вас контролировать диабет, но позволит улучшить компенсацию и качество жизни, при условии, что вы готовы уделить ей достаточно внимания и времени. Не бросайтесь в систему сломя голову, дайте себе время на изучение. Только вы несете ответственность за то, что делаете.**

* * *

## ***ВНИМАНИЕ:** Если вы до настоящего момента пользовались помпой Insight с пультом **SightRemote**, пожалуйста **обновите прошивку помпы до новейшей версии ** и ** деинсталлируйте SightRemote**.*

## Требования к аппаратному и программному обеспечению

* Помпа Roche Accu-Chek Insight (любая версия прошивки, работают все)

Примечание: AAPS всегда записывает данные в **первый профиль скорости базала в помпе **.

* An Android phone (Basically every Android version would work with Insight, but check on the [Module](module-phone) page which Android version is required to run AAPS.)
* The AAPS app installed on your phone

## Настройки

* Помпа Insight должна быть единовременно подключена только к одному устройству. Если вы ранее уже подсоединяли пульт дистанционного управления (глюкометр) Insight, то его необходимо удалить из списка сопряженных устройств помпы: Меню > Настройки > Связь > Удалить устройство
    
    ![Снимок экрана удаления глюкометра из помпы Insight](../images/Insight_RemoveMeter.png)

* In [Config builder](../Configuration/Config-Builder) of the AAPS app select Accu-Chek Insight in the pump section
    
    ![Снимок экрана конфигуратора Config Builder помпы Insight](../images/Insight_ConfigBuilder_AAPS3_0.jpg)

* Нажмите на значок шестеренки, чтобы открыть настройки Insight.

* В настройках помпы нажмите кнопку "Сопряжение Insight" в верхней части экрана. Вы увидете список устройств bluetooth поблизости (ниже слева).
* На помпе Insight перейдите в меню > Настройки > Связь > Добавить устройство. На помпе появится экран (внизу справа) с указанием серийного номера помпы.
    
    ![Снимок экрана сопряжения Insight 1](../images/Insight_Pairing1.png)

* Вернувшись к телефону, нажмите на серийный номер помпы в списке устройств Bluetooth. Затем нажмите на Сопряжение, чтобы подтвердить действие.
    
    ![Снимок экрана сопряжения Insight 2](../images/Insight_Pairing2.png)

* Как помпа, так и телефон отобразят код. Убедитесь, что коды одинаковы на обоих устройствах и подтвердите сопряжение на помпе и на телефоне.
    
    ![Снимок экрана сопряжения Insight 3](../images/Insight_Pairing3.png)

* Успешно! Pat yourself on the back for successfully pairing your pump with AAPS.
    
    ![Снимок экрана сопряжения Insight 4](../images/Insight_Pairing4.png)

* To check all is well, go back to Config builder in AAPS and tap on the cog-wheel by the Insight Pump to get into Insight settings, then tap on Insight Pairing and you will see some information about the pump:
    
    ![Снимок экрана информации о сопряжения Insight](../images/Insight_PairingInformation.png)

Примечание: Постоянного соединения между помпой и телефоном не будет. Подключение будет устанавливаться только в случае необходимости (т.е. для изменения временного базала, подачи болюса, чтения логов помпы и т.п...). В ином случае аккумуляторы телефона и помпы будут расходоваться слишком быстро.

(Accu-Chek-Insight-Pump-settings-in-aaps)=

## Настройки на AAPS

**Примечание: В настоящее время есть возможность (начиная с версии AAPS v2.7.) «Всегда использовать абсолютные значения базала», если вы намерены использовать Autotune с помпой Insight даже если 'Синхронизация включена' с Nightscout.** (В AAPS перейдите в [Настройки > NSClient > Расширенные настройки](Preferences-advanced-settings-nsclient)).

![Снимок экрана настроек Insight](../images/Insight_settings.png)

In the Insight settings in AAPS you can enable the following options:

* "Отслеживать замены картриджа": При выполнении команды "заполнение инфузионного набора" на помпе, это действие автоматически внесется в журнал как замена картриджа.

* "Log tube changes": This adds a note to the AAPS database when you run the "tube filling" program on the pump.

* "Log site change": This adds a note to the AAPS database when you run the "cannula filling" program on the pump. ** Примечание: Изменение места установки катетера также сбрасывает Autosens. **

* "Отслеживать замену батареи": При установке нового аккумулятора в помпе в базе данных Aaps добавляется соответствующая заметка.

* "Log operating mode changes": This inserts a note in the AAPS database whenever you start, stop or pause the pump.

* "Log alerts": This records a note in the AAPS database whenever the pump issues an alert (except reminders, bolus and TBR cancellation - those are not recorded).

* "Разрешить эмуляцию временного базала TBR": помпа Insight может давать только до 250% временной базальной скорости TBR. Чтобы обойти это ограничение, эмуляция временной базальной скорости TBR поручит помпе подать пролонгированный болюс с дополнительным инсулином если запрошенная временная скорость базала превышает 250%.
    
    **Рекомендуется применять только один растянутый болюс единовременно так как одновременное использование нескольких растянутых болюсов может вызвать ошибки.**

* "Отключить вибрации при ручной подаче болюса": Это отключает вибрации помпы Insight при ручной подаче простого или пролонгированного болюса. Этот параметр доступен только для последней версии встроенного ПО Insight (3.x).

* "Отключить вибрации при ручной подаче болюса": Это отключает вибрации помпы Insight при автоматической подаче микроболюса SMB или эмуляции временного базала. Этот параметр доступен только для последней версии встроенного ПО Insight (3.x).

* "Recovery duration": This defines how long AAPS will wait before trying again after a failed connection attempt. Можно выбрать от 0 до 20 секунд. Если вы испытываете проблемы с подключением, выберите более длительное время ожидания.   
      
    Пример мин. длительности восстановления = 5 и макс. длительности восстановления = 20   
      
    нет соединения -> подождать **5** сек.   
    повторите -> нет соединения -> подождать **6** сек.   
    повторите -> нет соединения -> подождать **7** сек.   
    повторите -> нет соединения -> подождать **8** сек.   
    ...   
    повторите -> нет соединения -> подождать **20** сек.   
    повторите -> нет соединения -> подождать **20** сек.   
    ...

* "Disconnect delay": This defines how long (in seconds) AAPS will wait to disconnect from the pump after an operation is finished. Значение по умолчанию - 5 секунд.

For periods when pump was stopped AAPS will log a temp. basal rate with 0%.

In AAPS, the Accu-Chek Insight tab shows the current status of the pump and has two buttons:

* "Обновить": Обновляет состояние помпы
* "Включить/выключить уведомление TBR": Стандартная помпа Insight издает сигнал по завершении временной TBR. Кнопка позволяет включить или отключить это оповещение без изменения конфигурации программного обеспечения.
    
    ![Снимок экрана текущего состояния Insight](../images/Insight_Status2.png)

## Настройки помпы

Настройте сигналы в помпе следующим образом:

* Меню > Настройки > Параметры устройства > Параметры режима > Тихий > Сигнал > Звук
* Меню > Настройки > Параметры устройства > Параметры режима > Тихий > Сигнал > 0 (удалить все полоски)
* Меню > настройка режимов > режим сигнала > тихий

This will silence all alarms from the pump, allowing AAPS to decide if an alarm is relevant to you. If AAPS does not acknowledge an alarm, its volume will increase (first beep, then vibration).

(Accu-Chek-Insight-Pump-vibration)=

### Вибрация

Depending on the firmware version of your pump, the Insight will vibrate briefly every time a bolus is delivered (for example, when AAPS issues an SMB or TBR emulation delivers an extended bolus).

* Прошивка 1.х: нет вибрации конструктивно.
* Прошивка 2.х: вибрация не может быть отключена.
* Firmware 3.x: AAPS delivers bolus silently. (minimum [version 2.6.1.4](Releasenotes-version-2-6-1-4))

Версию прошивки можно найти через меню.

## Замена батареи

Срок службы батареи для Insight в замкнутом цикле составляет от 10 до 14 дней, максимум 20 days. Пользователи, сообщающие об этом, используют литиевые батареи Energizer.

В помпе Insight есть небольшой внутренний аккумулятор для поддержания таких важных функций, как часы, которые продолжают работать при замене батарей помпы. Если смена батареи занимает слишком много времени, то в этой внутренней батарее может кончиться заряд, время на часах будет сброшено, и вам будет предложено ввести новое время и дату после установки новой батарейки. If this happens, all entries in AAPS prior to the battery change will no longer be included in calculations as the correct time cannot be identified properly.

(Accu-Chek-Insight-Pump-insight-specific-errors)=

## Специфические ошибки помпы Insight

### Пролонгированный болюс

Рекомендуется применять только один растянутый болюс единовременно так как одновременное использование нескольких растянутых болюсов может вызвать ошибки.

### Time out

Иногда помпа Insight может не отвечать во время установки соединения. В этом случае AAPS выдает следующее сообщение: "Таймаут сопряжения - выполните сброс bluetooth".

![Сброс Bluetooth помпы Insight](../images/Insight_ResetBT.png)

В этом случае выключите Bluetooth на помпе и смартфоне примерно на 10 секунд, а затем включите его обратно.

## Пересечение часовых поясов с помпой Insight

Информацию о пересечении часовых поясов см. в разделе [Пересечение часовых поясов с помпами](Timezone-traveling-insight).