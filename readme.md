# Banka API Entegrasyon Standartları


# Güncel API Tablosu

| API | VERSİYON | METODLAR
|--|--|--
| Banka Müşteri Kaydı| V1 | POST
| Müşteri SMS Onay İstek| V1 | POST
| Vadeli Mevduat Hesapları | V1 | GET/POST
| Banka - Ü.P.Y. Kaydı | V1 | POST
*Ü.P.Y. : Üçüncü Parti Yazılım*











# Banka Müşteri Kayıt
Bankaların API üzerinden müşteri kazanmalarına olanak sağlar.
***Önemli not: Bankaların müşteri kaydı sırasında bu API iskeletinde bulunmayan prosedürleri veya parametreleri varsa API' nın ilgili kısımlarında bütünlüğü bozmayacak şekilde eklemeler yapabilirler.***
### POST REQUEST DATA
    {
      "data": [{
        "customer": {
          "name": "Ahmet",
          "surname": "Özgür",
          "email": "deneme@deneme.com",
          "birthday": "30-06-1999",
          "dateType": "DD-MM-YEAR",
          "gender": "M",
          "countryCode": "TR",
          "countryId": 1449,
          "identityNumber": 11111111111,
          "job": "Engineer",
          "placeOfBirth": "İstanbul",
          "nationality": "TR"
        },
        "adress": {
          "adressType": "H",
          "openAdress": "Örnek Mah, Örnek Cad. No: 1, D: 41",
          "provinceCode": 34,
          "zipCode": 34030
        }, 
        "phone": {
          "countryCode": 90,
          "areaCode": 534,
          "phoneBody": 0427578,
          "phoneType": "P"
        },
        "family": {
          "motherName": "Ayşe",
          "fatherName": "Melih",
          "motherMaidenSurname": "Kaşık"
        }
      }]
    }


| PARAMETRE | AÇIKLAMA | VERİ TİPİ | DURUM
|--|--|--|--
| name | Müşterinin adı |string| Zorunlu
| surname | Müşterinin soyadı |string|Zorunlu
| email | Müşterinin email adresi |string|Zorunlu
| birthday | Müşteri doğum tarihi  |date |Zorunlu
| dateType | Doğum tarihi formatı |string|Zorunlu
| gender | Müşterinin cinsiyeti |string|Zorunlu
| countryCode | Müşterinin yaşadığı ülkenin kodu |string|Zorunlu
| countryId | Müşterinin yaşadığı ülkenin ID' si |int|Zorunlu
| identityNumber | Müşterinin T.C. Kimlik Numarası |string|Zorunlu
| job | Müşterinin mesleği |string|Opsiyonel
| placeOfBirth | Müşterinin doğum yeri (İl bazında) |string|Zorunlu
| nationality | Müşterinin uyruğu |string|Zorunlu
| adressType | Müşterinin adres tipi |string|Zorunlu
| openAdress | Müşterinin açık adresi |string|Zorunlu
| provinceCode | Müşteri adresinin il kodu |int|Zorunlu
| zipCode | Müşterinin adresinin posta kodu |string|Zorunlu
| countryCode | Müşteri telefon numarasının ülke kodu |string|Zorunlu
| areaCode | Müşteri telefon numarasının alan kodu |string|Zorunlu
| phoneBody | Müşteri telefon numarasının gövdesi (Alan kodsuz kısım)| string|Zorunlu
| phoneType | Müşteri telefonunun tipi |string|Zorunlu
| motherName | Müşterinin annesinin adı |string|Zorunlu
| fatherName | Müşterinin babasının adı |string|Opsiyonel
| motherMaidenSurname | Müşterinin annesinin kızlık soy adı |string|Zorunlu

### POST RESPONSE DATA

    {
      "data": {
        "customerNo": 11111111111
      },
      "message": {
        "status": "Müşteri başarı ile kaydedildi",
        "statusCode": 0
      }
    }
#### *statusCode içeriği*
|KOD|AÇIKLAMA|
|--|--|
| 0 | Başarılı işlem |
| 1 | Kullanıcı bulunamadı |
| 2 | Hesap bulunamadı |
| 3| Sunucu müsait değil |
| 4| Eksik veri yollandı |
| 5| Diğer tür sorunlar |





# Müşteri Onay İşlemleri

## Müşteri SMS Onay İstek
Son kullanıcının gerçekten iddia edilen kişi olup olmadığını anlamak amacıyla kullanılır.

### POST REQUEST DATA

    {
      "data": {
        "customerIdentityNumber": 111111111111,
        "customerPhoneAreaCode": "534",
        "customerPhoneBody": "0174070"
      }
    }
| PARAMETRE |AÇIKLAMA  | VERİ TİPİ
|--|--|--
| customerIdentityNumber | Müşteri T.C. Kimlik Numarası | string
| customerPhoneAreaCode| Müşteri telefon numarası alan kodu | string
| customerPhoneBody| Müşteri telefon numarası (Alan kodsuz) | string

### POST RESPONSE DATA

    {
      "data": {
        "verificationCode": "123456",
      },
      "message": {
          "status": "Müşteri SMS onay kodu",
          "statusCode": 0
      }
    }

|PARAMETRE|AÇIKLAMA|VERİ TİPİ
|--|--|--
| verificationCode | SMS onay kodu | string
| status| Dönen cevabın durumu hakkında bir mesaj içerir| string
| statusCode| Dönen cevabın başarı kodu | int
#### *statusCode içeriği*
|KOD|AÇIKLAMA|
|--|--|
| 0 | Başarılı işlem |
| 1 | Kullanıcı bulunamadı |
| 2 | Hesap bulunamadı |
| 3| Sunucu müsait değil |
| 4| Eksik veri yollandı |







# Vadeli Mevduat Hesapları 
Vadeli mevduat hesaplarını görüntülenmesini sağlar. Müşteri SMS Onay İstek API' sinden dönen customerNo ile çalışır.
### POST REQUEST DATA
Bankaya üçüncü parti yazılım tarafından yollanan json verisinin yapısı.

    {
      "data" : {
        "customerNo": "11111111111"
      }
    }
|PARAMETRE  | AÇIKLAMA | VERİ TİPİ
|--|--|--
| customerNo| Müşteri numarası | string



### POST RESPONSE DATA

    {
      "data": [{
        "date": {
          "depositEndDate": "30-06-1999",
          "depositStartDate": "30-05-1999",
          "expiryRenewDate": "01-07-1999",
          "lastChangeDate": "12-06-1999",
          "dateType": "DD-MM-YEAR"
        },
        "money": {
          "totalAmount": 12000,
          "freeAmount": 10000,
          "currencyUnit": "TR"
        },
        "account": {
          "accountNo": "TR 68 00062 0 0000222990028402",
          "accountType": 0,
          "isFlexible": true,
          "expiry": 31
        },
        "branch": {
          "branchName": "Bağcılar Şube",
          "branchNumber": 6121020,
          "branchNumberAreaCode": 212,
          "branchId": 8
        }
      }],
      "message": {
        "status": "Aranan hesaba ait vadeli mevduat yatırımları",
        "statusCode": 0
      }
    }
    
| PARAMETRE | AÇIKLAMA | VERİ TİPİ
|--|--| --
| depositEndDate | Vadeli mevduat hesabı bitiş tarihi | string
| depositStartDate | Vadeli mevduat hesabı başlangıç tarihi  | string
| expiryRenewDate | Vadeli mevduat hesabı yenileme tarihi |string
| lastChangeDate | Hesapta gerçekleşen son değişimin tarihi |string
| dateType| Cevaptaki tarihlerin veri tipi. Örnekteki gibi açıkça belirtilmelidir. |string
| totalAmount | Vade sonu elde edilecek toplam para (Anapara + faiz) |float
| freeAmount | Güncel çekilebilir tutar |float
| currencyUnit | Hesabın para biriminin kodu |string
| accountNo | Hesap numarası |string
| accountType | Hesap çeşiti (Aşağıda detaylı açıklandı) |int
| isFlexible | Hesaptan anapara vadede birikmiş miktar ile çekilebilir mi? |boolean
| expiry| Vade süresi (Gün cinsinden) | int
| branchName | Hesabın bağlı olduğu şube adı |string
| branchNumber | Hesabın bağlı olduğu şube telefon numarası  |string
| branchNumberAreaCode | Hesabın bağlı olduğu şube telefon numarası alan kodu |string
| branchId | Şube ID' si |int
| status| Dönen cevabın durumu hakkında bir mesaj içerir| string
| statusCode| Dönen cevabın başarı kodu | int

#### *accountType response içeriği*
 0. Vade sonunda anapara ve faiz vadesiz hesaba geçer
 1. Vade sonunda anapara değerlenmeye devam ederken faiz vadesiz hesaba geçer
 2. Vade sonunda anapara ve faiz vadeli mevduat hesabında değerlenmeye devam eder
#### *statusCode içeriği*
|KOD|AÇIKLAMA|
|--|--|
| 0 | Başarılı işlem |
| 1 | Kullanıcı bulunamadı |
| 2 | Hesap bulunamadı |
| 3| Sunucu müsait değil |
| 4| Eksik veri yollandı |







