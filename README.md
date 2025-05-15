# A_DIY_RAG: Basit RAG Yapısına Giriş

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
- `dotenv`: OpenAI API anahtarı gibi hassas verileri .env dosyasından yüklemek için  
- `gradio`: Hızlı prototipleme ve kullanıcı arayüzü oluşturmak için  
- `openai`: OpenAI'nin dil modeli API’si ile etkileşim kurmak için  

## Mevcut Eksiklikler ve Geliştirme Adımları

- Şu an yalnızca anahtar kelime eşleşmesine dayalı basit bir filtreleme yapılmaktadır; semantik benzerlik dikkate alınmamaktadır.  
- Vektör tabanlı arama (vector search) entegrasyonu henüz gerçekleştirilmemiştir.  
- Uzun içeriklerde chunking yapılmadığı için model performansı sınırlı olabilir.  
- LangChain ile geliştirilecek sürümde, vektörel arama, metin parçalama ve gelişmiş bellek yönetimi gibi özellikler eklenecektir.
