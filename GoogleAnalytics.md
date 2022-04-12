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

### 1.3.1 Etkileşim

Google Tag Manager ile yapılandırılan GA4 etiketinde etkileşim olayı otomatik gönderir. Bunun için herhangi bir kurulum yapmanıza gerek yoktur.
 
### 1.3.2 Ürün Listesi Görünümleri.

Kategori sayfalarında listelenen ürünlerin performansını ölçmek için, dataLayer’a bir ürün listesi gönderin ve bu verileri view_item_list olayında toplayın. ‘items’ içindeki objeler sayfadaki ürün sayısı kadar olacaktır.
Arama ile sonuçlanan ürünlerin listesi de bu olaya dahil edilir ve item_list_name değeri “Search Results” olur.

Kategori sayfalarında listelenen ürünlerin performansını ölçmek için, dataLayer’a view_item_list event eklenecek. ‘items’ objesine en fazla 7 adet ürün eklensin. 

# DÜZENLENECEK Arama sonucunda listelenen ürünler de bu olaya dahil edilir ve item_list_name değeri “Search Results” olur.
 
NOT: Eğer arama sonucu ürün bulunamazsa o zaman view_item_list olayı tetiklenmez.

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  'event': "view_item_list",
  'ecommerce': {
  'items': [
  {
	'item_name': "Bayan Kol Saati",
	'item_id': "12345",
	'price': 596.70,
	'item_brand': "Brand",
	'item_category': "Saat",
	'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
	'item_category3': "3. Kategori", 
	'item_category4': "4. Kategori",
	'item_variant': "Metalik Gri",
	'item_list_name': "Bayan Saat Modelleri",
  },
  {
	'item_name': "Erkek Kol Saati",
	'item_id': "67890",
	'price': 240.00,
	'item_brand': "Brand",
	'item_category': "Saat",
	'item_category2': "Erkek Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
	'item_category3': "3. Kategori", 
	'item_category4': "4. Kategori",
	'item_variant': "Metalik Gri"
  }]
  }
});
```
 
### 1.3.3 Ürün Görüntülüme
 
```view_item``` ürün görünümlerini ölçer. Bu olay, ürün sayfası yüklendiğinde gerçekleşir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "view_item",
    'currency': "TRY",
    'value': 7.77,
    'items': [{
        'item_name': "Bayan Kol Saati",
        'item_id': "12345",
        'price': 596.70,
        'item_brand': "Brand",
        'item_category': "Saat",
        'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
        'item_category3': "3. Kategori",
        'item_category4': "4. Kategori",
        'item_variant': "Metalik Gri",
        'item_list_name': "Search Results", // Kullanıcı hangi listeden gelmiş: Arama sonuçlarından, Bayan Saat Modelleri kategorisinden ya da ana sayfada Çok Satılanlar listesinden.
        'id': "12345", // item_id değeri eklenecek.
        'google_business_vertical': 'retail' // Google Ads parametresi, değişmez.
    }],
    'ecommerce': {
        'detail': {
            'actionField': {
                'list': 'Apparel Gallery' // Ürüne hangi listeden geldi? İsteğe bağlıdır. '' boş bırakılabilir.
            },
            'products': [{
                'name': 'Triblend Android T-Shirt',
                'id': '12345',
                'price': '15.25',
                'brand': 'Google',
                'category': 'Apparel',
                'variant': 'Gray'
            }]
        }
    }
});
```
 
### 1.3.4 Ürün Tıklama
 
Bu event, ürün görüntülemelerini veya ürün liste gönüntülemelerinde ürüne tıklamaları ölçmek için kullanılır.
Yukarıda bahsedilen ```view_item``` olayları tetiklenmeden önce, ürüne tıklanınca tetiklenir. Tıklanan ürünün bilgileri ```select_item``` event isminde dataLayer’a eklenir.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "select_item",
    'ecommerce': {
        'items': [{
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 596.70,
            'item_brand': "Guess",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Bayan Saat Modelleri",
        }]
    }
});
```
 
### 1.3.5 Sepete Ekle
 
```add_to_cart``` sepete eklenen ürünlerin ölçümünü yapar.
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "add_to_cart",
    'ecommerce': {
        'items': [{
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 596.70,
            'item_brand': "Guess",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Bayan Saat Modelleri",
        }]
    }
});
```
 
### 1.3.6 Sepetten Çıkar
 
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
            'price': 596.70,
            'item_brand': "Guess",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Bayan Saat Modelleri",
        }]
});
```

### 1.3.7 Ödeme
 
Bir kullanıcı ödeme işlemini başlattığında gönderilir ve ödeme başlatma işlemlerini ölçmek için kullanılır. Toplanan veri ```begin_checkout``` event isminde dataLayer’a ilave edilir.
 
 
```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
    'event': "begin_checkout",
    'currency': "TRY",
    'value': 7.77,
    'items': [{
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 596.70,
            'item_brand': "Guess",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Bayan Saat Modelleri",
        },
        {
            'item_name': "Bayan Kol Saati",
            'item_id': "12345",
            'price': 596.70,
            'item_brand': "Guess",
            'item_category': "Saat",
            'item_category2': "Bayan Saat Modelleri", // Varsa diğer alt kategoriler de eklesin
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri",
            'item_list_name': "Bayan Saat Modelleri",
        }]
});
```
 
### 1.3.8 Alışveriş

```purchase``` event satın alma işlemini tamamladığında gönderilir. Ödeme işlemi başarılı olduğu taktirde aşağıdaki kod bloğu çalışır ve satın alınan ürünlerin listesi obje halinde dataLayer’a eklenilir.
 
 
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
            'item_name': "GUGW0106L1 Bayan Kol Saati", // Ürün ismi.
            'item_id': "67890", // Ürün id
            'price': 596.70, // Ürün fiyatı (NOKTALI)
            'item_brand': "Guess", // Ürün markası
            'item_category': "Saat", // Ürün kategorisi
            // Ürün birkaç kategoriye aitse onlar da dinamik olarak eklensin yoksa tek kategori yeterlidir.
            'item_category2': "Bayan Saat Modelleri",
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri", // Ürün varyasyonu
            'quantity': 2 // Kaç tane eklendi sepete.
        },
        {
            'item_name': "WWG203802 Kol Saati", // Ürün ismi.
            'item_id': "67890", // Ürün id
            'price': 240.00, // Ürün fiyatı (NOKTALI)
            'item_brand': "Wesse", // Ürün markası
            'item_category': "Saat", // Ürün kategorisi
            // Ürün birkaç kategoriye aitse onlar da dinamik olarak eklensin yoksa tek kategori yeterlidir.
            'item_category2': "Bayan Saat Modelleri",
            'item_category3': "3. Kategori",
            'item_category4': "4. Kategori",
            'item_variant': "Metalik Gri", // Ürün varyasyonu
            'quantity': 3 // Kaç tane eklendi sepete.
        }
    ]
});
```
