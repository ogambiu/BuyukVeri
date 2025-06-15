# LoL Loadout Tahmin Sistemi

Bu proje, **League of Legends** oyuncuları için şampiyon, tier ve pozisyon bilgilerine dayanarak **item** ve **perk** tavsiyeleri sunan bir öneri sistemidir. Projede, Riot Games API'sinden veri çekilmiş, veri işlenmiş ve PyTorch kullanılarak **çoklu etiketli sınıflandırma (multi-label classification)** modelleri eğitilmiştir. Sonuçlar, Gradio üzerinden görsel bir arayüzle sunulmuştur.

---

## ✨ Projenin Amacı

Oyuncuların oyun başında yaptıkları seçimlerin başarıya etkisini analiz etmek ve sadece **şampiyon**, **tier** ve **pozisyon** bilgilerinden yola çıkarak çoklu öğe (örn. item ve perk) tahmini yapan bir sistem geliştirmek.

---

## 📃 Veri Seti

Veriler, Riot Games API kullanılarak otomatik şekilde aşağıdaki gibi toplanmıştır:

- 7 farklı tier (Iron, Bronze, Silver, Gold, Platinum, Emerald, Diamond)
- Her tier'dan 4 segment (I-IV), toplamda 28 segment
- Her segmentten ~100 oyuncunun 50 ranked maçı (yaklaşık **1.4 milyon katılımcı**)

### Veri Çekme

Kodlar Riot API ile JSON formatında veri çeker ve disk dosyasına kaydeder.

### Veri Birleştirme & Temizleme

- 28 dosya tek CSV dosyasına dönüştü.
- KDA < belirli seviye olan oyuncular çıkartıldı (performansa dayalı filtre).
- Sadece meta item ve perk ID'leri içerikte bırakıldı.
- Veri "long" formata dönüştürüldü (her satır = tek hedef).

---

## 🧱️ Kullanılan Teknolojiler

- **Python**
- **Pandas**, **Seaborn**, **Matplotlib**: veri işleme ve görsel analiz
- **PyTorch**: derin öğrenme modeli
- **Sklearn**: etiket dönüştürme ve metrik hesaplama
- **Gradio**: web arayüz

---

## 📊 Model Mimarisinin Özellikleri

- **Girdi**: Şampiyon ismi, tier, pozisyon (kategorik encoding)
- **Çıktı**: 6 perk ve 6 item ID'si (multi-label)
- **Embedding katmanları**: her girdi için
- **2 katmanlı Fully Connected + Dropout**
- **Loss**: BCEWithLogitsLoss
- **Optimizer**: Adam
- **Metrikler**: Precision@K, Recall@K

---

## 🌟 Gradio Arayüz

Projede, test edilebilir bir ön yüz Gradio ile sunulmuştur:

```bash
pip install gradio
```

```python
iface.launch()
```

- Kullanıcıdan `tier`, `champion` ve `lane` alır.
- Çıktı olarak **tavsiye edilen item ve perk listesi** verir.

---

## 📊 Görsel Sonuçlar

Modelin performansı şu grafiklerle sunulmuştur:

- BCE kayıp eğrisi
- Precision@6/7 & Recall@6/7
- Etiket frekans dağılımı (item ve perk)
- Confusion Matrix (TP, FP, FN) – Top 10 etiket için

> Grafikler `./figures/` klasörüne kaydedilmiştir.

---

## 📚 Kaynaklar

- Riot API: https://developer.riotgames.com/docs/portal
- PyTorch Docs: https://pytorch.org/docs/stable/index.html
- BCEWithLogitsLoss: https://pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html
- Multi-label classification: https://scikit-learn.org/stable/modules/multiclass.html#multilabel-classification
- Item ve Perk ID listesi: https://darkintaqt.com/blog/item-ids

---

## 🚀 Katkılar

Pull request ve issue'larınızı bekliyoruz. Her katkı memnuniyetle kabul edilir ✨

---

## 🚧 Uyarı

Bu proje, eğitim amaçlı bir çalışmadır ve herhangi bir Riot Games resmi desteğine sahip değildir. Tüm veriler kamuya açık API'ler kullanılarak elde edilmiştir.
