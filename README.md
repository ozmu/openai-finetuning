# Phishing Detection Model - Fine-Tuning with OpenAI GPT-4o-Mini

## Proje Açıklaması

Bu proje, phishing (oltalama) saldırılarını tespit etmek için OpenAI'nin GPT-4o-mini modelini basit bir şekilde fine-tune etme sürecini içerir. Proje kapsamında, phishing ve phishing olmayan e-posta örneklerinden oluşan bir veri seti kullanılarak model eğitilmiştir. Model, siber güvenlik alanında phishing saldırılarına karşı koruma sağlamak amacıyla geliştirilmiştir.

## Kullanım Amacı

Bu model, e-posta içeriklerini analiz ederek, phishing saldırılarını tespit etmek ve nedenlerini açıklamak amacıyla kullanılabilir. Model, eğitim sırasında öğrenilen kalıplar doğrultusunda e-posta içeriklerini değerlendirir ve JSON formatında sonuçlar üretir.

Bu proje, Boğaziçi Üniversitesi'nde düzenlenen Siber Kamp Konferansı'nda sunulmak üzere hazırlanmıştır.

## İçerik

- **dataset.jsonl**: Modelin fine-tune edilmesi için kullanılan veri seti. Bu veri seti, phishing ve phishing olmayan 10 örnek içerir.
- **fine_tuning_script.py**: Modelin fine-tune edilmesi ve test edilmesi için kullanılan Python script'i.
- **README.md**: Proje açıklaması ve kullanım talimatları.

## Gereksinimler

Projenin çalışabilmesi için aşağıdaki gereksinimlerin karşılanması gerekmektedir:

- Python
- OpenAI Python SDK
- OpenAI API Key

Aşağıdaki komutları kullanarak gerekli paketleri kurabilirsiniz:

```bash
pip install openai
````

## Modelin Fine-Tune Edilmesi

Bu proje, OpenAI'nin GPT-4o-mini modelini fine-tune etmek için oluşturulmuştur. Fine-tune işlemi sırasında model, phishing tespiti için özel olarak eğitilmiştir.

### Fine-Tune İşlemi

Veri setini hazırladıktan sonra, aşağıdaki adımları takip ederek modelinizi fine-tune edebilirsiniz:

1. **Veri Setini Yükleyin:**
    
    Öncelikle terminalden openai api key'inizi tanımlayın:
    ```bash
    export OPENAI_API_KEY="your-api-key"
    ```

    Daha sonra, veri setinizi OpenAI platformuna yükleyin:

    ```python
    from openai import OpenAI

    client = OpenAI()

    response = client.files.create(
      file=open("dataset.jsonl", "rb"),
      purpose='fine-tune'
    )

    print("File ID:", response["id"])
    ```

2. **Fine-Tune İşlemini Başlatın:**

    ```python
    response = client.fine_tuning.jobs.create(
      training_file="file-id",
      model="gpt-4o-mini-2024-07-18",
      suffix="my-experiment"
    )
    print("Fine-tune job ID:", response["id"])
    ```

3. **Fine-Tune İşleminin Durumunu Kontrol Edin:**

    Fine-tune işleminizin durumunu kontrol etmek için aşağıdaki kodu kullanabilirsiniz:

    ```python
    response = client.fine_tuning.jobs.retrieve(id="ft-job-id")
    print(response)
    ```

4. **Modeli Kullanın:**

    Fine-tune işlemi tamamlandıktan sonra, aşağıdaki kod ile modelinize sorular sorabilirsiniz:

    ```python
    completion = client.chat.completions.create(
      model="ft:gpt-4o-mini-2024-07-18:your-custom-id",
      messages=[
        {"role": "system", "content": "Bu model phishing ve phishing olmayan e-postaları JSON formatında tespit eder."},
        {"role": "user", "content": "Dear user, your account has been locked due to suspicious activity. Please click the link below to verify your identity."}
      ]
    )
    print(completion.choices[0].message['content'])
    ```

## Katkıda Bulunma

Bu projeye katkıda bulunmak isterseniz, lütfen pull request gönderin veya proje sahibine (@ozmu) ulaşın. Her türlü geri bildirim ve öneri değerlidir.

## İletişim

Bu proje hakkında daha fazla bilgi almak veya sorularınızı iletmek için:

- **E-posta:** ozturkmuhammeet@gmail.com
- **Konferans:** Boğaziçi Üniversitesi Siber Kamp Konferansı