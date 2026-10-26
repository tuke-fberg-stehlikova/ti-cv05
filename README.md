# cv05: PHP a formuláre, bezpečnosť

Stránka týždňa s teóriou a ukážkami: https://beata.stehlikova.website.tuke.sk/ti/06.html

## Odovzdávaš

V repository *cv05* na GitHub a vo svojom webovom priestore na sigma (`ti/cv05`):

- celý web z cv04 v `web/`,
- `web/content/register.php` s `method="post"` a samospracovaním (kontroly, hodnoty späť, hlášky pri fields, zhrnutie),
- `practice/process.php` (výpis `$_POST`, kontroly, výpočty, ošetrené výpisy),
- v `README.md` dve vety o XSS teste (úloha 6.7).

## Hodnotenie

1 bod. Form je spracovaný cez POST, `practice/process.php` má kontroly a výpočty, `web/content/register.php` sa spracuje sám s ošetrenými výpismi. **Uzávierka 2** je koniec tohto týždňa. Commit a push najneskôr do nasledujúceho cvičenia. Po každom push sa spustia automatické testy; výsledok nájdeš podľa návodu [04 Odovzdávanie](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), časť 6.

## Prompty pre AI

Na miestach `// DOPLŇ` a `[DOPLŇ …]` dopĺňaš sám.

### Ukážka 7: prompt na process.php

```text
Mám form v súbore content/register.php (posielam ho nižšie) na tému [DOPLŇ tému].
Vytvor súbor practice/process.php pre PHP 8.1, ktorý ho spracuje. Form mu posiela údaje
metódou POST na adresu ../practice/process.php. Bez databázy, bez JavaScriptu, bez funkcií.

Štruktúra:
1. Hore v bloku <?php ... ?>: každú hodnotu z $_POST načítaj do variable
   s anglickým názvom podľa name field, s trim a s ?? '' (pre skupinu checkboxov ?? []);
   čísla preveď cez (int). Pri každom riadku komentár, čo robí.
2. Zoznamy možností select, radio a checkbox ako asociatívne array
   (key = value z HTML, value = text pre človeka), názvy po anglicky.
3. Kontroly do asociatívneho array $errors (key = názov field) cez if / elseif:
   povinné meno s mb_strlen aspoň 3, e-mail cez filter_var FILTER_VALIDATE_EMAIL,
   číslo v rozsahu. Ostatné fields nechaj s komentárom // DOPLŇ: kontrola.
4. HTML5 stránka s Bootstrap 5.3 z CDN, body s class container:
   heading, <pre> s print_r($_POST), výpis troch hodnôt po jednom,
   výpis všetkých hodnôt loop foreach do <dl> s is_array a implode,
   potom if: ak sú chyby, alert-danger so zoznamom chýb (key: text),
   inak alert-success.
5. Všetky hodnoty od používateľa vo výpise ošetri cez
   htmlspecialchars($x, ENT_QUOTES, 'UTF-8').

Ku každému bloku krátky komentár, čo robí.

Tu je form:
[VLOŽ content/register.php]
```

### Ukážka 8: prompt na výpočty

```text
V súbore process.php (posielam) mám po kontrolách vetvu else, kde sú údaje
v poriadku. Doplň do nej výpočty z údajov form, PHP 8.1, bez funkcií:
- z field s dátumom [DOPLŇ name field]: dátum vo formáte d. m. Y, anglický názov dňa
  cez date('l'), počet dní odo dneška cez strtotime a time() delené 86400 a ceil,
- z field s rokom [DOPLŇ name field]: počet rokov do dnes cez (int) date('Y'),
- dĺžka textu z field [DOPLŇ name field] cez mb_strlen,
- jeden ďalší výpočet nechaj ako komentár // DOPLŇ: vlastný výpočet z mojej témy.
Výsledky vypíš v paragraphs s <?= ?>. Pri každom výpočte komentár, čo počíta a v akých jednotkách.

Tu je súbor:
[VLOŽ process.php]
```

### Ukážka 9: prompt na samospracujúci form

```text
Posielam content/register.php (form) a process.php (kontroly) z webu na tému
[DOPLŇ tému]. Prepíš content/register.php na samospracujúci form, PHP 8.1,
bez funkcií, bez databázy. Súbor sa vkladá do template cez include, takže v ňom
nie je <html>, <head> ani menu, len obsah pre <main>.

1. Hore PHP blok: $errors = []; $sent = $_SERVER['REQUEST_METHOD'] == 'POST';
   načítanie všetkých fields z $_POST s trim a ?? ako v process.php; array možností
   skopíruj z process.php; kontroly spusti len if ($sent) a nechaj tam komentár
   // DOPLŇ: skopíruj kontroly z process.php.
2. Potom if ($sent && count($errors) == 0): section so zhrnutím v <dl>, hodnoty
   od používateľa cez htmlspecialchars($x, ENT_QUOTES, 'UTF-8'), hodnoty zo
   select/radio preložené cez array možností na text pre človeka; zaškrtnuté
   checkboxy prelož loop foreach do pomocného array a spoj cez implode.
3. else: section s form s method="post" a action="": nad form
   alert-danger, ak sú chyby; každé input má value s hodnotou späť cez
   htmlspecialchars, class is-invalid condition isset($errors['field'])
   a pod sebou div.invalid-feedback s textom chyby cez ?? '';
   <option>, radio a checkbox vypíš loop foreach z array možností
   so selected / checked condition; textarea s hodnotou medzi tags.
   Pri jednom field nechaj value a is-invalid nevyplnené s komentárom
   // DOPLŇ: hodnota späť a chyba.
4. Texty, class Bootstrap, label a id nemeň. Pri každom riadku PHP komentár.

Tu sú súbory:
[VLOŽ content/register.php a process.php]
```

## Úlohy

**Úloha 6.1: Príprava Kubeflow.** Priprav si Kubeflow podľa návodu [04 Odovzdávanie](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), časť 1: v Explorer je hore WEB, dole na stavovej lište je **Go Live**. Ak tam nie je, rozšírenia vráti tento príkaz v termináli:

```bash
code-server --install-extension Natizyskunk.sftp --install-extension yandeu.five-server --install-extension bmewburn.vscode-intelephense-client --install-extension mblode.twig-language-2
```

Potom `F1`, **Developer: Reload Window**. Nastavenie pripojenia na sigma zostáva.

**Úloha 6.2: Zadanie a kópia minulého týždňa.** Prijmi zadanie *cv05* v Classroom 50 (link je vyššie) a repository naklonuj do `~/web/ti/cv05` podľa návodu 04, časti 2 a 3. Potom do neho prekopíruj svoj web z minulého týždňa:

```bash
cd ~/web/ti
rsync -a --exclude .git --exclude .github --exclude README.md cv04/ cv05/
```

Pravý klik na `cv05`, **Upload Folder**. Over na sigma `…/ti/cv05/web/`, že web beží.

**Úloha 6.3: Form na POST.** V `web/content/register.php` over, že všetky fields majú anglické `name` a `value` bez diakritiky (skupina checkboxov má `[]`). Doplň `method="post"` a `action="../practice/process.php"`. Odošli form: adresa po odoslaní nemá `?` a stránka hlási, že `process.php` nie je. To je v poriadku.

**Úloha 6.4: process.php.** Prompt z Ukážky 7 so svojím form. Výsledok ulož ako `practice/process.php`. Vyplň form na sigma, odošli, prezri `print_r`. Odošli ho aj s nezaškrtnutým checkboxom a pozri, ktorý key chýba.

**Úloha 6.5: Kontroly rukou.** Doplň `// DOPLŇ: kontrola` pre všetky zvyšné fields: dátum cez `strtotime` (a že je v budúcnosti, ak to pre tvoju tému dáva zmysel), select a radio cez `in_array` s `array_keys`, súhlas, dĺžka správy. Každú kontrolu vyskúšaj: pošli zlú hodnotu a pozri hlášku.

**Úloha 6.6: Prehliadač nestačí.** F12, v `<input>` pre meno zmaž `required`, odošli prázdne. `process.php` chybu chytí. Ak nie, kontrola chýba.

**Úloha 6.7: XSS.** Do mena napíš `<script>alert('ahoj')</script>`, odošli. Ak vyskočí okno, nájdi v `process.php` výpis bez `htmlspecialchars` a oprav ho. Prejdi všetky výpisy hodnôt od používateľa, aj `$value` v loop. Do `README.md` v repository dopíš dve vety: čo sa stalo pred opravou a po nej.

**Úloha 6.8: Výpočty.** Prompt z Ukážky 8. Doplň `// DOPLŇ: vlastný výpočet` niečím zo svojej témy (dni od, roky do, počet znakov, percento…).

**Úloha 6.9: Samospracujúci form.** Prompt z Ukážky 9. Výsledok ulož ako `web/content/register.php`. Doplň `// DOPLŇ`: skopíruj kontroly z `process.php` a doplň chýbajúce field. Na sigma: otvor Registráciu z menu (prázdny form), odošli s chybou (hodnoty zostali, červené fields), odošli správne (zhrnutie). `process.php` v repository nechaj, je to cvičný súbor ako `variables.php`.

**Úloha 6.10: Chyba naschvál.** V `web/content/register.php` prepíš v jednom `$_POST['email']` key na `$_POST['emial']`, odošli form, prečítaj, čo sa stalo (hláška, alebo tichá chyba vďaka `??`). Oprav. Potom zmaž `method="post"`, odošli, pozri adresu. Oprav.

**Úloha 6.11: Commit a push.** Podľa návodu [04 Odovzdávanie](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), časť 5; commit po každej hotovej úlohe. Výsledok testov podľa časti 6.

**Úloha 6.12: Upload Folder a kontrola na sigma.** Pravý klik na `cv05`, **Upload Folder**. Over `web/index.php`, `web/routes.php`, `web/form.php` (prázdny, s chybou, správne), `web/contact.php` a `practice/process.php` (otvorený priamo je prázdny).

**Hodnotenie:** 1 bod. V repository *cv05* na GitHub aj na sigma je form spracovaný cez POST, `process.php` s kontrolami a výpočtami, `content/register.php` ako samospracujúci form s ošetrenými výpismi. Commit a push najneskôr do nasledujúceho cvičenia.

**Uzávierka 2** je koniec tohto týždňa: form spracúva PHP s validáciou vstupov a ošetrením výstupov, beží na sigma.

---

# cv05: PHP and forms, security

The week page with theory and examples: https://beata.stehlikova.website.tuke.sk/ti/06.html

## What you submit

In the repository *cv05* on GitHub and in your web space on sigma (`ti/cv05`):

- the whole website from cv04 in `web/`,
- `web/content/register.php` with `method="post"` and self-processing (checks, values back, messages at the fields, a summary),
- `practice/process.php` (the `$_POST` output, checks, calculations, treated outputs),
- two sentences about the XSS test in `README.md` (task 6.7).

## Grading

1 point. The form is processed via POST, `practice/process.php` has checks and calculations, `web/content/register.php` processes itself with treated outputs. **Deadline 2** is the end of this week. Commit and push by the next lab at the latest. Automatic tests run after every push; find the result following guide [04 Submitting](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), part 6.

## AI prompts

You fill in the `// DOPLŇ` and `[FILL IN …]` places yourself.

### Example 7: a prompt for process.php

```text
I have a form in the file content/register.php (I am sending it below) for a website
about [FILL IN the topic]. Create the file practice/process.php for PHP 8.1 that processes it.
The form sends it the data with the POST method to ../practice/process.php. No database, no JavaScript, no functions.

Structure:
1. At the top in a <?php ... ?> block: load every value from $_POST into a variable
   with an English name according to the field's name, with trim and with ?? ''
   (for the checkbox group ?? []); convert numbers with (int). A comment on every
   line saying what it does.
2. The lists of options for select, radio and checkbox as associative arrays
   (key = value from HTML, value = text for a person), names in English.
3. Checks into an associative array $errors (key = name of the field) with if / elseif:
   required name with mb_strlen at least 3, e-mail with filter_var FILTER_VALIDATE_EMAIL,
   a number in a range. Leave the other fields with the comment // DOPLŇ: check.
4. An HTML5 page with Bootstrap 5.3 from the CDN, body with the class container:
   a heading, <pre> with print_r($_POST), three values printed one by one,
   all values printed with a foreach loop into <dl> with is_array and implode,
   then if: if there are errors, alert-danger with the list of errors (key: text),
   otherwise alert-success.
5. Treat all values from the user in the output with
   htmlspecialchars($x, ENT_QUOTES, 'UTF-8').

A short comment on every block saying what it does.

Here is the form:
[PASTE content/register.php]
```

### Example 8: a prompt for the calculations

```text
In the file process.php (I am sending it) I have, after the checks, an else branch
where the data is fine. Add calculations from the form data into it, PHP 8.1, no functions:
- from the date field [FILL IN the field's name]: the date in the format d. m. Y, the English
  name of the day via date('l'), the number of days from today via strtotime and time()
  divided by 86400 and ceil,
- from the year field [FILL IN the field's name]: the number of years until today via (int) date('Y'),
- the length of the text from the field [FILL IN the field's name] via mb_strlen,
- leave one more calculation as the comment // DOPLŇ: my own calculation from my topic.
Print the results in paragraphs with <?= ?>. At every calculation a comment saying
what it calculates and in what units.

Here is the file:
[PASTE process.php]
```

### Example 9: a prompt for the self-processing form

```text
I am sending content/register.php (the form) and process.php (the checks) from a website
about [FILL IN the topic]. Rewrite content/register.php into a self-processing form,
PHP 8.1, no functions, no database. The file is included into a template via include,
so there is no <html>, <head> or menu in it, only the content for <main>.

1. At the top a PHP block: $errors = []; $sent = $_SERVER['REQUEST_METHOD'] == 'POST';
   loading of all fields from $_POST with trim and ?? as in process.php; copy the
   arrays of options from process.php; run the checks only if ($sent) and leave
   the comment // DOPLŇ: copy the checks from process.php there.
2. Then if ($sent && count($errors) == 0): a section with the summary in <dl>, values
   from the user through htmlspecialchars($x, ENT_QUOTES, 'UTF-8'), values from
   select/radio translated through the arrays of options into text for a person;
   translate the checked checkboxes with a foreach loop into a helper array and join
   them with implode.
3. else: a section with the form with method="post" and action="": above the form
   alert-danger if there are errors; every input has value with the value back
   through htmlspecialchars, the class is-invalid by the condition isset($errors['field'])
   and below it div.invalid-feedback with the error text via ?? '';
   print <option>, radio and checkbox with a foreach loop from the arrays of options
   with selected / checked by a condition; textarea with the value between the tags.
   At one field leave value and is-invalid unfilled with the comment
   // DOPLŇ: value back and the error.
4. Do not change the texts, the Bootstrap classes, label and id. A comment on every PHP line.

Here are the files:
[PASTE content/register.php and process.php]
```

## Tasks

**Task 6.1: Preparing Kubeflow.** Prepare Kubeflow following guide [04 Submitting](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), part 1: WEB is at the top of Explorer, **Go Live** is at the bottom on the status bar. If it is not there, this command in the terminal brings the extensions back:

```bash
code-server --install-extension Natizyskunk.sftp --install-extension yandeu.five-server --install-extension bmewburn.vscode-intelephense-client --install-extension mblode.twig-language-2
```

Then `F1`, **Developer: Reload Window**. The connection settings for sigma stay.

**Task 6.2: The assignment and a copy of last week.** Accept the assignment *cv05* in Classroom 50 (the link is above) and clone the repository into `~/web/ti/cv05` following guide 04, parts 2 and 3. Then copy your website from last week into it:

```bash
cd ~/web/ti
rsync -a --exclude .git --exclude .github --exclude README.md cv04/ cv05/
```

Right-click `cv05`, **Upload Folder**. Check on sigma `…/ti/cv05/web/` that the website runs.

**Task 6.3: The form to POST.** In `web/content/register.php` check that all fields have English `name` and `value` without diacritics (the checkbox group has `[]`). Add `method="post"` and `action="../practice/process.php"`. Submit the form: the address after submitting has no `?` and the page reports that `process.php` does not exist. That is fine.

**Task 6.4: process.php.** The prompt from Example 7 with your form. Save the result as `practice/process.php`. Fill in the form on sigma, submit, look at `print_r`. Submit it also with a checkbox unchecked and see which key is missing.

**Task 6.5: Checks by hand.** Complete `// DOPLŇ: check` for all remaining fields: the date via `strtotime` (and that it is in the future, if that makes sense for your topic), select and radio via `in_array` with `array_keys`, consent, the length of the message. Try every check: send a bad value and look at the message.

**Task 6.6: The browser is not enough.** F12, delete `required` in the `<input>` for the name, submit empty. `process.php` catches the error. If not, a check is missing.

**Task 6.7: XSS.** Type `<script>alert('hi')</script>` into the name, submit. If a window pops up, find the output without `htmlspecialchars` in `process.php` and fix it. Go through all outputs of values from the user, also `$value` in the loop. Add two sentences to `README.md` in the repository: what happened before the fix and after it.

**Task 6.8: Calculations.** The prompt from Example 8. Complete `// DOPLŇ: my own calculation` with something from your topic (days since, years until, number of characters, percentage…).

**Task 6.9: The self-processing form.** The prompt from Example 9. Save the result as `web/content/register.php`. Complete `// DOPLŇ`: copy the checks from `process.php` and complete the missing field. On sigma: open Registration from the menu (an empty form), submit with an error (the values stayed, red fields), submit correctly (the summary). Leave `process.php` in the repository, it is a practice file like `variables.php`.

**Task 6.10: An error on purpose.** In `web/content/register.php` change one `$_POST['email']` key to `$_POST['emial']`, submit the form, read what happened (a message, or a silent error thanks to `??`). Fix it. Then delete `method="post"`, submit, look at the address. Fix it.

**Task 6.11: Commit and push.** Following guide [04 Submitting](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), part 5; commit after every finished task. The test result following part 6.

**Task 6.12: Upload Folder and the check on sigma.** Right-click `cv05`, **Upload Folder**. Check `web/index.php`, `web/routes.php`, `web/form.php` (empty, with an error, correct), `web/contact.php` and `practice/process.php` (opened directly it is empty).

**Grading:** 1 point. In the repository *cv05* on GitHub and on sigma the form is processed via POST, `process.php` has checks and calculations, `content/register.php` is a self-processing form with treated outputs. Commit and push by the next lab at the latest.

**Deadline 2** is the end of this week: the form is processed by PHP with input validation and output treatment, it runs on sigma.
