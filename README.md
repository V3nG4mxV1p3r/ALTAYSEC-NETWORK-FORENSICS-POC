# 🌐 AltaySec Blue Team Labs: Network Forensics (PoC)

Bu repository, siber güvenlik analistlerinin (SOC, Olay Müdahale, Tehdit Avcısı) ağ trafiği analizi yeteneklerini geliştirmeleri için tasarlanmış, **kurulum gerektirmeyen (zero-setup)** ve tarayıcı üzerinden çalışan interaktif bir laboratuvar serisidir.

Geleneksel laboratuvarların aksine, zafiyetli makineler kurmak veya devasa PCAP dosyaları indirmek zorunda kalmazsınız. Her seviye kendi izole Docker konteyneri içinde çalışır ve tarayıcınız üzerinden doğrudan analize başlarsınız.

## ✨ Teknik Altyapı & Yenilikler

* **Dinamik PCAP Üretimi (Scapy):** Laboratuvarlar ayağa kalktığında, statik dosyalar kullanmak yerine `Python Scapy` ile gerçek zamanlı olarak geçerli ağ paketleri (IP, DNS, ICMP vb.) ve sızıntı senaryoları üretilir.
* **Anti-Cheat (shc) Doğrulama Sistemi:** Analizcilerin bayrağı (Flag) almak için kullandığı `./submit` doğrulama scriptleri `shc` aracı ile C koduna dönüştürülüp makine dilinde derlenmiştir. Bu sayede kaynak kod (ve bayrak) okunamaz, analizin terminal üzerinden gerçekten yapılması zorunlu kılınır.
* **Terminal-over-Web (Gotty):** Tarayıcınız üzerinden hiçbir VPN veya SSH istemcisine ihtiyaç duymadan, komut satırından `tshark` aracını kullanarak ağ paketlerini analiz edebilirsiniz.

---

## 🛠️ Laboratuvar Senaryoları

### 🟢 Level 1: Fısıldayan Hayalet (DNS Tunneling)
* **Zorluk:** Kolay
* **Odak Noktası:** DNS Protokolü Analizi, Data Exfiltration, tshark Temelleri.
* **Senaryo:** Şirket iç ağındaki bir cihazın, güvenlik duvarını atlatmak için en masum protokol olan DNS'i kullanarak dışarıya gizli veri sızdırdığından şüphelenilmektedir. `evidence.pcap` dosyasını analiz et, anormal DNS (Port 53) sorgularını filtrele ve C2 alan adının içine gizlenmiş Base64 veriyi yakala.

### 🟡 Level 2: Sessiz Yankı (ICMP Exfiltration)
* **Zorluk:** Orta
* **Odak Noktası:** Ping Paketi Analizi, Payload İncelemesi, Hex/ASCII Okuma.
* **Senaryo:** Saldırganlar, ağda hiç dikkat çekmemek için verileri standart `ping` (ICMP) paketlerinin içine saklamıştır. Güvenlik duvarının izin verdiği bu trafiği `tshark -x` parametresiyle detaylı incele, hedefe giden anormal ICMP paketini bul ve paketin veri (load) kısmına gizlenmiş şifreli metni deşifre et.

---

## 🚀 Lab Nasıl Çalıştırılır?

Herhangi bir seviyenin klasör dizinine gidin ve aşağıdaki komutla laboratuvarı inşa edip ayağa kaldırın:

```bash
docker-compose up -d --build
```
Daha sonra web tarayıcınızı açın ve laboratuvarın çalıştığı porta bağlanın (Örn: Kolay seviye için `http://localhost:8083`, Orta seviye için `http://localhost:8084`).

Karşınıza çıkan analist terminalinde görev dosyasını (cat `gorev.txt`) okuyun, `tshark -r evidence.pcap` komutuyla analize başlayın ve bulgularınızı `./submit` komutunu çalıştırarak sisteme girin!

Developed by Emir - Information Security Specialist / Blue Team Lab Researcher
