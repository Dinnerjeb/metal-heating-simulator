# Metal Heating Lab / Metal Isıtma Laboratuvarı

[English](#english) · [Türkçe](#türkçe)

https://dinnerjeb.github.io/metal-heating-simulator

---

## English

An interactive, single-file browser simulation of four metal bars (copper, iron, stainless steel, tungsten) being heated. As the metals warm up, they glow the way real hot metal does: dull red near 800 K, then orange, yellow and white. The colour is computed from the Planck blackbody spectrum, not picked from a fixed palette.

No build step, no dependencies, works offline. Open `index.html` in a browser.

### Features

- **Heat sources:** contact heater, induction coil, laser
- **Blackbody glow:** colour and brightness from Planck's law, with a bloom effect
- **Temperature–time graph:** one probe per bar; click a bar to move its probe
- **Heat map:** switch between realistic view and false-colour temperature view
- **Thermal expansion:** bars lengthen as they heat (drawn 12× exaggerated)
- **Melting and latent heat:** temperature holds at the melting point while latent heat is absorbed
- **Environment:** air, vacuum, fan, water spray
- **Spectrometer:** live Planck curve (log scale) of the selected probe, visible band and Wien peak
- **Time scaling:** 1× to 500×, Kelvin or Celsius

### Physics model

Each bar is a 1D rod (0.4 m long, 10 mm thick, 96 cells) solved with explicit finite differences:

```
ρc · ∂T/∂t = k · ∂²T/∂x²  −  (2/d) · [ εσ(T⁴ − Tₐ⁴) + h(T − Tₐ) ]  +  q(x)
```

- Conduction along the bar, radiation and convection from both faces (ambient Tₐ = 300 K)
- Convection coefficient *h*: air 15, vacuum 0, fan 120, water spray 2500 W/m²K
- Heater: thermostat-controlled block with contact conductance 3000 W/m²K at one end
- Induction: volumetric heating in a ±4 cm zone, scaled per material; iron loses efficiency above its Curie point (1043 K)
- Laser: Gaussian spot (σ = 2 cm) on the selected bar, scaled by each metal's absorptivity
- Melting: enthalpy method; each cell stores latent heat and holds at the melting temperature until fully melted or solidified
- Glow colour: Planck radiance sampled at 465, 550 and 610 nm, normalised and tone-mapped; brightness compression approximates eye response

| Material | α (m²/s) | ρc (J/m³K) | ε | Melting point (K) | Linear expansion (1/K) |
|---|---|---|---|---|---|
| Copper | 1.11e-4 | 3.45e6 | 0.30 | 1358 | 17e-6 |
| Iron | 2.3e-5 | 3.5e6 | 0.70 | 1811 | 12e-6 |
| Stainless steel | 4.0e-6 | 3.9e6 | 0.60 | 1700 | 17e-6 |
| Tungsten | 6.8e-5 | 2.58e6 | 0.35 | 3695 | 4.5e-6 |

Values are typical textbook approximations, not precise engineering data.

### Limitations

- 1D model: no heat flow across the bar's thickness, no 2D heat map
- Molten metal does not flow or drip
- Emissivity, conductivity and heat capacity are constant (they change with temperature in reality)
- Induction and laser power levels are illustrative, not calibrated to a specific device
- Not suitable for engineering design or safety decisions

### Run locally

```
git clone <your-repo-url>
cd <your-repo>
open index.html
```

Or enable **GitHub Pages** (Settings → Pages → deploy from `main`) to host it online.

### License

MIT. See `LICENSE`.

---

## Türkçe

Dört metal çubuğun (bakır, demir, paslanmaz çelik, tungsten) ısıtılmasını gösteren, tek dosyalık, tarayıcıda çalışan etkileşimli bir simülasyon. Metaller ısındıkça gerçek sıcak metal gibi parlar: 800 K civarında donuk kırmızı, sonra turuncu, sarı ve beyaz. Renk sabit bir paletten seçilmez, Planck kara cisim spektrumundan hesaplanır.

Derleme adımı ve bağımlılık yok, çevrimdışı çalışır. `index.html` dosyasını tarayıcıda açmanız yeterli.

### Özellikler

- **Isı kaynakları:** kontak ısıtıcı, indüksiyon bobini, lazer
- **Kara cisim parlaması:** renk ve parlaklık Planck yasasından, ışık yayılması (bloom) efektiyle
- **Sıcaklık-zaman grafiği:** her çubukta bir ölçüm noktası; çubuğa tıklayarak noktayı taşıyabilirsiniz
- **Isı haritası:** gerçekçi görünüm ile sahte renkli sıcaklık görünümü arasında geçiş
- **Isıl genleşme:** çubuklar ısındıkça uzar (çizimde 12 kat abartılı)
- **Erime ve gizli ısı:** gizli ısı emilirken sıcaklık erime noktasında sabit kalır
- **Ortam:** hava, vakum, fan, su püskürtme
- **Spektrometre:** seçili noktanın canlı Planck eğrisi (logaritmik ölçek), görünür bant ve Wien tepe noktası
- **Zaman ölçeği:** 1× ile 500× arası, Kelvin veya Celsius

### Fizik modeli

Her çubuk, açık sonlu farklar yöntemiyle çözülen tek boyutlu bir çubuktur (0,4 m uzunluk, 10 mm kalınlık, 96 hücre):

```
ρc · ∂T/∂t = k · ∂²T/∂x²  −  (2/d) · [ εσ(T⁴ − Tₐ⁴) + h(T − Tₐ) ]  +  q(x)
```

- Çubuk boyunca iletim; iki yüzeyden ışıma ve konveksiyon (ortam sıcaklığı Tₐ = 300 K)
- Konveksiyon katsayısı *h*: hava 15, vakum 0, fan 120, su püskürtme 2500 W/m²K
- Isıtıcı: bir uçta, 3000 W/m²K temas iletkenliğiyle termostatlı blok
- İndüksiyon: ±4 cm’lik bölgede hacimsel ısıtma, malzemeye göre ölçeklenir; demirin verimi Curie noktasının (1043 K) üstünde düşer
- Lazer: seçili çubukta Gauss profilli nokta (σ = 2 cm), her metalin soğurma oranıyla ölçeklenir
- Erime: entalpi yöntemi; her hücre gizli ısıyı depolar ve tamamen eriyene ya da katılaşana kadar erime sıcaklığında kalır
- Parlama rengi: 465, 550 ve 610 nm’de Planck ışıma yoğunluğu, normalize edilip ton eşlenir; parlaklık sıkıştırması göz tepkisini yaklaşık olarak taklit eder

| Malzeme | α (m²/s) | ρc (J/m³K) | ε | Erime noktası (K) | Doğrusal genleşme (1/K) |
|---|---|---|---|---|---|
| Bakır | 1,11e-4 | 3,45e6 | 0,30 | 1358 | 17e-6 |
| Demir | 2,3e-5 | 3,5e6 | 0,70 | 1811 | 12e-6 |
| Paslanmaz çelik | 4,0e-6 | 3,9e6 | 0,60 | 1700 | 17e-6 |
| Tungsten | 6,8e-5 | 2,58e6 | 0,35 | 3695 | 4,5e-6 |

Değerler ders kitaplarındaki tipik yaklaşık değerlerdir, hassas mühendislik verisi değildir.

### Sınırlamalar

- Tek boyutlu model: çubuğun kalınlığı boyunca ısı akışı yok, 2 boyutlu ısı haritası yok
- Erimiş metal akmaz, damlamaz
- Yayma katsayısı, iletkenlik ve ısı kapasitesi sabittir (gerçekte sıcaklıkla değişir)
- İndüksiyon ve lazer güçleri örnekleyicidir, belirli bir cihaza göre kalibre edilmemiştir
- Mühendislik tasarımı veya güvenlik kararları için uygun değildir

### Yerelde çalıştırma

```
git clone <depo-adresiniz>
cd <depo-adi>
open index.html
```

Ya da **GitHub Pages**’i açarak (Settings → Pages → `main` dalından yayınla) çevrimiçi yayınlayabilirsiniz.

### Lisans

MIT. Bkz. `LICENSE`.
