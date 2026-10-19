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
