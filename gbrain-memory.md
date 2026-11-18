# GBrain للمشروع

ابدأ من الصفحة `projects/upwork-intercome`. دي نقطة الدخول لحالة مشروع دعم Shopify، أهم نتائج البحث، والأدلة اللي ترجع لها قبل ما تكمّل Discovery.

## فتح ذاكرة المشروع

شغّل الأوامر من جذر المشروع في PowerShell. `GBRAIN_HOME` هنا بيتحدد للعملية الحالية وبيترد لقيمته بعد القراءة. مصدر `default` في الأوامر هو المصدر جوّه قاعدة المشروع `.gbrain/brain.pglite`. استخدم جلسة PowerShell خالية من `GBRAIN_DATABASE_URL` و`DATABASE_URL`، لأن إعدادات الاتصال دي ممكن تختار قاعدة تانية.

```powershell
$projectBrainPreviousHome = [Environment]::GetEnvironmentVariable('GBRAIN_HOME', 'Process')
$projectBrainCli = 'C:\Users\kerol\.bun\bin\gbrain.exe'
try {
    $env:GBRAIN_HOME = (Resolve-Path -LiteralPath '.').Path
    & $projectBrainCli get 'projects/upwork-intercome' --source-id default --include-content --content-only --json
    & $projectBrainCli get 'projects/upwork-intercome/state' --source-id default --include-content --content-only --json
    & $projectBrainCli recall 'projects/upwork-intercome' --source default --json
} finally {
    [Environment]::SetEnvironmentVariable('GBRAIN_HOME', $projectBrainPreviousHome, 'Process')
}
```

داخل نفس كتلة `try`، استخدم `& $projectBrainCli context-pack --entities 'projects/upwork-intercome' --source-id default --budget-tokens 2600 --json` لو محتاج سياق مختصر لإيجنت جديد. للبحث عن نتيجة بعينها، استخدم `& $projectBrainCli search 'CWILL' --source-id default --match keyword --json`.

## إيه اللي هتلاقيه

| صفحة GBrain | المحتوى |
| --- | --- |
| `projects/upwork-intercome` | نقطة الدخول وفهرس المرجعيات. |
| `projects/upwork-intercome/state` | مرحلة المشروع، المطلوب من العميل، القرارات المفتوحة، وأدلة الاستكمال. |
| `projects/upwork-intercome/findings` | أهم ملاحظات المنافسين وVoice of Customer، مع المصادر وحدود التحقق. |
| `projects/upwork-intercome/opportunity-solution-tree` | هدف قياس مقترح، ترتيب المشاكل، التجارب المقترحة، وإيه اللي مستني تأكيد. |

للرجوع للنص الكامل، راجع [إعلان العميل](<job description.txt>)، [المتطلبات والأسئلة](requirements-and-questions.md)، [بحث المنافسين](competitive-research.md)، [Voice of Customer](voice-of-customer-snapshot.md)، و[Opportunity Solution Tree](opportunity-solution-tree.md).

## تسجيل الشغل الجديد

- لما يتغيّر وضع المشروع أو يطلع دليل مهم، حدّث الصفحة المعنية بالقرار أو النتيجة، تاريخها، مرجعها، وحالتها. حافظ على التمييز بين اللي العميل قاله، اللي اتلاحظ في السوق، والفرضيات اللي لسه محتاجة إثبات.
- لو هتعدّل صفحة، اقرأ المحتوى ورقم المراجعة بـ `get <slug> --source-id default --include-content --content-only --json`. مرّر Markdown الصفحة كاملة للـCLI بـ `put <slug> --source-id default --expected-revision <revision>` من داخل نفس `GBRAIN_HOME`. أكّد إن الحفظ تم واقرا الصفحة تاني.
- للحقائق الصغيرة القابلة للاستدعاء، استخدم `gbrain remember` مع `--entity projects/upwork-intercome` ومرجع الدليل في `--provenance`. استخدم `gbrain recall` للتحقق من الحفظ. خلي نص المحادثات وبيانات العملاء وأسرار الحسابات في مصادرها المصرّح بيها.

الذاكرة محلية على نفس الجهاز. التسجيل يدوي والبحث بالكلمات. مجلد `.gbrain` مستبعد من Git. اتصال GBrain MCP المشترك بيقرأ القاعدة المشتركة، واستخدام الأوامر هنا بيفتح قاعدة المشروع.
