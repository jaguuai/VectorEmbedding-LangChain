# 1-A Diy RAG: Basit RAG Yapısına Giriş

Bu çalışma, OpenAI dil modeli destekli, sigorta şirketi çalışanlarına özel bir soru-cevap sistemi geliştirmek amacıyla hazırlanmıştır. Proje, Retrieval-Augmented Generation (RAG) mimarisine dayanmaktadır ve temel işleyişin anlaşılmasını sağlamak üzere sadeleştirilmiş bir yapıyla başlamaktadır.

## Amaç

- RAG mimarisinin temel işleyişini gözlemlemek  
- Gelişmiş vektör aramaya geçmeden önce kurala dayalı bir yapı üzerinde denemeler yapmak  
- Sonraki aşamada LangChain ile semantik arama entegrasyonu için temel oluşturmak  

## Uygulama Özeti

- Veri olarak, AI yardımıyla oluşturulmuş bir şirket bilgi kümesi kullanılmıştır.  
- Kullanıcıdan alınan mesajda `context_title.lower() in message.lower()` ifadesiyle basit bir anahtar kelime eşleşmesi yapılmaktadır.  
- Eşleşen başlıklar üzerinden bağlam oluşturularak OpenAI dil modeline iletilmektedir.  
- Sistem, bu yapısıyla temel düzeyde bir RAG örneği sunmaktadır.  

## Çalışma Mantığı

- **Bağlam Çekme:** `get_relevant_context()` fonksiyonu, kullanıcı sorusundaki isimleri (örneğin Lancaster, Carllm) dosya isimleriyle eşleştirir.  
- **Soru Genişletme:** `add_context()` fonksiyonu ile çekilen bağlam bilgisi kullanıcı mesajına eklenir.  
- **Cevap Üretme:** OpenAI API’si, sistem mesajı ve bağlam ile genişletilmiş kullanıcı sorusunu kullanarak akışlı (stream) cevap üretir.  

## Kullanılan Kütüphaneler

- `glob`: Dosya sisteminden belirli uzantılara sahip belgeleri okumak için  
- `dotenv`: OpenAI API anahtarı gibi hassas verileri `.env` dosyasından yüklemek için  
- `gradio`: Hızlı prototipleme ve kullanıcı arayüzü oluşturmak için  
- `openai`: OpenAI'nin dil modeli API’si ile etkileşim kurmak için  

## Mevcut Eksiklikler ve Geliştirme Adımları

- Şu an yalnızca anahtar kelime eşleşmesine dayalı basit bir filtreleme yapılmaktadır; semantik benzerlik dikkate alınmamaktadır.  
- Vektör tabanlı arama (vector search) entegrasyonu henüz gerçekleştirilmemiştir.  
- Uzun içeriklerde chunking yapılmadığı için model performansı sınırlı olabilir.  
- LangChain ile geliştirilecek sürümde, vektörel arama, metin parçalama ve gelişmiş bellek yönetimi gibi özellikler eklenecektir.  

---

# 2-LangChainTextSplitter: Metin Parçalama ile Gelişmiş RAG Yapısı

Bu çalışma, ilk RAG sisteminin eksikliklerini gidermek ve verimliliği artırmak için **LangChain** kütüphanesi kullanılarak geliştirilmiş bir yapı sunar. Ana odak, metin parçalama (text splitting) ve bağlam yönetimi üzerinedir.

## 1. Metin Parçalama (Text Splitting)

- `CharacterTextSplitter` sınıfı ile belgeler **1000 karakterlik** parçalara bölünür.  
- **200 karakterlik örtüşme (chunk overlap)** sayesinde bağlam kopması önlenir.  
  - Örneğin:
    - İlk parça: Karakter 1–1000  
    - İkinci parça: Karakter 801–1800  
    - Üçüncü parça: Karakter 1601–2600  
    - Böylece parçalar arasında 200 karakterlik çakışma sağlanır.  

## 2. Metadata Entegrasyonu

- Her parçaya `doc_type` (örneğin: employees, contracts) bilgisi eklenir.  
- Bu sayede yalnızca belirli türdeki belgelerde filtreleme ve arama yapılabilir.  

## Mevcut Eksiklikler

- **Vektör Tabanlı Arama:** Henüz semantik benzerlik desteği bulunmamaktadır.  
- **Chunk Optimizasyonu:** Bazı parçalar 1000 karakter sınırını aşabiliyor (uyarı verilse de).  
- **Güvenlik:** Hassas veriler için henüz şifreleme veya RBAC (Role-Based Access Control) desteği eklenmemiştir.  

