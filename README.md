# Techno-TLN intraneti PoC: realisatsioonijuhis

**Sihtgrupp:** Andres (PoC realisatsiooni jätkaja)
**Versioon:** 1.0
**Olek:** osaliselt realiseeritud, jätkamine sammust 7

---

## 1. Üldine kontekst

### 1.1. Mida ehitame

Techno-TLN sisemine intranet **Microsoft Teamsi ja M365 vahenditega**, ilma uue süsteemi arendamiseta. Aluseks on üks privaatne Teams-tiim, mille taga töötab SharePointi sait. Sisu, vormid ja töövood ehitame karbist tulevate vahenditega: SharePoint Pages, Lists, Forms, Planner, Power Automate.

### 1.2. Miks Teams + M365

- O365 litsents on niikuinii olemas, lisakulu puudub.
- Töötajad tunnevad keskkonda.
- Mobiilirakendus olemas (Teams äpp).
- Identiteet ja õigused Entra ID kaudu, eraldi haldust pole vaja.
- Töötajate andmed (valdkond, linnak, ametinimetus) on juba O365-s.
- PoC saab püsti kiiresti ilma arendustööta.

### 1.3. PoC eesmärk

Näidata 2–4 nädala jooksul, et Teams + M365 katavad intraneti põhivajadused. **Sihtgrupp testimisel:** IT-meeskond + juhtkond (5–15 inimest).

### 1.4. PoC skoop

| Sees | Faas 2 (PoC järel) |
|---|---|
| Üks tiim, 6 kanalit | Mitu tiimi/valdkonda |
| Üks SharePointi sait | Hub sait + alamsaitid |
| Uudised (3 näidist) | Audience targeting, Viva Connections |
| Dokumendiraamatukogu omanike-veeruga | Metaandmed, elutsükkel, automatiseeritud üle vaatamine |
| Sündmuste list (3 vaadet) | Modereerimine automaatne |
| Tugiteate vorm + Planner | Eraldi vood HR/IT/haldus, eskaleerimine |
| Sünnipäevade list opt-out võimalusega | Automaatne sünk O365-st |
| Süsteemide lingid | Personaliseeritud Viva dashboard |
| Onboarding-viit välisele süsteemile | Süvaintegratsioon |

---

## 2. Arhitektuur

```
                 Teams "Techno-TLN intranet (PoC)" (Private)
                 ─────────────────────────────────────
                  │
        ┌─────────┼─────────┬──────────┬──────────┬──────────┐
        ▼         ▼         ▼          ▼          ▼          ▼
    01 📢    02 📰      03 📁     04 🛟     05 🎉      06 🔗
    Üldine   Uudised    Dok.      Tugi      Sündmused   Süsteemid
        │         │         │          │          │          │
        ▼         ▼         ▼          ▼          ▼          ▼
              ┌──────────────────────────────────────────────┐
              │   SharePoint sait "Intranetprelive"          │
              │   https://technoee.sharepoint.com/sites/     │
              │                                Intranetprelive│
              │   • Home page (News web part)                │
              │   • Pages: Eeskirjad, Arengukava, Struktuur, │
              │            Süsteemid, Onboarding             │
              │   • Lists: Sündmused, Sünnipäevad            │
              │   • Library: Dokumendid                      │
              └──────────────────────────────────────────────┘
                        ▲                      ▲
                        │                      │
              ┌─────────┴────────┐   ┌─────────┴──────────┐
              │  Microsoft Forms │   │   Power Automate   │
              │  • Tugiteade     │   │   • Tugi → Planner │
              │  • Sündmus       │   │   • Sündmus → mod. │
              │  • Sünnip. valik │   │   • Sünnip. opt-out│
              └──────────────────┘   └────────────────────┘
                        │
                        ▼
                  ┌──────────┐
                  │ Planner  │  "Tugiteated" board
                  └──────────┘
```

---

## 3. Kanalistruktuur

Kanalid on nummerdatud, et garanteerida järjekord olenemata emoji-sortimisest.

| Kanal | Roll | Postitusõigus | Tab-id (planeeritud) |
|---|---|---|---|
| **01 📢 Üldine** | Tähtsamad teated kogu majale | Ainult omanikud | Postitused |
| **02 📰 Uudised** | Sisemised uudised | Kõik (PoC) → omanikud (prod) | Postitused, Uudised (SharePoint News) |
| **03 📁 Dokumendid** | Siseeskirjad, juhendid | Kõik | Postitused, Failid, Eeskirjad (SharePoint Page) |
| **04 🛟 Tugi** | Tugiteated, KKK | Kõik | Postitused, Esita tugiteade (Forms), KKK |
| **05 🎉 Sündmused** | Sündmused (modereeritud) | Omanikud (modereeritud) | Postitused, Kalender, Agenda, Kategooriad |
| **06 🔗 Süsteemid** | Lingid teistesse keskkondadesse | Ainult omanikud | Postitused, Süsteemid (SharePoint Page) |

---

## 4. Olek 2026-05-29: mis on TEHTUD

✅ **Tiim loodud:** `Techno-TLN intranet (PoC)`, privaatne.
✅ **SharePointi sait olemas:** `https://technoee.sharepoint.com/sites/Intranetprelive`
✅ **Kõik 6 kanalit loodud** ülaltoodud nimedega ja kanaligrupi "Põhikanalid" all.
✅ **Postitusõigused seatud:**
   - 01 📢 Üldine — ainult omanikud saavad postitada
   - 05 🎉 Sündmused — modereerimine sees
✅ **Saidi regionaalsed sätted muudetud eestipäraseks** (Estonian locale, UTC+02:00 Tallinn, 24h kellaaeg).
✅ **SharePoint list "Sündmused" loodud** koos 8 veeruga:
   - Title (Pealkiri)
   - Algus (Date and time, with time)
   - Lõpp (Date and time, with time)
   - Kategooria (Choice: Tähtis sündmus / Õppekäik / Koolitus / Koosolek / Tähtaeg / Muu)
   - Asukoht (Single line of text)
   - Korraldaja (Person)
   - Sihtgrupp (Choice, multi-select: Kõik / Õpetajad / Töötajad / Juhtkond / Üksus)
   - Kirjeldus (Multiple lines of text, rich text)
   - Staatus (Choice: Mustand / Ootel kinnitust / Kinnitatud / Tühistatud — vaikimisi "Kinnitatud")
✅ **3 näidissündmust** sisestatud.
✅ **3 vaadet** loodud listile:
   - Kalender (kuukalendrivaade)
   - Agenda (kronoloogiline list, ainult tulevased)
   - Kategooria järgi (grupeeritud)
✅ **Vaate ja Teams-tabide üle arutletud** — soovitatud lisada 3 eraldi tab-i, igaüks oma vaatega.

---

## 5. Olek 2026-05-29: mis on POOLELI

🟡 **Sündmuste tab-ide lisamine Teamsi:** kasutaja sai juhise lisada Teamsi **05 🎉 Sündmused** kanalile **kolm eraldi Lists-tabi** (Kalender, Agenda, Kategooriad), igaüks viitab listile `Sündmused`, aga avab eri vaatega. Vajadusel kontrollida, et see on tehtud.

---

## 6. Mis on TEGEMATA — jätkamise juhend

Allpool on iga järgmise sammu juhend. Igal sammul on **mida teha**, **miks teha**, **klikiteed**, ja **kontroll**.

> ⚠️ **Üldised märkused enne tööd:**
> - Töökeskkond on M365 tenant `technoee.sharepoint.com`.
> - SharePointi sait: `https://technoee.sharepoint.com/sites/Intranetprelive`.
> - Teamsi tiim: `Techno-TLN intranet (PoC)`.
> - Microsoft muudab liideste paigutust regulaarselt — kui menüüpunkti ei ole täpselt seal, kus kirjutatud, otsi sama nimega valikut lähedalt.
> - Kõik klikiteed on antud nii inglise kui eestikeelses variandis, sest tenant võib olla mõlemas keeles.

---

### Samm 7: Dokumendiraamatukogu seadistus

**Miks:** kogu PoC kõige käitatum osa hilisemas elus — siseeskirjad, juhendid, arengukava. Iga dokument peab omama **omanikku**, et asi ei muutuks andmesurnuaiaks.

**Mida teha:**

1. Ava SharePoint sait → vasakult **Documents / Dokumendid**.
2. Loo kaustad (klõpsa **+ New / + Uus** → **Folder / Kaust**):
   - `Siseeskirjad`
   - `Juhendid`
   - `Arengukava`
   - `Struktuur`
   - `Vormid`
3. Lisa veerud dokumendiraamatukogule. Ülaservas **+ Add column** iga jaoks:

| Veerg | Tüüp | Märkused |
|---|---|---|
| `Omanik` | Person | Single, kohustuslik |
| `Üle vaadatud` | Date | Viimase ülevaatuse kuupäev |
| `Kehtib kuni` | Date | Kuupäev, millal järgmine ülevaatus |
| `Kategooria` | Choice | Eeskiri / Juhend / Arengukava / Struktuur / Vorm / Muu |

4. Lae üles 5–10 näidisdokumenti (võib olla suvalisi PDF/DOCX või lihtsalt placeholder-faile).
5. Igale dokumendile **määra Omanik** (mõni reaalne inimene IT või juhtkonnast).
6. Loo vaade `Omanike kaupa`:
   - Vaate valija → **Create new view** → nimi `Omanike kaupa`.
   - Pärast loomist: **Group by Omanik**.
7. Loo vaade `Üle vaadata`:
   - Filter: `Kehtib kuni` ≤ täna + 30 päeva.
   - Sort: `Kehtib kuni` kasvavalt.
   - Salvesta.

**Kontroll:**
- [ ] 5 kausta loodud
- [ ] 4 veergu lisatud
- [ ] 5–10 näidisdokumenti üles laetud, kõikidel Omanik määratud
- [ ] Vaade "Omanike kaupa" töötab
- [ ] Vaade "Üle vaadata" töötab

---

### Samm 8: SharePointi avaleht

**Miks:** kõigi News-postituste ja kiirviidete keskpunkt. Hiljem (Viva-faasis) see saab branditud "intraneti välimuse".

**Mida teha:**

1. SharePoint sait → ülaservas **Home / Avaleht** → paremal **Edit / Muuda**.
2. Kustuta vaikimisi sektsioonid (tühi prügikast iga sektsiooni juures).
3. Lisa sektsioonid (klõpsa "+" sektsioonide vahel):

   **Sektsioon 1 — Hero**
   - Web part: **Hero**
   - 4–5 suurt kaarti tähtsamatele kohtadele:
     - Eeskirjad ja juhendid → link Documents/Siseeskirjad
     - Tugiteade → link Forms-vormile (loome sammus 9)
     - Sündmused → link Sündmused listile
     - Süsteemid → link Süsteemide lehele (loome sammus 14)

   **Sektsioon 2 — Uudised**
   - Web part: **News**
   - Allikas: **This site**
   - Layout: **Hub news** või **Side-by-side**

   **Sektsioon 3 — Kiirviited**
   - Web part: **Quick links**
   - Lisa 6–8 linki sageli kasutatavatele teenustele (O365 portaal, Tahvel, Moodle, ÕIS, jne — täpne loend tuleb sammus 14).

   **Sektsioon 4 — Tulevased sündmused**
   - Web part: **List**
   - Vali list **Sündmused**, vaade **Agenda**.

   **Sektsioon 5 — Kontaktid**
   - Web part: **People**
   - 3–5 võtmeisikut (IT-juht, halduse kontakt, HR).

4. **Publish / Avalda**.
5. Veendu, et see leht on **Home / Avaleht** — kui ei, **Make this the home page**.

**Kontroll:**
- [ ] 5 sektsiooni avalehel
- [ ] News web part töötab (võib esialgu tühi olla)
- [ ] Sündmused web part näitab tulevasi sündmusi listist
- [ ] Quick links näitab kiirviiteid

---

### Samm 9: Uudised — 3 näidispostitust

**Miks:** uudistesektsioon vajab sisu, et testijad näeksid, mis välja näeb.

**Mida teha:**

1. SharePoint sait → **+ New / + Uus** → **News post / Uudis**.
2. Vali šabloon **Basic** või **Visual**.
3. Loo 3 erinevat uudist:
   - **Uudis 1:** Juhtkonna teade — nt "Õppeaasta algus 2026/2027 kavandamine".
   - **Uudis 2:** IT-teadaanne — nt "Hooldustööd reedel 18.06 — Moodle pole kättesaadav 18:00–20:00".
   - **Uudis 3:** Lõbusam uudis — nt "Uus kohvimasin C-korpuses!" + foto.
4. Iga uudise puhul **Publish / Avalda**.

### Samm 9.1: Lisa Uudised-tab 02 📰 Uudised kanalile

1. Teams → 02 📰 Uudised kanal → ülaservas **+**.
2. Otsi: **SharePoint** → vali **SharePoint** äpp.
3. Vali **News / Uudised**.
4. Vali sait **Intranetprelive** → kuvatakse hiljutised News-postitused.
5. Tab-i nimi: `Uudised`. Maha võta "Post to the channel about this tab".
6. **Save**.

**Kontroll:**
- [ ] 3 News-postitust avaldatud
- [ ] Need on nähtavad nii SharePointi avalehel kui Teamsi tabis
- [ ] Igal postitusel autor ja kuupäev nähtav

---

### Samm 10: SharePoint Pages — eeskirjad, arengukava, struktuur

**Miks:** püsiv sisu (mitte voogu sobiv) tuleb SharePointi lehtedel hoida. Lehed on otsitavad, versioneeritud, lihtsalt redigeeritavad.

**Mida teha:**

1. SharePoint sait → vasakult **Pages / Lehed** → **+ New / + Uus** → **Site page / Saidi leht**.
2. Loo järgmised lehed (igale eraldi):

   **Leht: Siseeskirjad ja juhendid**
   - Lühike sissejuhatus.
   - Kategooriad jaotatud rubriikidesse (sissejuhatav teave / personalipoliitika / töökorraldus jne).
   - Iga kategooria all **Quick links** või **Document library** web part viidates Documents/Siseeskirjad kausta.

   **Leht: Arengukava**
   - Lühike kokkuvõte (mis on, miks, kehtivusaeg).
   - Link täisversioonile dokumendiraamatukogus.
   - Visioon, missioon, strateegilised suunad (lühikesed plokid).

   **Leht: Struktuur**
   - Tekstiline kirjeldus struktuurist.
   - Pildina või Visio web part-iga organigramm.
   - **People** web part juhtkonna ja võtmeisikutega.

3. Igal lehel lehe lõpuni **People** web part → lehe omanik.
4. **Publish** kõik lehed.

### Samm 10.1: Lisa lehed navigatsiooni

1. SharePoint sait → vasakul navigatsiooni alaservas **Edit / Muuda**.
2. Lisa lingid loodud lehtedele.
3. Salvesta.

### Samm 10.2: Lisa "Eeskirjad" tab 03 📁 Dokumendid kanalile

1. Teams → 03 📁 Dokumendid kanal → **+**.
2. Otsi: **SharePoint Pages**.
3. Vali leht **Siseeskirjad ja juhendid**.
4. Tab-i nimi: `Eeskirjad`. Maha võta "Post to the channel about this tab".

**Kontroll:**
- [ ] 3 lehte loodud ja avaldatud
- [ ] Lehed on saidi navigatsioonis
- [ ] Tab "Eeskirjad" lisatud Dokumendid kanalile

---

### Samm 11: Tugiteate Forms-vorm + Planner + Power Automate

**Miks:** "ticketing" PoC versioonis. Üks vorm → ühele tahvlile → kõigile nähtav. Hiljem (faas 2) eraldame eri vood (IT/HR/haldus).

#### 11.1. Forms-vormi loomine

1. Ava `forms.microsoft.com` → **+ New Form / + Uus vorm**.
2. Pealkiri: `Techno-TLN tugiteade`
3. Kirjeldus: `Esita pöördumine IT, halduse, HR või turunduse osas. Kinnitus saadetakse e-postile.`
4. Lisa väljad:
   - **Valdkond** (Choice, kohustuslik): IT, Haldus, HR, Turundus, Muu
   - **Asukoht või ruum** (Text)
   - **Kiireloomulisus** (Choice): Madal, Tavaline, Kõrge, Kriitiline
   - **Probleemi kirjeldus** (Long text, kohustuslik)
   - **Lisafail** (File upload, valikuline, max 10 MB)
5. **Settings / Sätted**:
   - **Who can fill out this form:** Only people in my organization can respond.
   - **Record name:** sees.
   - **One response per person:** väljas.

#### 11.2. Planneri tahvli loomine

1. Teams → 04 🛟 Tugi kanal → ülaservas **+**.
2. Otsi: **Planner** või **Tasks by Planner and To Do**.
3. **Create a new plan** → nimi `Tugiteated`.
4. Loo bucketid (Categories):
   - `Uus`
   - `Töös`
   - `Ootel`
   - `Lahendatud`
5. Loo labelid (Tags):
   - `IT`, `Haldus`, `HR`, `Turundus`, `Muu` (värvilised)
   - `Madal`, `Tavaline`, `Kõrge`, `Kriitiline`

#### 11.3. Power Automate töövoog

1. Ava `make.powerautomate.com` → **+ Create / + Loo** → **Automated cloud flow**.
2. Nimi: `Tugiteade → Planner`
3. **Trigger:** Microsoft Forms → **When a new response is submitted** → vali `Techno-TLN tugiteade`.
4. **Action 1:** Microsoft Forms → **Get response details** → Response Id = trigger Response Id.
5. **Action 2:** Planner → **Create a task**:
   - Plan Id: `Tugiteated`
   - Bucket Id: `Uus`
   - Title: dünaamiline — `[Valdkond]: [Probleemi kirjeldus 50 esimest tähemärki]`
6. **Action 3:** Planner → **Update task details**:
   - Notes: kogu vormi sisu vormindatud kujul.
7. **Action 4** (Switch valdkonna järgi):
   - Case IT → Assign to: IT-juht
   - Case Haldus → Assign to: haldusjuht
   - jne
8. **Action 5:** Teams → **Post message in a chat or channel**:
   - Team: `Techno-TLN intranet (PoC)`
   - Channel: `04 🛟 Tugi`
   - Sõnum: `Uus tugiteade [Valdkond]: [Pealkiri]. Vt Plannerist.`
9. **Action 6:** Office 365 Outlook → **Send an email**:
   - To: vormi täitja
   - Subject: `Sinu tugiteade on vastu võetud`
   - Body: kinnitus + viit Planneri tahvlile.
10. **Save** → **Test** → esita testpöördumine vormist.

#### 11.4. Lisa Forms-tab Teamsi

1. Teams → 04 🛟 Tugi kanal → **+** → **Forms**.
2. **Add an existing form** → vali `Techno-TLN tugiteade`.
3. **Show only results** või **Collect responses** — vali **Collect responses**.
4. Tab-i nimi: `Esita tugiteade`. Maha võta postitusteade.

**Kontroll:**
- [ ] Forms-vorm valmis ja testitud
- [ ] Planneri tahvel + bucketid + labelid loodud
- [ ] Power Automate töövoog testitud — pöördumine → kaart Planneris + kinnitus e-postiga + teade kanalis
- [ ] Forms-tab Tugi kanalile lisatud

---

### Samm 12: Sündmuste modereerimise töövoog

**Miks:** kui hiljem laieneme suuremale grupile, ei taha me, et igaüks kalendrisse suvalisi sündmusi paneb. PoC-s näitame, kuidas vormi → kinnituse → kalendrisse pääsemine töötab.

#### 12.1. Forms-vorm "Sündmuse ettepanek"

1. Loo uus vorm `Sündmuse ettepanek`.
2. Väljad:
   - Pealkiri (Text)
   - Kirjeldus (Long text)
   - Algusaeg (Date + Time)
   - Lõppaeg (Date + Time)
   - Asukoht (Text)
   - Sihtgrupp (Choice, multi): Kõik, Õpetajad, Töötajad, Juhtkond, Üksus
   - Kategooria (Choice): Tähtis sündmus, Õppekäik, Koolitus, Koosolek, Tähtaeg, Muu

#### 12.2. Power Automate töövoog

1. **+ Create** → **Automated cloud flow**.
2. Nimi: `Sündmuse modereerimine`
3. Trigger: Forms → **When a new response is submitted** → `Sündmuse ettepanek`.
4. Action: Forms → **Get response details**.
5. Action: Approvals → **Start and wait for an approval**:
   - Approval type: **Approve/Reject - First to respond**
   - Title: `Uus sündmuseettepanek: [Pealkiri]`
   - Assigned to: PoC-eestvedaja (üks inimene)
   - Details: vormi sisu kokku võetud.
6. **Condition:** Outcome = Approve?
   - **Jah-haru:**
     - SharePoint → **Create item** listile `Sündmused`:
       - Title: vormist
       - Algus, Lõpp, Kategooria, Sihtgrupp, Asukoht, Kirjeldus: vormist
       - Korraldaja: vormi täitja
       - **Staatus: Kinnitatud**
     - Outlook → **Send email** vormi täitjale: "Sündmus kinnitatud, lisatud kalendrisse."
   - **Ei-haru:**
     - Outlook → **Send email**: "Sündmus tagasi lükatud, põhjus: [Approver response]"

#### 12.3. Lisa vorm 05 🎉 Sündmused kanali tabiks

1. Teams → 05 🎉 Sündmused → **+** → **Forms** → vali `Sündmuse ettepanek` → **Collect responses**.
2. Tab-i nimi: `Esita sündmus`.

**Kontroll:**
- [ ] Vorm valmis
- [ ] Töövoog testitud (esita → kinnita Approvals → kontrolli, et sündmus ilmus listi)
- [ ] Töövoog testitud teises suunas (esita → lükka tagasi → kontrolli, et email saabus)
- [ ] Vorm-tab lisatud Sündmuste kanalile

---

### Samm 13: Sünnipäevade ja tööjuubelite list + opt-out

**Miks:** demonstreerib, kuidas isikupuutuvaid andmeid (sünnipäev, juubel) saab näidata **iga töötaja oma valikul**, mitte HR-i suvast.

#### 13.1. SharePoint list

1. SharePoint sait → **+ New** → **List** → **Blank list** → nimi `Sünnipäevad ja juubelid`.
2. Veerud:
   - `Töötaja` (Person, single)
   - `Sünnipäev` (Date, ilma kellaajata — pane "Include time" välja)
   - `Tööle tulnud` (Date)
   - `Näita sünnipäeva` (Yes/No, default: **Yes**)
   - `Näita tööjuubelit` (Yes/No, default: **Yes**)

3. Lisa käsitsi 5–10 näidisrida (PoC ajal käsitsi; faas 2 — automaatne sünk).

#### 13.2. Forms-vorm "Sünnipäeva ja tööjuubeli eelistused"

Väljad:
- "Kas soovid, et Sinu **sünnipäev** oleks intranetis nähtav?" (Yes/No)
- "Kas soovid, et Sinu **tööjuubel** oleks intranetis nähtav?" (Yes/No)

#### 13.3. Power Automate töövoog "Sünnipäeva eelistuste uuendus"

1. Trigger: Forms → vormi vastus.
2. Action: Forms → Get response details.
3. Action: SharePoint → **Get items** listist `Sünnipäevad ja juubelid`:
   - Filter Query: `Töötaja/EMail eq '[Responders email]'`
4. Action: SharePoint → **Update item**:
   - Näita sünnipäeva: vormi vastus
   - Näita tööjuubelit: vormi vastus
5. Action: Outlook → kinnituskiri kasutajale.

#### 13.4. Avalehe sünnipäevakaart

1. Avalehel → **Edit** → lisa **List** web part.
2. Vali `Sünnipäevad ja juubelid`.
3. Filter: `Näita sünnipäeva = Yes` ja sünnipäev jääb +/- 7 päeva ulatusse.
4. Layout: kompaktne kaart.

#### 13.5. Eraldi loend "Tulevased tööjuubelid"

Sama loogikaga, eraldi web part avalehel.

**Kontroll:**
- [ ] List loodud, 5-10 näidisrida
- [ ] Vorm valmis
- [ ] Töövoog testitud
- [ ] Avalehel sünnipäeva ja juubeli kaardid nähtavad
- [ ] Vormi kaudu opt-out tehes muutub kaart vastavalt

---

### Samm 14: Süsteemide leht + onboarding-viit

**Miks:** üks koht, kust kasutaja näeb kõiki Techno-TLN keskkondi ja jõuab nende juurde.

#### 14.1. SharePoint leht "Süsteemid"

1. SharePoint sait → **Pages** → **+ New** → **Site page**.
2. Pealkiri: `Süsteemid`.
3. Lisa **Quick links** web part — kaartide stiilis kuva. Sisesta lingid:

| Süsteem | URL | Märkus |
|---|---|---|
| O365 portaal | https://www.office.com | |
| Tahvel | (tenant URL) | Õpiotsing |
| Moodle | (tenant URL) | E-õpe |
| ÕIS | (tenant URL) | Õppeinfosüsteem |
| Personalisüsteem | TBD | |
| Raamatupidamine | TBD | |
| Koduleht | https://www.tthk.ee | |

> **Märkus:** täpsed URL-id küsida IT-meeskonnalt enne sisestamist.

4. Iga link saab ikooni (vali kaartide juures) ja lühikirjelduse.
5. **Publish**.

#### 14.2. Lisa Süsteemide tab 06 🔗 Süsteemid kanalile

1. Teams → 06 🔗 Süsteemid → **+** → **SharePoint Pages** → vali leht `Süsteemid`.

#### 14.3. Onboarding-viit

> **Andresele märkus:** *Onboarding-süsteemi URL ja täpne staatus tuleb täpsustada PoC-tellijaga (Toivo Pärnpuu). Kui süsteem on veel valikus/arendamisel, jäta see osa kohatäitega.*

1. SharePoint → **Pages** → **+ New** → **Site page**, nimi `Onboarding`.
2. Lehe sisu:
   - Lühike sissejuhatus ("Tere tulemast Techno-TLN-i! Onboarding-protsess käib eraldi süsteemis. Vajuta nuppu altpoolt.").
   - Üks suur **Button** web part → URL **TBD** (kohatäide kuni täpsustamiseni).
   - Lühike loetelu, mida onboardingu osana läbida.
3. Lisa link **Süsteemide** lehele või avalehe Quick links sektsiooni.

**Kontroll:**
- [ ] "Süsteemid" leht loodud, lingid sisestatud (TBD kohad märgitud)
- [ ] Tab lisatud Süsteemide kanalile
- [ ] Onboarding-leht loodud (URL TBD)

---

### Samm 15: Branding ja viimistlus

**Miks:** PoC tunduks rohkem "tõeline".

1. **SharePoint sait → Settings ⚙️ → Change the look** → vali Techno-TLN värviteemale lähedane (sinine/oranž).
2. **Logo:** lae üles Techno-TLN logo SharePointi saidi logoks (256×256 px, läbipaistev taust).
3. **Tiimi ikoon Teamsis:** tiimi seaded → **Manage team** → **Settings** → **Team picture** → lae üles sama logo.
4. **Avalehe banner:** pane peale tervitustekst "Tere tulemast Techno-TLN intranetti (PoC versioon)".

**Kontroll:**
- [ ] Saidi värvid ja logo paigas
- [ ] Tiimi ikoon Teamsis muudetud
- [ ] Avalehe banner kohandatud

---

### Samm 16: Testijate lisamine ja onboard

**Miks:** PoC tegelik test käib siit edasi.

1. **Lisa testijad tiimi:**
   - Teams → tiimi `⋯` → **Add member** → IT + juhtkonna isikud (5–15 inimest).
2. **Saada testijatele avakiri** (e-post või Teamsi sõnum):
   - Pealkiri: `Techno-TLN intraneti PoC — palun testige 2 nädalat`
   - Sisu: lühitutvustus, Teamsi tiimi link, testitavad funktsioonid, viit tagasisidevormile (loome sammus 17).
3. **Korralda 30-min tutvustus** Teamsi koosolekul:
   - Demo: iga kanali ülevaade, testpöördumise esitamine, näidisuudis.
   - Mainida: PoC, sisu on näidis.
   - Vastuste küsimused.

**Kontroll:**
- [ ] Kõik testijad lisatud tiimi
- [ ] Avakiri saadetud
- [ ] Tutvustuse koosolek toimunud

---

### Samm 17: Tagasiside vorm ja testimine

1. Loo Forms-vorm `Techno-TLN intraneti PoC — tagasiside`:
   - Hindamine 1–5: leitavus, kasutusmugavus, mobiilne kogemus, üldine mulje.
   - "Mis töötas hästi?"
   - "Mis ei töötanud?"
   - "Mida puudu jäi?"
   - "Kas Sinu hinnangul on see lahendus valmis ametlikuks intranetiks?" (jah/ei/osaliselt + miks)
2. Lisa vorm 01 📢 Üldine kanali tabiks **või** saada link e-postiga 13. päeval.
3. Testperiood: 9.–13. päev.
4. **Lõpuarutelu** 14. päeval (45 min Teamsis): vaata tulemused, otsusta edasised sammud.

**Kontroll:**
- [ ] Tagasisidevorm valmis
- [ ] Vorm saadetud testijatele
- [ ] Vastused kogutud
- [ ] Lõpuarutelu toimunud, järeldused dokumenteeritud

---

## 7. Üldised soovitused Andresele

### 7.1. Töövõtted

- **Tee asju järjekorras** — sammud on järjestatud nii, et iga sammuga sünnivad järgmise sõltuvused.
- **Kontrolli pärast iga sammu** — kontrollnimekirjad on selleks olemas.
- **Testi mobiilis ka** — Teamsi äpis võivad mõned asjad teisiti välja näha.
- **Tee oma märkmeid** — kui kuskil takerdud, kirjuta üles ja arutame.

### 7.2. Tüüpilised probleemid ja lahendused

| Sümptom | Põhjus | Lahendus |
|---|---|---|
| Channel calendar / list tab ei ilmu | Pole installitud | `+` → otsi → Add |
| Forms-vorm ei näe Power Automate'i triggeris | Vorm tehtud teises kontekstis | Loo vorm uuesti, õige konto |
| Power Automate töövoog ei käivitu | Trigger ei seotud | Ava → Edit → vali trigger uuesti |
| SharePoint News ei ilmu Teamsi tabis | Ei avaldatud | Mine SharePointi → Publish |
| Person-veerg ei leia kasutajat | Tenant filter | Kontrolli, et kasutaja on tenandis |
| Kuupäev inglise vormingus | Kasutaja oma seade | Kasutaja profiilis "Always follow web settings" |
| Mobiilis erineb | Normaalne | Testi mõlemas, dokumenteeri |

### 7.3. Lahtised küsimused, mille kohta on vaja tellijapoolset täpsustust

- **Onboarding-süsteemi URL** — vajalik Sammus 14.3.
- **Sündmuste Approver** — kes on PoC-faasis sündmusekinnitaja? PoC-eestvedaja ainukese inimesena või mitu?
- **Süsteemide täpsed URL-id** — vajalik Sammus 14.1 (Tahvel, Moodle, ÕIS, Personalisüsteem, Raamatupidamine).
- **Tugiteate assignmendi loogika** — kes täpselt saab IT/HR/halduse pöördumised Plannerisse?
- **Sünnipäeva ja tööjuubeli andmete päritolu** — käsitsi sisestada vs. eksportida O365-st (PoC-s käsitsi, faas 2-s vaja otsustada).

### 7.4. Faas 2 (pärast PoC heakskiitu)

Kui PoC saab kinnituse, on järgmised loogilised sammud:

1. Laienda kogu majale — uue avalikuma tiimi loomine, sisu migratsioon.
2. **Viva Connections** seadistus (branditud intraneti välimus Teamsi sees).
3. Eraldi ticketing-vood (IT, HR, haldus, turundus eraldi).
4. Ruumide ja seadmete broneerimine (Microsoft Bookings või Power Apps).
5. Töötajate andmete automaatne sünk O365-st.
6. Onboarding-süsteemi sügavam integratsioon.
7. Governance dokument ja sisestajate koolitus.
8. Töötajate küsitlus (algses memos kavandatud) PoC-järelt korrigeeritud küsimustega.

---

## 8. Töövahendid Andresele

- **Teams** (desktop + mobiil testimiseks)
- **SharePoint Admin Center** (`https://technoee-admin.sharepoint.com`) — saidi haldus, kui vaja
- **Power Automate** (`make.powerautomate.com`) — töövoogude loomine
- **Microsoft Forms** (`forms.microsoft.com`)
- **Microsoft Lists** (`lists.microsoft.com`) — kõik listid ühes vaates

---

## 9. Kontaktid

- **PoC omanik / tellija:** [Toivo Pärnpuu]
- **PoC realisatsiooni jätkaja:** Andres Ojalill
- **Tenant administraator:** [TBD — kui vaja õigusi laiendada]

---

## 10. Lõpuks

PoC eesmärk on **õppida**, mitte ehitada lõplikku lahendust. Kui keskel selgub, et midagi tuleks teha hoopis teisiti, on see õige käitumine — dokumenteeri tähelepanek ja arutame edasi. Iga "ümbermõtlemine" PoC-faasis säästab kümme kordset töömahtu faas 2-s.

Edu!
