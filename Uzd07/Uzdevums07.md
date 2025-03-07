Septitais uzdevums: zonālā statistika
================

## Termiņš

Līdz ~~(2025-01-10)~~ ~~(2025-01-27)~~ **2025-02-07**, izmantojot
[fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
un [pull
request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)
uz zaru “Dalibnieki”, šī uzdevuma direktorijā pievienojot .Rmd vai .qmd
failu, kura nosaukums ir Uzd07\_\[JusuUzvards\], piemēram,
`Uzd07_Avotins.Rmd`, kas sagatavots izvadei github dokumentā (piemēram,
YAML galvenē norādot `output: rmarkdown::github_document`), un tā radīto
izvades failu.

## Premise

Lai aprakstītu datubāzu vērtības kādās noteiktās telpas daļās, var būt
nepieciešama ģeometriju pārklāšana, griešana un savienošana, lai iegūtu
sevi interesējošos aprakstošās statistikas rādītājus. Tomēr šāds
uzdevums ar vektordatiem, ja tie sastāv no liela skaita ģeometrijām un
daudz virsotnēm, var būt ārkārtīgi RAM un laika izaicinošs. Jau atkal to
var risināt, izmantojot rastru.

Zonālā statistika ir rastra vērtību apkopošana un aprakstīšana iepriekš
definētās telpas daļās - zonās. Zonas var būt definētas kā rastrs (ja
tās nepārklājas) vai kā vektordati. Tālākā procesēšanā lietotājam ērtāk
ir izmantot vektordatus, to lietojums pieļauj arī zonu pārklāšanos
(piemēram, 1000 m buferi ap punktiem, kas ir 100 m attālumā cits no
cita). Nozīmīgākie risināmie jautājumi zonālajā statistikā, kas ir plaši
pieejama darbība dažādās GIS ir:

- rastra šūnas izmērs: jo mazāka ir rastra šūna, jo smagāki ir aprēķini,
  kas nozīmē RAM un laika resursus; jo lielāka ir šūna, jo nozīmīgāks ir
  jautājums par to kā aprēķinos iekļaut šūnas uz zonu robežām;

- šūnas uz zonu robežām: visbiežāk tradicionālā GIS pēc noklusējuma
  zonas aprakstā iekļauj visas rastra šūnas, kuru centri (vai jebkāda
  daļa, atkarībā no programmas un iestatījumiem) atrodas zonā. Ja
  aprakstāmā rastra šūnas ir mazas, tad šie malu efekti atstāj relatīvi
  nelielu ietekmi uz rezultātu, tomēr augstas izšķirtspējas rastru
  aprakstīšana ir procesēšanā izaicinoša. Atsevišķās GIS ir iespējams
  norādīt svarošanu - aprakstāmā rastra ik šūnai tiek aprēķināta daļa,
  kas iekļaujas interesējošajā zonā un šīs vērtības tiek izmantotas
  svērtajos aprēķinos. Šī ir noklusējuma procedūra R pakotnē
  {exactextractr}, kas turklāt ir viens no ātrākajiem vispār
  pieejamajiem risinājumiem zonālajai statistikai;

- zonu savstarpējā pārklāšanās: daļa tradicionālās GIS zonālās
  statistikas aprēķinos iejauc ar vektordatiem definētu zonu
  rasterizēšanu, bet rastrā zonas nevar pāklāties, kas daļā programmu
  noved pie kļūdainiem aprēķiniem, daļā citu - pie ļoti lēniem
  aprēķiniem.

Jāņem vērā, ka aprēķinu izmaksas (laiks, operatīvā atmiņa u.tml.) pieaug
arī ar zonas platību un zonu skaitu. Laika un operatīvās atmiņas
ierobežojumus var risināt ar iteratīviem aprēķiniem, aprakstāmā rastra
šūnas izmēru, jo sevišķi, ja ir pieejami svērtie aprēķini. Šī projekta
kontekstā pieminēšanas vērta ir arī paraugošanas blīvuma retināšana - ja
zonālā statistika ir veicama 10 km buferzonās ap punktiem, kas ir
novietoti ik pēc 100 m, blakus punktus rezultējošie aprēķini vienmēr būs
gandrīz identiski, attiecīgi, ir iespējams retināt paraugošanas blīvumu
ar to saprotot buferzonu zonālās statistikas aprēķināšanu, piemēram, ik
1x1 km šūnas centram, nevis katrai 100x100 m šūnai, ja novērojamā
novirze (to nepieciešams salīdzināt un aprakstīt) ir pieļaujama.

R pakotne {exactextractr} rezultātu atgriež tajā pašā secībā, kādā ir
ievades dati.

## Uzdevums

1.  Izmantojiet {exactextractr}, lai 500 m buferzonās ap [projekta
    *Zenodo*
    repozitorijā](https://zenodo.org/communities/hiqbiodiv/records?q=&l=list&p=1&s=10&sort=newest)
    pieejamo 100 m šūnu centriem (`pts100_sauszeme.parquet`), kuri
    atrodas piektajā uzdevumā izvēlētajās kartes lapās, aprēķinātu sestā
    uzdevuma ceturtajā apakšpunktā izveidotā rastra katras klases
    platības īpatsvaru. No aprēķinu rezultātiem sagatavojiet rastra
    slāņus, kas atbilst [projekta *Zenodo*
    repozitorijā](https://zenodo.org/communities/hiqbiodiv/records?q=&l=list&p=1&s=10&sort=newest)
    dotajam 100 m rastram (`LV100m_10km.tif`).

2.  Brīvi izvēlaties desmit blakus esošus 1km kvadrātus, kas atrodas
    trešajā uzdevumā ar Lauku atbalsta dienesta datiem aptvertajā
    teritorijā. Izmantojiet [projekta *Zenodo*
    repozitorijā](https://zenodo.org/communities/hiqbiodiv/records?q=&l=list&p=1&s=10&sort=newest)
    dotos 300 m un 100 m tīklu centrus, kas atrodas izvēlētajos
    kvadrātos. Aprēķiniet 3 km buferzonas ap ik centru. Veiciet zonālās
    statistikas aprēķinus lauka bloku īpatsvaram buferzonā (no tās
    kopējās platības), mērogojot aprēķiniem nepieciešamo laiku (desmit
    atkārtojumos):

- ik 100 m tīkla centram, kā aprakstāmo rastru izmantojot iepriekšējos
  uzdevumos sagatavoto lauku klātbūtni 10 m šūnā;

- ik 100 m tīkla centram, kā aprakstāmo rastru izmantojot iepriekšējos
  uzdevumos sagatavoto lauku īpatsvaru 100 m šūnā;

- ik 300 m tīkla centram, kā aprakstāmo rastru izmantojot iepriekšējos
  uzdevumos sagatavoto lauku klātbūtni 10 m šūnā, savienojiet iegūtos
  rezultātus ar 100 m tīklu, izmantojot kopīgos identifikatorus;

- ik 300 m tīkla centram, kā aprakstāmo rastru izmantojot iepriekšējos
  uzdevumos sagatavoto lauku īpatsvaru 100 m šūnā, savienojiet iegūtos
  rezultātus ar 100 m tīklu, izmantojot kopīgos identifikatorus.

Kāds ir aprēķiniem nepieciešamais laiks katrā no četriem variantiem?
Kādas tendences ir saskatāmas? Kādas ir novērotās lauku platības
īpatsvara atšķirības? Kādas ir maksimālās teorētiski sagaidāmās
atšķirības?

## Padomi

Tiks pievienoti pēc jautājumu saņemšanas.
