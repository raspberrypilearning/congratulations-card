## قم بتنشيط مفرقعات الحفلة

<div style="display: flex; flex-wrap: wrap">
<div style="flex-basis: 200px; flex-grow: 1; margin-right: 15px;">
يجب أن تكون قادرًا على تنشيط مفرقعات الحفلة من Raspberry Pi Pico. في هذه الخطوة ، ستقوم بعمل نموذج أولي لمفتاحك باستخدام أسلاك التوصيل. 
</div>
<div>
! [صورة تظهر مشروع مفرقعات للحفلة بمفتاح مصنوع من زوج من الأسلاك.] (images / switch-buzzer-led.jpg) {: width = "300px"}
</div>
</div>

<p style='border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;'>
<span style="color: #0faeb0">عروض ضوئية وصوتية</span> تستخدم التكنولوجيا في الاحتفالات في جميع أنحاء العالم. تعمل هذه الخيارات ** المستدامة ** و ** القابلة لإعادة الاستخدام ** على إنشاء عروض ممتعة ووسائل ترفيه تفاعلية. الآن ، بدلاً من العناصر التي يمكن التخلص منها مثل المفرقعات البلاستيكية أو الألعاب النارية الكيميائية، يحتفل الناس بالطائرات بدون طيار والليزر وعروض الإسقاط!
</p>

--- task ---

احصل على **سلك توصيل ثنائي المقبس - دبوس** لاستخدامه في مفتاح السحب.

**قم بتوصيل** سلك توصيل واحد بـ **GP18** وواحد إلى **دبوس GND** المجاور له.

![مخطط الأسلاك يوضح سلك توصيل متصل بـ GP18 وسلك توصيل آخر متصل بـ GND.](images/jumper-switch.png)

--- /task ---

<p style='border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;'>هناك طريقتان يمكنك من خلالهما تشغيل التعليمات البرمجية بناءً على حالة الإدخال (مثل مفتاح أو جهاز استشعار). الأول هو استخدام حلقة ومواصلة التحقق من الحالة ، وهذا ما يسمى <span style="color: #0faeb0">الاقتراع</span>. لقد استخدمت الاقتراع في مشروع LED اليراع. والثاني هو استدعاء دالة عندما تتغير حالة الإدخال ، باستخدام <span style="color: #0faeb0">أحداث</span> التي تكتشف التغييرات عند حدوثها. 
</p>

--- task ---

قم بتغيير الكود الخاص بك لإخبار `picozero` باستدعاء وظيفة `pop` كلما تم فتح مفتاح السحب (غير متصل).

عند استخدام حدث مثل `when_opened`، ستعمل الوظيفة حتى تكتمل ولن تتمكن من مقاطعتها. هذا ما تريده في هذه الحالة ، فأنت تريد أن يحدث تأثير الصوت بالكامل وتأثير تغيير اللون عند تنشيط مفرقعات الحفلة.

**تذكر** أنك ستحتاج أيضًا إلى استيراد `Switch` من `picozero` على السطر 1.

--- code ---
---
language: python filename: party_popper.py line_numbers: true line_number_start: 1
line_highlights: 1, 5, 19
---
from picozero import RGBLED, Speaker, Switch from time import sleep

rgb = RGBLED(red=1, green=2, blue=3) # Pin numbers pull = Switch(18) speaker = Speaker(5)

def pop(): print("Pop") # Print to the shell rgb.color = (255, 0, 255) # Purple speaker.play(523, 0.1) # 523 = note C4, 0.1 seconds rgb.color = (0, 0, 0) # LED no colour – off sleep(0.1) rgb.color = (255, 0, 255) # Purple speaker.play(523, 0.6) # Note C4, 0.6 seconds rgb.off()

pull.when_opened = pop # The pop function will be called when the pull switch is opened

--- /code ---

**نصيحة:** تأكد من أنك **لا** تضيف `()` إلى نهاية `pull.when_opened = pop`. يخبر هذا السطر `picozero` أنه في كل مرة يحدث فيها الحدث `when_opened` ، يتم استدعاء وظيفة `pop`.

--- /task ---

--- task ---

**اختبار**: قم بتشغيل الكود الخاص بك للتأكد من أضواء RGB LED وتشغيل الصوت في كل مرة يكون فيها المفتاح **مفتوحًا**.

**تصحيح**:

--- collapse ---

---
title: أرى الرسالة `لم يتم تعريف المفتاح`
---

أضف `، المفتاح` إلى نهاية السطر 1.

--- /collapse ---

--- collapse ---

---
title: يتم تشغيل التعليمات البرمجية قبل ان اشغل المفتاح
---

+ تحقق للتأكد من توصيل اسلاك مفتاح السحب بالمسامير الصحيحة
+ تحقق للتأكد من أن كبلات مفتاح السحب لديك متصلة بشكل جيد مع بعضها البعض
+ تأكد من أنك قد أزلت السطر `pop ()` واستبدله بـ `pull.when_opened = pop`

--- /collapse ---

--- collapse ---

---
title: لا تظهر الرسالة "Pop" في الغلاف
---

تحقق من وحدة تحكم Thonny بحثًا عن أي رسائل خطأ وقم بإصلاح التعليمات البرمجية الخاصة بك بحيث تبدو تمامًا مثل المثال.

--- /collapse ---

--- collapse ---

---
العنوان: توقف RGB LED أو الجرس عن العمل
---

+ تحقق من أن الأسلاك الصحيحة متصلة بالمسامير الصحيحة
+ تحقق من عدم وجود أي قطع في الاتصالات
+ تحقق من أن مؤشر LED لم ينفجر

--- /collapse ---

--- /task ---