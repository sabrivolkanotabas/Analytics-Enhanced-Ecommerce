![Anatomi](./anatomi-png.png) 

#

# Universal Analytics (UA) Gelişmiş E-Ticaret Geliştirici Kılavuzu
 
Bu belgenin amacı, e-ticaret siteniz için kullanıcı davranışını, ürün/hizmet satışlarının ve kullanıcı eylemlerinin Universal Analaytics’e gönderilmesini sağlamak için gerekli dataLayer kodlarını entegre etmeye yardımcı olmaktır.
 
 
## 1. Gelişmiş E-ticaret Etkinlikleri
 
Gelişmiş E-Ticaret raporlaması, kullanıcılarınızın alışveriş davranışlarına ilişkin öngörüler sağlar ve en popüler ürünlerinizi ölçmenize, promosyonların ve ürün yerleştirmenin gelirleri nasıl etkilediğini görmenize olanak tanır.

- Etkileşim: Kullanıcı bir sayfa gördüğünde etkileşim olay gönderilir.
- Ürün Görüntüleme: Kullanıcı ürün sayfasının detayına geldiğinde ürünle ilgili bilgiler ```view_item``` event isminde dataLayer’a eklenir.
- Tıklama: Yukarıda bahsedilen view_item_list ve view_item olayları tetiklenmeden önce, kategori veya ürüne tıklanınca tıklanan sayfanın bilgileri ```select_item``` event isminde dataLayer’a eklenir.
- Sepete Ekle: Kullanıcı ürünü sepete eklediğinde ürün bilgileri ```add_to_cart``` event isminde dataLayer’a eklenir.
- Ödeme: Kullanıcı ödeme adımlarına geldiğinde ürün biligileri ```begin_checkout``` event isminde dataLayer’a eklenir.
- Alışveriş: Satın alma işlemi başarılıyla tamamlandığında, ```purchase``` event isminde dataLayer’a eklenir.
- Promosyonlar: Sayfadaki Slyat veya Banner Performansı ölçmek için içerdiği bilgiler ```view_promotion``` event isminde dataLayer’a eklenir.


### 1.3.1 Etkileşim

Google Tag Manager ile yapılandırılan UA etiketinde etkileşim olayı otomatik gönderir. Bunun için herhangi bir kurulum yapmanıza gerek yoktur.

 
### 1.3.2 Ürün Görüntülüme
 
```view_item``` ürün görünümlerini ölçer. Bu olay, ürün sayfası yüklendiğinde gerçekleşir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "view_item",
    'ecommerce': {
        'detail': {
            'actionField': {
                'list': 'Search Results'  // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Saat kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
            },
            'products': [{
                'name': 'Bayan Kol Saati',
                'id': '12345',
                'price': 7.70,
                'brand': 'Google',
                'category': 'Saat',
                'variant': 'Metalik Gri',
                'quantity': 1
            }]
        }
    }
});
```
 
### 1.3.3 Ürün Tıklama
 
Bu event, ürüne tıklamaları ölçmek için kullanılır.
Yukarıda bahsedilen ```view_item``` olayları tetiklenmeden önce, ürüne tıklanınca tetiklenir. Tıklanan ürünün bilgileri ```select_item``` event isminde dataLayer’a eklenir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "select_item",
    'ecommerce': {
        'click': {
            'actionField': {
                'list': "Search Results", // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Saat kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
            },
            'products': [{
                'name': 'Bayan Kol Saati',
                'id': '12345',
                'price': 7.70,
                'brand': 'Google',
                'category': 'Saat',
                'variant': 'Metalik Gri'
            }]
        }
    }
});
```
 
### 1.3.4 Sepete Ekle
 
```add_to_cart``` sepete eklenen ürünlerin ölçümünü yapar.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "add_to_cart",
    'ecommerce': {
        'currencyCode': 'TRY',
        'add': {
            'products': [{
                'name': 'Bayan Kol Saati',
                'id': '12345',
                'price': 7.70,
                'brand': 'Google',
                'category': 'Saat',
                'variant': 'Metalik Gri',
                'quantity': 1
            }]
        }
    }
});
```

### 1.3.5 Ödeme Bilgisi
 
```add_payment_info``` kullanıcı ödeme sırasında ödeme bilgilerini eklediğinde tetiklenir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "add_payment_info",
    'currency': "TRY",
    'value': 156, // Toplam tutar.
    'items': [{
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 96.70,
            'item_brand': "Google",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
        },
        {
            'item_name': "Erkek Kol Saati",
            'item_id': "12345",
            'price': 59.30,
            'item_brand': "Apple",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
        }
    ]
});
```

### 1.3.6 Ödeme Başlatma
 
Bir kullanıcı ödeme işlemini başlattığında gönderilir ve ödeme başlatma işlemlerini ölçmek için kullanılır. Toplanan veri ```begin_checkout``` event isminde dataLayer’a ilave edilir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "begin_checkout",
    'ecommerce': {
        'checkout': {
            'actionField': {
                'step': 1,
            },
            'products': [{
                    'name': 'Bayan Kol Saati',
                    'id': '12345',
                    'price': 96.70,
                    'brand': 'Google',
                    'category': 'Saat',
                    'variant': 'Metalik Gri',
                    'quantity': 1
                },
                {
                    'name': 'Erkek Kol Saati',
                    'id': '12345',
                    'price': 59.30,
                    'brand': 'Apple',
                    'category': 'Saat',
                    'variant': 'Metalik Gri',
                    'quantity': 1
                }
            ]
        }
    }
});
```
 
### 1.3.7 Alışveriş

```purchase``` satın alma işlemini tamamladığında gönderilir. Ödeme işlemi başarılı olduğu taktirde aşağıdaki kod bloğu çalışır ve satın alınan ürünlerin listesi obje halinde dataLayer’a eklenilir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': "purchase",
    'ecommerce': {
        'purchase': {
            'actionField': {
                'id': 'T12345',
                'revenue': '35.43',
                'tax': '4.90',
                'shipping': '5.99',
            },
            'products': [{
                    'name': 'Bayan Kol Saati',
                    'id': '12345',
                    'price': 96.70,
                    'brand': 'Google',
                    'category': 'Saat',
                    'variant': 'Metalik Gri',
                    'quantity': 1
                },
                {
                    'name': 'Erkek Kol Saati',
                    'id': '12345',
                    'price': 59.30,
                    'brand': 'Apple',
                    'category': 'Saat',
                    'variant': 'Metalik Gri',
                    'quantity': 1
                }
            ]
        }
    }
});
``` 

## 1.4 Promosyon Görünümleri ve Etkileşimler
 
Slider/Promosyon görünümlerini ve etkileşimlerini ölçmek, kullanıcılar tarafından ne sıklıkta görüntülendiği ve seçildiği hakkında bilgi verir. Ürün verileriyle birlikte aşağıdaki olaylar da kampanyaların etkileşimini ölçmenize yardımcı olabilir.

Eğer sitenizde slider veya promosyon olarak kullandığınız görsel yoksa bu etkinlikleri yoksayabilirsiniz.
 
### 1.4.1 Slider Etkileşimi
 
```view_promotion``` bir kullanıcıya belirli bir slider/promosyon gösterildiğinde gönderilir.

Not: Bu event her slider kaydırıldığı zaman tetiklenir.
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': 'view_promotion',
    'ecommerce': {
        'promoView': {
            'promotions': [{
                'id': 'JUNE_PROMO13',
                'name': 'June Sale',
                'creative': 'banner1',
                'position': 'slot1'
            }]
        }
    }
});
```
 
### 1.4.2 Slider Tıklanması
 
```select_promotion``` bir kullanıcı belirli bir slaytı tıkladığında gönderilir.
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': 'select_promotion',
    'ecommerce': {
        'promoClick': {
            'promotions': [{
                'id': 'JUNE_PROMO13',
                'name': 'June Sale',
                'creative': 'banner1',
                'position': 'slot1'
            }]
        }
    }
});
```
