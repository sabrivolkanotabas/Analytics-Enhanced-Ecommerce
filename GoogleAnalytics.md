![Logo](https://s3-eu-central-1.amazonaws.com/roipublic.com/wp-content/uploads/2020/03/13230345/Roipublic-Black.png)

#

# Google Analytics (UA/GA4) Gelişmiş E-Ticaret Geliştirici Kılavuzu.
 
Bu belgenin amacı, e-ticaret siteniz için kullanıcı davranışını, ürün/hizmet satışlarının ve kullanıcı eylemlerinin Universal Analaytics ve Google Analytics GA4’e gönderilmesini sağlamak için gerekli dataLayer kodlarını entegre etmeye yardımcı olmaktır.
 
 
## 1. Gelişmiş E-ticaret Etkinlikleri.
 
Gelişmiş E-Ticaret raporlaması, kullanıcılarınızın alışveriş davranışlarına ilişkin öngörüler sağlar ve en popüler ürünlerinizi ölçmenize, promosyonların ve ürün yerleştirmenin gelirleri nasıl etkilediğini görmenize olanak tanır.

- Etkileşim: Kullanıcı bir sayfa gördüğünde etkileşim olay gönderilir.
- Ürün Listesi Görünümleri: Ürün Kategori sayfasındaki ürün bilgileri ```view_item_list``` event isminde dataLayer’a eklenir.
- Ürün Görüntüleme: Kullanıcı ürün sayfasının detayına geldiğinde ürünle ilgili bilgiler ```view_item``` event isminde dataLayer’a eklenir.
- Tıklama: Yukarıda bahsedilen view_item_list ve view_item olayları tetiklenmeden önce, kategori veya ürüne tıklanınca tıklanan sayfanın bilgileri ```select_item``` event isminde dataLayer’a eklenir.
- Sepete Ekle: Kullanıcı ürünü sepete eklediğinde ürün bilgileri ```add_to_cart``` event isminde dataLayer’a eklenir.
- Sepetten Çıkar: Kullanıcı sepet sayfasında ürünü sepetten çıkardığında çıkarılan ürün bilgileri ```remove_from_cart``` event isminde dataLayer’a eklenir.
- Ödeme: Kullanıcı ödeme adımlarına geldiğinde ürün biligileri ```begin_checkout``` event isminde dataLayer’a eklenir.
- Alışveriş: Satın alma işlemi başarılıyla tamamlandığında, ```purchase``` event isminde dataLayer’a eklenir.
- Promosyonlar: Sayfadaki Slyat veya Banner Performansı ölçmek için içerdiği bilgiler ```view_promotion``` event isminde dataLayer’a eklenir.
 
## 1.3 Kurulum Dokümantasyonu.

NOT: Kurulum aşamasında kodlar içerisinde dikkatinizi çekebilir; ürünle ilgili bilgileri items dizisine ekliyorsak neden tekrardan products dizisine ekliyoruz? Ya da items dizi içinde ürün id değeri item_id anahtarına ekliyorsak neden tekrar id anahtarına ekliyoruz? Bu Google Analytics UA ve GA4 arasındaki farklardır, GA4 items dizisi içerisinden alırken, UA products dizisi içine alıyor. Aynı şekilde GA4 ürün id değerini item_id'den elırken, Google Ads Remarkeiting id parametsi üzerinden almaktadır. 

### 1.3.1 Etkileşim

Google Tag Manager ile yapılandırılan GA4 etiketinde etkileşim olayı otomatik gönderir. Bunun için herhangi bir kurulum yapmanıza gerek yoktur.
 
### 1.3.2 Ürün Listesi Görünümleri.

Kategori sayfalarında listelenen ürünlerin performansını ölçmek için, dataLayer’a bir ürün listesi gönderin ve bu verileri ```view_item_list``` event altında toplayın. ‘items’ içindeki objeler, sayfadaki ürünlerden eklenecektir ve en fazla 7 adet ürün eklensin.

Arama ile sonuçlanan ürünlerin listesi de bu olaya dahil edilir ve item_list_name değeri “Search Results” olur. 
 
NOT: Eğer arama sonucu ürün bulunamazsa o zaman view_item_list olayı tetiklenmez.

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "view_item_list",
    "item_list_name": "Search Results",
    'items': [{
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 50.70,
            'item_brand': "Google", // Ürün markası
            'item_category': "Saat", // Ürün kategorisi
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Search Results",
            'id': "12345", // item_id değeri eklenecek.
            'google_business_vertical': 'retail' // Google Ads parametresi, değişmez.
        },
        {
            'item_name': "Erkek Kol Saati",
            'item_id': "67890",
            'price': 23.00,
            'item_brand': "Google", // Ürün markası
            'item_category': "Saat", // Ürün kategorisi
            'item_category2': "Erkek Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Search Results",
            'id': "12345", // item_id değeri eklenecek.
            'google_business_vertical': 'retail' // Google Ads parametresi, değişmez.
        }
    ]
});
```
 
### 1.3.3 Ürün Görüntülüme.
 
```view_item``` ürün görünümlerini ölçer. Bu olay, ürün sayfası yüklendiğinde gerçekleşir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "view_item",
    'currency': "TRY",
    'value': 7.77, // Ürün fiyatı
    'items': [{
        'item_name': "Bayan Kol Saati",
        'item_id': "12345",
        'price': 7.70,  // Ürün fiyatı
        'item_brand': "Google", // Ürün markası
        'item_category': "Saat", // Ürün kategorisi
        'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
        'item_category3': "3. Kategori",
        'item_category4': "4. Kategori",
        'item_variant': "Metalik Gri",
        'quantity': 1,
        'item_list_name': "Search Results", // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Saat kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
        'id': "12345", // item_id değeri eklenecek.
        'google_business_vertical': 'retail' // Google Ads parametresi, değişmez.
    }],
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
 
### 1.3.4 Ürün Tıklama.
 
Bu event, ürüne tıklamaları ölçmek için kullanılır.
Yukarıda bahsedilen ```view_item``` olayları tetiklenmeden önce, ürüne tıklanınca tetiklenir. Tıklanan ürünün bilgileri ```select_item``` event isminde dataLayer’a eklenir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "select_item",
    'item_list_name': "Search Results", // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Saat kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
    'items': [{
        'item_name': "Bayan Kol Saati",
        'item_id': "12345",
        'price': 7.70, // Ürün fiyatı
        'item_brand': "Google", // Ürün markası
        'item_category': "Saat", // Ürün kategorisi
        'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
        'item_category3': "3. Kategori",
        'item_category4': "4. Kategori",
        'item_variant': "Metalik Gri",
        'item_list_name': "Search Results", // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Saat kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
    }],
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
 
### 1.3.5 Sepete Ekle.
 
```add_to_cart``` sepete eklenen ürünlerin ölçümünü yapar.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "add_to_cart",
    'currency': "TRY",
    'value': 7.77,
    'items': [{
        'item_name': "Bayan Kol Saati",
        'item_id': "12345",
        'price': 7.70, // Ürün fiyatı
        'item_brand': "Google", // Ürün markası
        'item_category': "Saat", // Ürün kategorisi
        'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
        'item_category3': "3. Kategori",
        'item_category4': "4. Kategori",
        'item_variant': "Metalik Gri", 
        'quantity': 1,
        'id': "12345", // item_id değeri eklenecek.
        'google_business_vertical': 'retail' // Google Ads parametresi, değişmez.
    }],
    'ecommerce': {
        'currencyCode': 'TRY',
        'add': {
            'products': [{
                'name': 'Bayan Kol Saati',
                'id': '12345',
                'price': 7.70,
                'brand': 'Google',
                'category': 'Saat',
                'variant': 'Metalik Gri'
                'quantity': 1
            }]
        }
    }
});
```
 
### 1.3.6 Sepetten Çıkar.
 
```remove_from_cart``` kullanıcı sepet sayfasından bir ürünü çıkardığında gönderilir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "remove_from_cart",
    'currency': "TRY",
    'value': 7.77,
    'items': [{
        'item_name': "Bayan Kol Saati",
        'item_id': "12345",
        'price': 7.70, // Ürün fiyatı
        'item_brand': "Google", // Ürün markası
        'item_category': "Saat", // Ürün kategorisi
        'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
        'item_category3': "3. Kategori",
        'item_category4': "4. Kategori",
        'item_variant': "Metalik Gri",
    }],
    'ecommerce': {
        'remove': {
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

### 1.3.7 Ödeme Başlatma.
 
Bir kullanıcı ödeme işlemini başlattığında gönderilir ve ödeme başlatma işlemlerini ölçmek için kullanılır. Toplanan veri ```begin_checkout``` event isminde dataLayer’a ilave edilir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "begin_checkout",
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
    ],
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
 
### 1.3.8 Alışveriş.

```purchase``` satın alma işlemini tamamladığında gönderilir. Ödeme işlemi başarılı olduğu taktirde aşağıdaki kod bloğu çalışır ve satın alınan ürünlerin listesi obje halinde dataLayer’a eklenilir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': "purchase",
    'transaction_id': "T12345", // Sipariş numarası.
    'value': "809.70", // Sipariş toplam tutarı.
    'tax': "0.00", // Vergi.
    'shipping': "0.00", // Kargo tutarı.
    'currency': "TRY", // Para birimi.
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
    ],
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

## 1.4 Promosyon Görünümleri ve Etkileşimler.
 
Slider/Promosyon görünümlerini ve etkileşimlerini ölçmek, kullanıcılar tarafından ne sıklıkta görüntülendiği ve seçildiği hakkında bilgi verir. Ürün verileriyle birlikte aşağıdaki olaylar da kampanyaların etkileşimini ölçmenize yardımcı olabilir.

Eğer sitenizde slider veya promosyon olarak kullandığınız görsel yoksa bu etkinlikleri yoksayabilirsiniz.
 
### 1.4.1 Slider Etkileşimi.
 
```view_promotion``` bir kullanıcıya belirli bir slider/promosyon gösterildiğinde gönderilir.

Not: Bu event her slider kaydırıldığı zaman tetiklenir.
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': 'view_promotion',
    'creative_name': "Bayan Saati", 
    'creative_slot': "Doğum Günü Hediyesi",
    'location_id': "3", // Slider içinde 3. slide
    'promotion_id': "P_12345", // Silder id numarası
    'promotion_name': "Summer Sale", // Eğer bu ürün bir kampanya ise kampanya ismi.
    'items': [{
        'item_id': "SKU_12345",
        'item_name': "Stan and Friends Tee",
        'currency': "TRY",
        'item_brand': "Google",
        'item_category': "Apparel", // Varsa alt kırılımlar.
        'item_category2': "Adult",
        'item_category3': "Shirts",
        'item_category4': "Crew",
        'item_category5': "Short sleeve",
        'item_variant': "green",
        'location_id': "3",
        'price': 9.99,
    }]
});
```
 
### 1.4.2 Slider Tıklanması
 
```select_promotion``` bir kullanıcı belirli bir slaytı tıkladığında gönderilir.
 
```javascript
window.dataLayer = window.dataLayer || [];
dataLayer.push({
    'event': 'select_promotion',
    'creative_name': "Bayan Saati", 
    'creative_slot': "Doğum Günü Hediyesi",
    'location_id': "3", // Slider içinde 3. slide
    'promotion_id': "P_12345", // Silder id numarası
    'promotion_name': "Summer Sale", // Eğer bu ürün bir kampanya ise kampanya ismi.
    'items': [{
        'item_id': "SKU_12345",
        'item_name': "Stan and Friends Tee",
        'currency': "TRY",
        'item_brand': "Google",
        'item_category': "Apparel", // Varsa alt kırılımlar.
        'item_category2': "Adult",
        'item_category3': "Shirts",
        'item_category4': "Crew",
        'item_category5': "Short sleeve",
        'item_variant': "green",
        'location_id': "3",
        'price': 9.99,
    }]
});
```
