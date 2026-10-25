# Desecure AİM Game

Desecure Technology fuar oyunu: Alarm İzleme Merkezi (AİM) operatörü olarak enerji santralini (GES / RES / TES / JES / HES / BESS) kur, savun, her alarmda doğru kararı ver.

- **Canlı:** https://bugraozgenc-dotcom.github.io/desecureaimgame/
- **Tek dosya:** `oyna.html` (çift tıkla açılır) · GitHub Pages için `github-yayin/index.html`
- **Geliştirme:** `npm install` → `npm run dev` · derleme `npm run build` · tek dosya `npm run build:single`


Enerji tesisleri (GES / RES) için 3D izometrik güvenlik strateji oyunu.
**Planla → Yerleştir → Savun.**

## Hemen oyna
`oyna.html` dosyasını Chrome / Edge / Safari ile açın (tek dosya, kurulum gerekmez).

- Varsayılan açılış **fuar görevi**dir: bekleme ekranı → ad soyad → santral seçimi (GES / RES / TES) → kurulum (90 sn) → savunma (~80 sn) → AİM özeti açılır penceresi (YouTube QR) → sonuç (ad soyad, puan, Instagram + YouTube QR kodu, günün en iyileri).
- Sesler sağ üstteki hoparlör düğmesiyle açılıp kapatılır; tercih cihazda hatırlanır.
- Instagram hesabı / QR: `src/fair/FairApp.tsx` (INSTAGRAM_HANDLE) ve `src/assets/instagram-qr.svg`.
- YouTube kanalı / QR: `src/fair/FairApp.tsx` (YOUTUBE_HANDLE) ve `src/assets/youtube-qr.svg` (https://www.youtube.com/@DesecureTechnology).
- Olay "Hırsız" kararıyla AİM'e iletildiğinde sağda Desecure AİM animasyonu açılır (operatörler video duvarını izler, 4 adım tamamlanır, ~6 sn sonra kapanır): `src/ui/AimPanel.tsx`, `src/ui/aim.css`. Tam oyunda "AİM'e bildir" düğmesi de aynı paneli açar.
- Görev sonunda "Desecure Alarm İzleme Merkezi bu olayları gerçekleşmeden önledi" penceresi 20 sn gösterilir (`AimSummary`, `src/fair/FairApp.tsx`); metinler buradan değiştirilebilir.
- Tam strateji sürümü için adresin sonuna `#tam` ekleyin (ör. `oyna.html#tam`).
- Stant görevlisi: **Ctrl+Shift+L** günün skor tablosunu sıfırlar. Kurulumda 75 sn hareketsizlikte, sonuç ekranında 45 sn sonra oyun bekleme ekranına döner.
- Fuar ayarları: `src/game/config/levels.ts` (FAIR_LEVEL) ve `src/fair/fairStore.ts` (süre, hız, ekipmanlar).

## Geliştirme
```bash
npm install
npm run dev          # http://localhost:5173
npm run build        # dist/ (çok dosyalı üretim derlemesi)
npm run build:single # dist-single/index.html (tek dosya)
```
Gereksinim: Node.js 18+.

## Mimari (src/)
| Klasör / dosya | Görev |
|---|---|
| `game/types.ts` | Ortak tipler — tüm sistemler aynı dünya koordinatlarını kullanır (1 hücre = 1 m) |
| `game/config/equipment.ts` | 12 ekipmanın maliyet, görüş açısı, menzil, gece performansı |
| `game/config/levels.ts` | Bölümler (yeni bölüm = bu diziye bir kayıt) |
| `game/world/maps.ts` | TerrainManager veri katmanı: GES ve RES haritaları, engeller, risk, bölgeler |
| `game/systems/vision.ts` | Görüş konisi + line-of-sight (arazi yükseltisi, bina/panel/ağaç yüksekliği, gece/sis) |
| `game/systems/coverage.ts` | CameraCoverageSystem / BlindSpotManager — kapsama = gerçekten görülen hücrelerin birleşimi |
| `game/systems/pathfinding.ts` | A* navigasyon (NPC'ler engellerden geçmez) |
| `game/systems/SimulationEngine.ts` | Simülasyon döngüsü, algılama, senaryo üretimi, teknik olaylar |
| `game/systems/ThreatAI.ts` | Hırsız / hayvan / bakım personeli davranışları |
| `game/systems/PatrolAI.ts` | Devriye rotası, sevk, takip, yakalama |
| `game/systems/IncidentManager.ts` | Müdahale komutları (incele, PTZ, sınıflandır, uyar, alarm, devriye, AİM, sonlandır) |
| `game/systems/ScoringEngine.ts` | 0–100 puan, metrikler ve öneriler (simülasyon kayıtlarından) |
| `game/store.ts` | Oyun durumu (zustand), PlacementSystem kuralları, bütçe |
| `scene/*` | React Three Fiber sahnesi: arazi, instanced paneller/ağaçlar, cihaz modelleri, karakterler, katmanlar |
| `ui/*` | Menü, HUD, mağaza, olay konsolu, sonuç ekranı |
| `tests/headless.ts` | Tarayıcısız simülasyon testi |

## Notlar
- Desecure AİM, AİM, MGS gibi özellikler oyun içi simülasyon olarak modellenmiştir; gerçek ürün kabiliyetlerini temsil etmez.
- Karakterler, yapılar ve arayüz özgündür.

## Fuar: paylaşım kartı ve e-posta
- Oyun sonunda ad soyad, puan, Instagram ve YouTube QR kodları tek bir **paylaşım kartı** olarak gösterilir (1080×1350).
- **PAYLAŞ** → ziyaretçi e-postasını girer, gönderim onayını işaretler (pazarlama izni isteğe bağlı).
- E-posta ayarları: `src/fair/mailer.ts` içindeki `MAIL_CONFIG.endpoint` (ve gerekirse `headers`). Boşken talepler cihazda kuyrukta bekler; ayar yapılınca oyun açılışta kuyruğu otomatik gönderir.
- Stant paneli: **Ctrl+Shift+E** → bekleyen e-posta listesi, CSV indir / kopyala, kuyruğu temizle.
- Yeni hırsız tipleri: sürünen hırsız ve post giymiş hırsız (hayvan gibi görünür). Devriye yetişince hırsız malzemeyi bırakıp koşarak kaçar.

## Kablo makaraları ve oyun menüsü
- Tesiste kablo makaraları bulunur (`planStock`, `src/game/systems/SimulationEngine.ts`; sahnede `src/scene/CableReels.tsx`). Hırsız işini bitirince makarayı sırtlanıp kaçar.
- Hırsız devriye veya jandarma tarafından yakalanırsa çaldığı makaralar yerine döner; kaçarsa kayıp kalıcıdır.
- Bütün makaralar sahadan götürülürse görev "Tesis boşaltıldı!" ile biter.
- Oyun sırasında sağ üstteki menü düğmesi: Devam et / Oyunu bitir (puanı göster) / Ana sayfaya dön. Menü açıkken oyun durur.
- Jandarma: AİM bildirimi veya devriye yakalaması sonrası araçla gelir, şüpheliyi gözaltına alıp götürür (`src/scene/Jandarma.tsx`).

## Bonus Desecure ürünleri (MİK · MAS · MİA)
- Kurulum ekranının sağında "BONUS" paneli: oyuncu ücretsiz olarak bir Desecure ürünü seçip sahaya koyar (görev başına 1 hak, "Başka ürün seç" ile değiştirilebilir).
- MİK (Mobil İzleme Kulesi, 16 m), MAS (Mobil Alarm Standı, 12 m, en hızlı), MİA (Mobil İzleme Aracı, 14 m). 360° görür.
- Görüş alanına giren hırsızı otomatik yakalar: şüpheli yerinde durdurulur, AİM'e bildirilir ve jandarma gelip götürür. Hayvanları sesli ikazla uzaklaştırır, personele dokunmaz.
- Modeller: `src/scene/DesecureProducts.tsx`; ayarlar: `src/game/config/equipment.ts` (mik/mas/mia); ürün görselleri: `src/assets/products/`.

## Gündüz / Gece
- Görev seçim ekranında "Zaman: Gündüz / Gece" seçilir (son seçim hatırlanır).
- Gece: karanlık sahne, el fenerli hırsızlar, %15 fazla bütçe. Mağazaya **Termal Kamera** (ısı izinden görür) ve **Aydınlatma Direği** (karanlık noktaları aydınlatır; aydınlık alanda kameralar tam menzil görür) eklenir. Görev hedeflerinde "Termal kamera kur" ve "Karanlık bir noktayı aydınlat" çıkar.

## "Ya güvenliğim Desecure'da olsaydı?" demosu
- Sonuç ekranındaki düğme aynı sahayı (aynı santral, gündüz/gece, zorluk) tam Desecure kurulumuyla yeniden başlatır: her kritik varlıkta kamera (gece termal + aydınlatma), köşelerde kule, MİK + MAS + MİA, devriye, araç, siren.
- 60 saniye boyunca olaylar tek tek sırayla gelir (hırsız → hayvan → personel …) ve her hareket otomatik tanınır (`level.demo`), hırsızlar durdurulup jandarmaya teslim edilir, personel/hayvan yanlış alarm üretmez. Sağda AİM paneli ve solda canlı akış görünür.
- Bitince kısa bir özet kartı çıkar ve oyuncunun kendi puan ekranına dönülür (puanı değişmez). `startDemo` / `endDemo`: `src/fair/fairStore.ts`; `DemoHUD`: `src/fair/FairApp.tsx`.

## Puanlama (fuar)
- Korunan kablo makarası × 150 · Tespit edilen hırsız × 120 · Doğru karar × 100 · Yanlış karar/alarm −60; sonuç zorluk çarpanıyla çarpılır (Kolay ×0,8 · Normal ×1 · Zor ×1,6).
- Yıldız: olası en yüksek puana oranla %25 / %50 / %75. Tesis boşaltılırsa veya görev hiç karşılaşma olmadan bırakılırsa 0 yıldız ("Tesis boşaltıldı!" / "Pes etti").

## Eğlence paketi (v2)

- **Seri bonusu:** art arda doğru kararlar puanı katlar (2. ×1,5 · 3. ×2 · 4+ ×3). Üst barda turuncu "×N seri" çipi; yanlış karar seriyi bozar.
- **Alarm fırtınası:** son 30 saniyede ekran kenarı kırmızı yanıp söner, müzik hızlanır, iki olay üst üste gelir (`storm` bayrağı, `SimulationEngine.stormAt`).
- **Taktik müdahale (savunma sırasında, sol alt):** Saha sireni ×2, Ek devriye ×1, gece Projektör ×1 (`useTactic`, `tacticSiren/tacticBackup/tacticFloodlight`).
- **Hava ve sürprizler:** her görevde rastgele açık/sis/yağmur (üst barda rozet, kurulum kartında not); Normal/Zor'da bir kez 10 sn **elektrik kesintisi** — sıradan kameralar kör, termal/kule/Desecure ürünleri çalışır (`blackout`).
- **Kaçış sahnesi:** makarayla sahadan çıkan hırsızı kamyonet alır ve uzaklaşır (`Getaway`, `scene/Getaway.tsx`); AİM özetinde "Kaçırılan makara değeri: N × 18.000 ₺".
- **Günün Baş Operatörü:** bekleme ekranında ve 1. olan oyuncunun sonuç ekranında büyük şerit (`PRIZE_TEXT` sabitiyle ödül yazısı).
- **Düello (2 oyuncu):** kayıt ekranında sekme; aynı saha, aynı olay sırası (aynı tohum), sırayla oynanır; sonunda karşılaştırma + rövanş.
- **Rozetler:** Sıfır yanlış alarm, Jandarma dostu, Gece kartalı, Makara bekçisi, Seri avcı, Fırtına dindi, Karanlıkta nöbet, Keskin göz, Taktikçi, Sis delici / Yağmur nöbetçisi — sonuç ekranında ve puan kartında.
- **Meydan Okuma (eski adıyla kampanya):** görev seçiminde "GES → RES → TES"; bölümler arasında temsili Desecure vaka dosyası, sonunda toplam puan ve meydan okuma unvanı.
- Hırsız tipleri: koşucu, sinsi, ekip, sahte personel, sürünen, post giymiş ve **ağaç kılıklı** (1–2 sn yürür, 2–4 sn ağaç gibi donar; dururken neredeyse görünmez, bacakları alttan belli olur; kaçarken kılığı atar — `variant: tree`, `TreeDisguise`).

## Yeni sahalar: JES ve HES

- **JES-4 Jeotermal Santrali** (`mapId: 'jes'`, görev 104): dört üretim kuyusu (K-1…K-4, buhar tüten vana ağaçları), reenjeksiyon kuyusu, buhar separatörü, hava soğutmalı kondenser, türbin-jeneratör binası, şalt sahası; kuyulardan separatöre yükseltilmiş buhar hatları (`GeoPipes`). Hedefler: şalt sahası + en az 2 kuyu başı.
- **HES-5 Hidroelektrik Santrali** (`mapId: 'hes'`, görev 105): kuzeyde baraj gövdesi ve rezervuar (su; hırsız o yönden gelmez, kuzey çiti yok), üç cebri boru, santral binası, şalt sahası, trafo yağı tankı, kuyruk suyu kanalı + servis köprüsü (`HydroWater`). Ana kapı güneyde x=-16. Hedefler: şalt sahası + cebri borular.
- Meydan okuma artık beş santral (GES → RES → TES → JES → HES), iki yeni vaka dosyasıyla; meydan okuma unvanı toplam yıldıza göre ölçeklenir.
- Yeni bina türleri: `wellhead`, `fancooler`, `separator`, `dam`, `penstock` (`IndustrialModels.tsx`).
- **BESS-6 Batarya Depolama Tesisi** (`mapId: 'bess'`, görev 106): dört batarya bloğu (A–D, her biri 3 konteyner + PCS inverter skidi, HVAC üniteli beyaz kabinler, yeşil durum LED'i), şalt sahası, yedek modül deposu, yangın söndürme konteyneri, kontrol binası. Hedefler: şalt sahası + en az 2 batarya bloğu. Meydan okuma altı tesise çıktı. Yeni bina türleri: `battery`, `pcs`.

## Zorluğa göre harita boyutu

- **Kolay:** temel saha. Kurulum süresi her zorlukta 120 sn. **Normal:** çit her yönde 4 hücre dışa alınır (daha geniş açık alan, bütçe ×1,15). **Zor:** çit 8 hücre dışa alınır, bütçe ×1,35 (RES'te ×1,6) — büyüyen bantta rastgele ağaç/çalı ve çit içinde kör nokta adayı çalılar (`Builder.grow`, `growFill`, `LevelDef.mapGrow`).
- **RES Zor:** 6 türbin sahaya dağınık (T1–T6, `MapOpts.turbines`), her türbine ayrı servis yolu, kuzey kapısı x=-20.
- Kamera uzaklığı ve sis mesafeleri harita boyutuyla ölçeklenir (`mapScale`).

## Grafik ve seçim ekranı (v3)

- Render: ACES filmic ton eşleme, yumuşak gölgeler (PCFSoft), gradyan gökyüzü kubbesi, güneş/ay diski, süzülen alçak poligon bulutlar, gece yıldızları, gündüz kuş sürüsü (`scene/Sky.tsx`); ana yollarda kesikli orta şerit ve kenar bandı.
- Görev seçimi: sol sütunda zaman/zorluk ve canlı "Görev özeti" (hırsız sayısı, saha boyutu, kurulum süresi, puan çarpanı), sağda 3×2 kart ızgarası — kartta kod rozeti, ÖNERİLEN/YENİ etiketleri, bütçe/süre ve günün en iyi puanı, hover'da büyüyen görsel ve "Seç" düğmesi.
- Açılış metni: "Göreve hazır mısın, operatör?" / "GÖREVE BAŞLA".
- Detay katmanı (`scene/Props.tsx`): çim tutamları, çiçek öbekleri, çakıl, bina çevresinde varil/kasa/palet/makara, kontrol binasında koni ve tabela, çatılarda klima üniteleri ve havalandırma bacaları, ana yol boyunca aydınlatma direkleri (gece yanar), çit dışında kütükler. Zeminde 2×2 dama deseni ve bina dibinde aşınmış toprak halkası (`buildingWear`). Bulutlar kaldırıldı. Canvas'ta hafif doygunluk filtresi.

- Logo: koyu zemin logosunda "TECHNOLOGY" yazısı beyaz (amblem ve ayraç bordo kaldı); puan kartı aynı SVG'yi kullandığı için orada da beyaz.
- Teknoloji: Phaser.js kullanılmadı — React 18 + TypeScript + three.js (@react-three/fiber, drei) + zustand + Vite.

## Gözcü Drone

Fuar mağazasında Güvenlik Kulesi yerine **Gözcü Drone** (DRN): 3.800 kredi, 1 hücrelik dok, 10 m irtifada 360° görüş, 22 m menzil, engellerin/tepelerin üzerinden bakar, kendi bataryasıyla elektrik kesintisinde çalışır, görev başına en fazla 2 adet. Dok çevresinde daire çizerek süzülür, pervaneleri döner, konum ışıkları yanar (`SecurityEquipment.tsx` → `case 'drone'`). Demo kurulumunda köşelere drone konur. Tam oyunda kule hâlâ mevcut.

## Sürpriz olaylar ve puanlama (v4)

- **Gündüz:** elektrik kesintisi yok; onun yerine görevin ortasında 14 sn **toz fırtınası** — optik kameraların menzili yarıya iner, termal/drone/Desecure ürünleri %75'te kalır (`env.dust`, `startDustStorm`); sahne kum rengi sise bürünür, yatay toz parçacıkları uçuşur, HUD'da sayaçlı bant. Rozet: "Toz fırtınası gazisi".
- **Gece:** 10 sn elektrik kesintisi devam eder. (Kolay'da iki olay da yok.)
- **Puanlama:** sahaya girip hiç tespit edilemeyen her hırsız **−100 puan** ("Tespit edilemeyen hırsız" satırı). Sonuç ekranında açılır "Puan dökümü" listesi tüm satırları gösterir.
- **Puan kartı PNG:** sonuç ekranında karta tıklamak ya da "Puan kartını PNG olarak indir" düğmesi 1080×1350 PNG'yi bilgisayara indirir (`desecure-aim-game-ad-soyad.png`).
- Performans: canvas üzerindeki CSS doygunluk filtresi kaldırıldı (bazı GPU'larda kare hızını düşürüyordu), gölge PCF'e döndü, yağmur yokken yağmur parçacıkları hesaplanmıyor. **Uyarlanabilir kalite** (`scene/gfx.ts`): 4 sn ortalaması 28 fps altına düşerse otomatik olarak çözünürlük çarpanı 1'e, gölge haritası 1024'e iner ve çim/çiçek/çakıl katmanı gizlenir.

## Fuar modu (şifreli)

Giriş ekranında iki mod kartı var: **Normal mod** (varsayılan, "GÖREVE BAŞLA") ve **Fuar modu** ("ŞİFRE İLE AÇ" → şifre kartı, `src/game/config/expo.ts`). Fuar modu açılınca ekran altın temaya geçer (üst şerit, "FUAR MODU AÇIK" rozeti, fuar kartı seçili), Normal mod kartındaki "Normal moda geç" ile kapanır; ayar `localStorage`'da kalır (`desecure-fair-expo`). Aynı oyun ve görevler, stant kurallarıyla:

- Mağazada **Bullet Kamera (Sabit)** (tek başlı, `bulletCam`, 200 kredi) ve sabit kamera **Çift Bullet Kamera (Sabit)** (400 kredi) olur: tek direkte iki başlık (`Device.yaw2`). Köşeye konunca başlıklar iki çite, düz çitte iki yöne bakar (`fenceYaws`); "Döndür" ikisini birlikte çevirir. Görüş hesabı (`cameraSees`) iki yönden birini yeterli sayar; koniler ve hayalet önizleme iki yelpaze çizer.
- **Termal kamera** diğer kameraların 3 katı menzile sahip (23 → 69 m) ve gündüz de mağazada.
- **Bullet kamera yalnızca çit hattına ve köşelere** konabilir (içeride geçersiz: "Bullet kamera yalnızca çit sınırına kurulur"); PTZ, termal ve drone hem çit üstüne hem içeriye konabilir (`validatePlacement` → `nav === 2`). Yerleştirdikten sonra "Döndür" ile yön değiştirilebilir.
- Kamera konunca (ya da taşınınca) **en yakın çite / dışarıya** kendiliğinden döner; köşede çapraz bakar (`fenceYaw`). Hayalet önizleme de bu yönü gösterir.
- **Kurulum süresi yok:** sayaç çipi "∞", otomatik savunmaya geçiş kapalı; operatör "Savunmaya geç" der.
- **Hırsız ve hayvanlar kapsama dışındaki çitten girer:** simülasyon başında her çit/kapı hücresinde hırsızın çömelip kestiği nokta için kamera görüşü hesaplanır (`computeFenceCoverage` → `nav.fenceCovered`). Kapsanan çit hücresinin geçiş maliyeti 12 kat (`pathfinding.cellCost`), doğum noktası ağırlığı kapsanan bölümde 0,08×, açık bölümde 3× (`spawnBias`). Acil yerleştirilen kamera kapsamayı günceller.
- **Çit çevresi seyrek:** fuar haritasında çitin iki yanındaki 9 m'lik banttaki ağaçların ~%18'i kalır, 3 m içinde hiç ağaç kalmaz; görüş/geçiş engelleri buna göre yeniden işlenir (`thinFenceTrees`, `getMap(..., { expo })`), harita kenarı süs ağaçları da seyrekleşir. Sınır her yönden görünür.
- Üst çubukta sarı **FUAR** rozeti; kurulum giriş kartında fuar notu.

### Fuar modu: sınır güvenliği kurgusu

Amaç oyuncuya sınır güvenliğini yaşatmak; iç alan için ipucu verilmez.

- **Kredi:** her harita / zorluk / gece-gündüz / hava için çitin tamamını kapatmanın en ucuz maliyeti açgözlü küme örtüsüyle hesaplanır (`systems/expoBudget.ts`, `expoPerimeterPlan`). Bütçe = bu maliyet ×1,25 + iç alan için iki kamera payı (gündüz 2 PTZ, gece 2 termal). Örn. GES Normal gündüz: sınır 2.600 → bütçe 6.900. Oyuncu pahalı ekipmana önce harcarsa sınırı kapatamaz.
- **Kurulum:** görev hedefleri, makara etiketleri (x/y), risk katmanı, makara notu ve taktik notu gizli; "Stratejini sen kur."
- **Hırsızlar:** çitte kamera görmeyen bölüm varsa hırsız ve hayvanlar yalnızca o boşluğa yakın noktadan doğar (`spawnBias`), kapsanan çit 60 kat pahalı (`pathfinding`). Çiti 2,6 sn'de keser, içeride 1,6× hızla hedef makaraya koşar (`expoEntry`). Çit %100 kapsamdaysa çite yaklaşırken görülür.
- **Otomatik AİM:** karar kartı yok. Kameraya giren olayı AİM 0,4 sn içinde kendisi sınıflandırır (`fairStore.autoAim`): hırsız → hedefli uyarı (geri kaçar, `aimRepel`) + devriye + jandarma, hayvan → sesli uyarı, personel → doğrulama. Elle karakter işaretleme kapalı, taktik çubuğu yok. Savunma başladıktan sonra kamera eklenemez (acil yerleştirme hakkı 0, mağaza gizli).
- **Çalınma uyarısı:** hırsız fark edilmeden makara alırsa ekranın ortasında "BAKIR KABLO ÇALINDI! Makaraları koruyan bir kamera olmadığı için hırsız fark edilmeden kabloları aldı." uyarısı, 3,4 sn ağır çekim ve kamera o noktaya kayar (`engine.theftLog`).
- **Puan:** çit kapsaması ×5 (en çok 500) + korunan makara ×150 + AİM'in durdurduğu hırsız ×120 − fark edilmeden giren hırsız ×100. %100 sınırda "Sınır bekçisi" rozeti. Sonuç özetinde "Sınır kapsaman %X — N boşluktan M hırsız girdi, K makara çalındı"; puan kartında "Çit kapsaması". Desecure demosu fuar modunda çit hattını %100 kapatan kurulumla oynar.

### Fuar modu: akıllı hırsız ve kamera sabotajı

- Bir çit kamerasının direğini başka hiçbir kamera görmüyorsa ve konisi komşu koniyle çit üstünde kesişmiyorsa kamera **savunmasız** sayılır (`camWatched` / `fenceConnected`, `unwatchedFenceCams`, `engine.vulnerable`). Hırsız kameranın hemen dış tarafında (`coverage.sabotageSpot`) çömelip sabote eder. Bullet kamera çit boyunca baktığı için kendi direğini göremez; onu komşu kamera izlemelidir.
- Çitte boşluk yokken savunmasız kamera varsa hırsız **Sabotajcı hırsız** olarak doğar (`spawnSaboteur`): kameraya en yakın noktadan kör noktaya yaklaşır, 2,6 sn çömelip kamerayı devre dışı bırakır (`engine.sabotage`, `DeviceRuntime.sabotaged`), çit kapsaması yeniden hesaplanır ve açılan boşluktan en yakın makaraya koşar. Sonraki hırsızlar da bu boşluğu kullanır.
- Sabotajcı yaklaşırken bir kameraya görünürse AİM müdahale eder ve sabotajcı planından vazgeçip kaçar.
- Sabotaj anında turuncu uyarı: "KAMERA SABOTE EDİLDİ! BLT-02 kamerasını başka bir kamera görmüyordu. Hırsız kör noktadan yaklaşıp kamerayı devre dışı bıraktı." (ağır çekim + kamera odaklanır). Kameranın konisi kırmızıya döner.
- Puan: sabote edilen her kamera −100; "Sınır bekçisi" rozeti için sabotaj olmamalı. Sonuç özetinde "N kamera sabote edildi: kameralar birbirini görmüyordu".
- Kredi artık **karşılıklı görüşlü** tam sınır planına göre: çit kapsandıktan sonra her kameranın sabotaj noktasını başka bir kamera görene dek kamera eklenir (`expoPerimeterPlan`, `coverCost` / `cost`). Bütçe = bu maliyet ×1,2 + iç alan payı. Örn. GES Normal gündüz: kapsama 2.600, güvenli plan 4.200. Fuar modunda teknik arıza (kamera kopması) yok; kamerayı yalnızca hırsız düşürür. Demo sabotaj içermez.

### Fuar modu: kör noktadan sızan hırsız, pahalı termal

- Simülasyon başında (ve kamera eklenince / sabote edilince) her kameranın gördüğü hücreler çıkarılır (`coverage.cameraSeenCells` → `nav.camSeen`). Hırsız rotasında kameranın gördüğü her hücre +12 maliyet: akıllı hırsız dışarıda da içeride de kör noktalardan dolanarak ilerler (`pathfinding.cellCost`). Hayvanlar bu hesabı yapmaz.
- Hedef seçimi: önce kameranın görmediği makara, hepsi izleniyorsa en yakını (`pickTarget`). İç alandaki makaraları kamerayla izlemek böylece doğrudan işe yarar.
- Fuar modunda **termal kamera 5.000 kredi** (`EXPO_THERMAL_COST`; normal modda 2.200). İç alan kredi payı artık 2 PTZ (+ gece 2 aydınlatma direği); termal paya girmez. Örn. GES Normal gündüz bütçesi 8.700.

### Demo: Ana sayfa butonu

Desecure demosunun üst çubuğunda (ses düğmesinin yanında) **Ana sayfa** butonu var; demo bittiğinde çıkan kartta da "Puanıma dön"ün yanında yer alır. Tıklanınca demo durur ve giriş (mod seçimi) ekranına dönülür (`DemoHUD.goHome` → `toAttract`).

### Kaçış rotası ve metinler

- Hırsız geldiği yoldan kaçar (her modda): doğduğu nokta (`originX/Z`) ve çitten ilk girdiği hücre (`entryCell`) kaydedilir. Makarayı alınca (ya da siren/devriye ile kaçarken) önce kendi kestiği deliğe, oradan geldiği noktaya yürür; kendi deliğinden çıkarken çiti yeniden kesmez (`startEscape`, `ThreatAI` "ownHole").
- "Gerçek sahada AİM bunu durdurur / 7/24 tutar" türü cümleler kaldırıldı (kamyonet uyarısı, görev özeti, pes / tesis boşaldı notları, demo sonu, meydan okuma sonu, fuar sınır özeti).

### Fuar modu: tempo ve sınır algılama düzeltmesi

- **Bekleme yok:** ilk hırsız savunma başlar başlamaz (~2 sn) doğar, ~3–5 sn içinde çite varır; sonrakiler düzenli aralıkla gelir (`generateSchedule`, fuar dalı). Hırsız sayısı fuar modunda Kolay 20 · Normal 24 · Zor 32 (`EXPO_THIEVES`, `fairStore.ts`; Normal'de ~3 sn'de bir hırsız) + alarm fırtınasında 2 hırsız. Aynı anda sahada en fazla 4–6 hırsız, 2–3 jandarma aracı olur. Sahadan ayrılan / gözaltına alınan şüphelinin olayı kendiliğinden kapanır, harita işaretleri birikmez. Hedef makaraya maliyet alanı önbelleğe alınır ve hırsızın yolu doğrudan bu alandan çıkarılır (`pathFromField`); doğum başına ~1–3 ms. Görev seçimindeki özet de bu sayıyı ve "Süresiz kurulum"u gösterir.
- **Dört yandan gelirler:** hırsızlar karıştırılmış bir sırayla K/D/G/B yönlerinden sırayla doğar (`sideOrder`); hedef makara o yöne yakın olanlardan seçilir. Çitte boşluk varsa boşluğun yönü ve komşu yönler kullanılır (uzak yönden dolaşma yok). HES'te kuzey (baraj) yoktur, üç yön.
- **Akıllı yaklaşma, dolaşma yok:** hırsız önce hedef makarayı seçer (kamerasız olan önce), sonra hedefe en ucuz yoldan (çit boşluğu / kör nokta) ulaşabileceği doğum noktasından gelir (`expoEntry`, `pathfinding.costField`). Eskiden kapsanan sınırda çit dışında 30–50 sn dolaşıyordu.
- **Kapsanan çitten giriş yok:** kapsama ölçütü artık hırsızın çiti çömelerek kestiği nokta (çit hücresi + dış kenarı, diz hizası; `FENCE_CUT_H`, `fenceCutPoint`). Kredi planı (`expoPerimeterPlan`) aynı ölçütle hesaplanır. Çit kesme 2,6 sn (`EXPO_CUT`), AİM görüntü analizi algılamayı 3 kat hızlandırır, AİM 0,4 sn içinde müdahale eder ve kamerada görülen hırsızı hedefli uyarıyla geri kaçırır (`aimRepel`). Eskiden uyarı %33 ihtimalle işe yaramıyor, hırsız kameraya rağmen giriyordu.
- **Sürpriz olay yok:** fuar modunda toz fırtınası ve elektrik kesintisi kapalı (kapsamayı rastgele bozup "kameraya rağmen giriş" yaratıyordu).
- **İlk hırsız sabotajcı olmaz:** ziyaretçi önce AİM'in sınırda nasıl durdurduğunu görür; kameralar birbirini görmüyorsa 2. hırsızdan itibaren sabotaj gelir.
- Test (her harita, Kolay/Normal/Zor, açık/sis, gündüz/gece, 8 tohum): tam ve karşılıklı görüşlü kurulumda içeri giren hırsız 0; yalnızca çiti kapsayan kurulumda sabotaj sonrası, boşluklu kurulumda boşluktan giriş sürüyor. İlk temas ortalama 3–6 sn.

### Fuar modu: sabotaj ölçütü (gevşek)

- Bir çit kamerası **izleniyor** sayılır: (a) direği başka bir kameranın konisinin içindeyse (`camWatched`) **veya** (b) konisi çit üstünde komşu bir kameranın konisiyle kesişiyorsa, yani aradaki çit kesintisiz kapsanıyorsa (`fenceConnected`: çit halkasında kameradan iki yöne yürünür, kapsanmayan hücreye rastlamadan başka bir kameraya ulaşılırsa izleniyor) — `unwatchedFenceCams`, `coverage.ts`.
- Yani ekranda koniler çit üstünde birbirine değiyorsa sabotaj olmaz. Sabotaj yalnızca kamera hem menzil dışında hem de konisi komşusuna ulaşmıyorsa mümkündür; bu durumda genelde çitte gerçek bir boşluk vardır ve hırsız sabotaj yerine boşluktan girer. Pratikte sabotaj çok nadir görülür.
- Eski ölçütler: önce kameranın 1,4 m dışındaki sabotaj noktası (kayalar / çalılar / eğim gizleyince yanlış sabotaj), sonra yalnızca direk-konide (27 m aralıklı köşe + kapı kameralarında koniler ortada kesişse de direkler 19 m menzil dışında kalıyor, sabotaj oluyordu).
- Sabotaja açık kamera listesi savunma başında bir kez çıkarılır; kredi planındaki karşılıklı görüş adımı (`expoPerimeterPlan`) aynı ölçütü kullanır — çit %100 kapsanınca ek kamera gerekmez, kredi = kapsama maliyeti ×1,2 + iç alan payı (örn. GES Normal gündüz 6.200).
- Test: köşe + kenarlara 8–22 m arayla çift bullet (6 saha) ve tam kapsama planları: sabotaj 0; boşluklu kurulumda boşluktan giriş sürüyor.

### v70: dört yandan giriş, HES notu, demo kamerası, sürüm etiketi

- **Kapı artık çit kadar zor:** fuar modunda kapalı kapının geçiş maliyeti çitle eşitlendi (`pathfinding.cellCost`, nav 3). Eskiden kapalı kapı (8) kapsanan çitten (14) ucuz olduğu için bütün hırsızlar ana kapıya yığılıyor, tek kamera her işi yapıyordu.
- **Hedef çeşitliliği:** hırsız hedefini yönüne yakın makaralar arasından seçer; kamerasız makara 3 kat çekici ama tek hedef değil. Doğum noktası en ucuza 25 m paylı 8 adaydan rastgele (`expoEntry`). Normal GES'te 26 hırsız çitin 12 ayrı 8 m'lik bölümünden yaklaştı (HES'te 3 cepheden 9 bölüm).
- **HES:** görev kartı notu (v71'de tekneyle giriş geldi; bkz. aşağısı).
- **Desecure demosu:** kamera artık her yakalamada olay yerine yaklaşmıyor; saha uzaktan izlenir (`DemoHUD`'daki focusOn efekti kaldırıldı).
- **Sürüm etiketi:** giriş ekranının sol altında `SÜRÜM v70 · 23.10.2026` (`src/version.ts` → `APP_VERSION`, `.cr-ver`). Her yayında burayı bir artır.

### v71: HES'te tekneyle gelen hırsızlar

- **Kuzey çiti (v71'de etekte, v72'de tepede):** HES'in kuzey çit hattı artık suda değil (v71: eğik yüzün dibinde z = −18,5; v72: tepede z = −27,5; `MapData.fenceNorthZ`, `grid.fenceNorth`). Doğu/batı çitleri bu hatta bağlanır; cebri boruların geçtiği yerde çit yok (borular engel). Bu hatta bullet/çift bullet kamera konur, kapsama ve kredi planı onu da sayar (`fenceRing`, `expoPerimeterPlan`).
- **Tekne rotası:** rezervuarda 10 doğum noktası (`SpawnPoint.boat`, kuzey yönü). Hırsız kayıkla kıyıya gelir (3,2 m/s), baraj tepesine çıkar (13 m), eğik yüzden etekteki çite iner, çiti keser; kara yolu karaya çıkınca hesaplanır (`MapData.boat`, `engine.boatWps`, `boatLanded`, `NPC.wp` düz çizgi ara noktaları — nav yok sayılır, `NPC.yOff` zemin üstü yükseklik). Kayık + kürek modeli `SimLayer.tsx`.
- **Kaçış:** su/baraj üstündeyken fark edilirse tekneyle geri döner; içeride fark edilirse baraj eteğine yürür, tepeden tekneye biner (`wpAfter`). Jandarma su/baraj üstündeki şüpheliye gönderilmez; tekneyle kaçırılan makara `lootLost`'a yazılır.
- Hayvanlar tekne noktalarını kullanmaz. Diğer haritalarda değişiklik yok (`fenceNorthZ` yok → −L).
- Test: tam kapsama planında 8/8 tekne hırsızı etekteki çitte fark edilip tekneyle kaçtı; kuzey hattı kamerasızken 7/8 içeri girdi. HES'te dört yön: D/K/G/B dağılımı dengeli.

### v72: barajın tepesinde çit, HES kredisi, kapsama kenar payı, tam tur testi

- **Kuzey çiti baraj tepesinde:** HES'in kuzey çit hattı barajın tepe yolunun rezervuar kenarında (z = −27,5); doğu/batı çitleri yamaçtan tırmanıp bu hatta bağlanır — baraj çitle çevrilidir. Baraj gövdesi içinde zemin tepe yoluna çıkarılır (`HES.crestH`, `heightAt`), gövde görüş engeli yalnızca korkuluk (1 m); böylece tepedeki kameralar 17 m'den rezervuarı ve sahayı görür. Tekneyle gelen hırsız kıyıya çıkar, **tepedeki çiti keser**, eğik yüzden sahaya iner (`Waypoint.tag`, çit hücresine varınca `cutting`). Kaçışta suda ise doğrudan geri, tepedeyse çit → kıyı → su.
- **HES kredisi +%15** (`expoBudget`, `mapMult`): örn. Normal gündüz 8.900, Zor gece 16.000.
- **Kapsama kenar payı:** çit hücresi kameranın koni kenarına 8° yakınsa "kapsanıyor" sayılmaz (`FENCE_FOV_MARGIN`; plan ve oyun aynı ölçüt). Gece köşelerde koni kenarındaki hücreden fark edilmeden giriş oluyordu; artık plan o köşeye kamera ekler.
- **Tam tur testi (gerçek derleme, Playwright):** 6 bölüm × Normal-gündüz ve Zor-gece; kurulum → savunma → sonuç ekranı; JS hatası 0; tam kapsamda içeri giren 0; hırsızlar dört yönden dengeli (HES'te tekne dâhil, örn. G8/B8/D7/tekne7). Başsız test (`hl.ts`): 6 bölüm × Kolay/Normal/Zor × açık/sis × gündüz/gece — fark edilmeden giriş 0, yaklaşma 9–19 ayrı 8 m'lik çit bölümünden.

### v73: baraj çiti görünür

- Baraj tepesindeki çit hattı tepenin **saha tarafındaki kenarına** alındı (z = −24,5): rezervuar kenarındaki çit, açık renkli betonun üstünde ve korkuluğun arkasında görünmüyordu. Tekne rotası: kıyı → tepe (kuzey kenar) → çit (güney kenar, kesilir) → etek.
- Baraj tepesi çiti **koyu renkli** ve direkleri 2 m'de bir (`MapObjects.Fence`: HES kuzey kenarı ayrı geometri, `darkPostMat`); baraj modelinde saha tarafındaki korkuluk kaldırıldı (çit onun yerinde).
- Doğu/batı çitleri yamaçtan tırmanıp bu hatta bağlanır; baraj tepesi + iki yamaç çitle çevrilidir.

### v74: baraj çiti gerçekten görünür

- v73'te çit vardı ama tepedeki kalın bordo şerit (0,5 m) ve yol kalınlığı çitin alt yarısını örtüyordu; uzaktan görünmüyordu. Tepe zemini yol seviyesine (`HES.crestH` 13,3) çıkarıldı, bordo şerit inceltildi (0,2 m). Koyu çit ve direkler artık yolun önünde net.

### v75: tesis ve baraj birlikte çit içinde

- HES'in kuzey çiti barajın tepesinde **rezervuar kenarında** (z = −27,5): çit tesisi ve barajın tamamını çevreler. Doğu/batı çitleri yamaçtan bu hatta bağlanır. Baraj modelindeki korkuluklar kaldırıldı (çit onların yerinde), koyu çit ve sık direkler yolun arkasında, suya karşı net görünür.
- Tekne rotası: kıyı → çit (kesilir) → tepe → eğik yüzden etek. Kaçış: tepedeyse çit → kıyı → su; suda ise doğrudan geri.

### v76: baraj dört taraftan çit içinde

- Barajın arkasında 5 m'lik **kuru kıyı şeridi** açıldı (`HES.shoreGap`; su artık z < −33'te başlar, `HydroWater` buna göre). Kuzey çiti bu şeridin ortasında (z = −31,5): çit, tesisi ve barajı dört taraftan çevreler, baraj ile çit arasında yürünebilir kuru alan vardır (kullanıcının krokisi). Doğu/batı çitleri barajın iki ucundan dolanıp bu hatta bağlanır.
- Tekne rotası: su → kıyı şeridi → çit (kesilir) → barajın arkasından tepeye tırmanış → eğik yüzden etek. Kaçış tersi.
- Kredi: çit uzadığı için HES kredisi arttı (Normal gündüz açık 10.300 civarı).

### v77

- HES kıyı çitindeki boşluk (eski cebri boru geçidi, yalnızca görselde kalmıştı) kapatıldı; çit kesintisiz.

### v78: her durumda dört yandan

- Çitte boşluk olsa da hırsızlar K/D/G/B sırasıyla kendi yönlerinden gelir; boşluk başka yöndeyse çit boyunca dışarıdan dolanıp boşluğa gider (eski "boşluğun yönünden gel" istisnası kaldırıldı). Tüm bölüm ve zorluklarda test: tam kapsama ve boşluklu kurulumda dağılım dengeli (örn. GES Normal boşluklu D8/K5/G7/B6).

### v80: tek noktaya yığılma yok

- Kapsanan çit hücresinin geçiş maliyeti 14×60'tan 14×3'e indirildi (`EXPO_COVERED_MULT`, `pathfinding.ts`). Boşluk ~30 m'den yakınsa hırsız oradan girer; uzaktaysa kendi yönündeki çiti keser (kamera görüyorsa AİM yakalar). Böylece boşluklu kurulumda bile giriş denemeleri çitin 6–15 ayrı noktasına dağılır; boşluktan fark edilmeden giriş sürer. Tüm bölüm/zorluklarda test edildi.

### v81: AİM paneli sağ altta

- Desecure AİM animasyon paneli (`ui/aim.css`) sağ alt köşeye alındı (`bottom: 16px`, 360 px; kısa ekranlarda 320 px). Ortadaki "AİM: HIRSIZ TESPİT EDİLDİ" geri bildirimi ve uyarıları örtmüyor; 1280×800, 1366×768, 1920×1080'de çakışma yok. Kısa ekran medya sorgusundaki `top: 84px` kalıntısı paneli uzatıyordu, kaldırıldı.
- Tam tur testi: 6 bölüm × Kolay-gündüz, Normal-gece, Zor-gündüz — JS hatası 0, tam kapsamda giriş 0, dört yön dengeli.

### v82: grafik kalitesi seçimi

- Kullanıcı grafik modunu kendisi seçer: **Otomatik** (kare hızı düşerse kalite kendiliğinden azalır), **Yüksek**, **Düşük** (çözünürlük 1×, gölge 1024, ayrıntı katmanı yok). Seçim cihazda saklanır (`desecure-gfx`, `scene/gfx.ts`).
- Düğme: giriş ekranı üst çubuğunda ses düğmesinin yanında, oyun içi üst çubukta (tıkla → sırayla değişir) ve duraklatma menüsünde üç seçenekli kutu (`ui/GfxToggle.tsx`, `GfxPicker`).

### v83: e-posta ile gönderme kaldırıldı

- Sonuç ekranındaki "PAYLAŞ · Puan kartını e-postana gönder" düğmesi ve e-posta penceresi kaldırıldı; puan kartı yalnızca PNG olarak indirilir. Stant paneli (Ctrl+Shift+E) ve `mailer.ts` kuyruğu kodda duruyor ama yeni talep üretilmez.

### v84: telsiz kutusu kaldırıldı

- Giriş ekranındaki "FUAR · EIF / STANT KONSOLU" (normal modda "TELSİZ · JANDARMA") kutusu kaldırıldı.

### v3.6: yeni sürüm numarası

- Sürüm numarası v3.6'dan yeniden başlar (sol altta yalnızca "v3.6" görünür, "SÜRÜM" başlığı yok). Bundan sonra her yayında 0.1 artar: `src/version.ts` → `APP_VERSION`.

### Fuar şifresi (GitHub'a uygun)

Şifre kaynakta ve yayın dosyasında açık yazılmaz; yalnızca özeti tutulur (`EXPO_PASSWORD_HASH`, `checkExpoPassword`, `src/game/config/expo.ts`). Büyük/küçük harf fark etmez. Şifreyi değiştirmek için oyunu açıp tarayıcı konsolunda `__expoHash('yeni-sifre')` çalıştırın, çıkan değeri `EXPO_PASSWORD_HASH`'e, yeni uzunluğu `EXPO_PASSWORD_LEN`'e yazıp yeniden derleyin. Not: bu bir gizleme; kodu inceleyen kararlı biri yine de deneme yoluyla bulabilir, fuar modu için yeterlidir.

## GitHub'a yükleme

`github-yayin/` klasöründeki iki dosya deponun köküne yüklenir:

- `index.html` → oyunun kendisi (tek dosya, GitHub Pages bunu açar)
- `README.md` → bu açıklama

Depo: github.com/bugraozgenc-dotcom/desecureaimgame → **Add file → Upload files** → iki dosyayı sürükle → **Commit changes**. Pages ayarı (Settings → Pages → Branch: main / root) bir kez yapıldıysa 1–2 dk içinde canlı adres güncellenir.

## Giriş ekranı: AİM izleme merkezi (Konsept B)

Bekleme ekranı bir alarm izleme merkezidir (`FairApp.tsx` → `Attract`, stiller `fair.css` → `.cr-*`). Sahne 1440×900 sabit tasarlanır ve ekrana orantılı ölçeklenir (`useStageScale`), her çözünürlükte aynı görünür.

- Üstte logo, "CANLI · AİM ALARM İZLEME MERKEZİ", canlı saat ve ses düğmesi; altında kırmızı LED tabela "GÖREVE HAZIR MISIN, OPERATÖR?".
- Video duvarında iki mod ekranı: **Normal mod** (kırmızı, nişangâh köşeleri, "GÖREVE BAŞLA") ve **Fuar modu** (gece görüntüsü, altın, "ŞİFRE İLE AÇ"). Ekranlarda tarama çizgisi, kayan ışık bandı, tarih/saat damgası. Görseller `assets/cr-normal.jpg`, `assets/cr-expo.jpg` (oyunun kendi sahneleri).
- Masa: klavye, PTZ joystick, telsiz ekranı ve önde ekranlara bakan özel güvenlik görevlisi (`CrGuard`, CSS ile çizilir).
- Fuar şifresi: duvar kararır, LED "ERİŞİM KİLİDİ · FUAR MODU" olur, 7 kutulu erişim kodu paneli büyüyerek açılır. Yanlış kodda kutular kırmızı titrer ve "ERİŞİM REDDEDİLDİ" yazar; Esc veya dışarı dokunmak kapatır.
- Fuar modu açıkken oda altın tonuna geçer: "FUAR MODU AÇIK" rozeti, "✓ ERİŞİM ONAYLANDI", altın "GÖREVE BAŞLA"; Normal ekran "BEKLEMEDE" ve "Normal moda geç".
- Sesler (`sfx`): ekran üzerine gelince `uiHover`, `modeNormal`, `modeExpo`, tuş `key`, `unlock`, `denied`, `cancel`, `expoStart`. Ses düğmesi kapalıysa çalmaz.
