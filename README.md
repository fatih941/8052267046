# 8052267046
Bot
import requests
import time

# إعدادات البوت
BOT_TOKEN = '8052267046:AAEuPTMsfOS-Kx9sE0a7sAjm2Ct1tF3ofPI'
CHAT_ID = '1182820085'

# العملات التي تريد مراقبتها
SYMBOLS = ['WUSDT', 'IDUSDT', 'TIAUSDT', 'PYTHUSDT', 'HOOKUSDT']

# الحد الأدنى لحجم التداول بالدولار خلال دقيقة (مثلاً 100,000$)
VOLUME_THRESHOLD = 100000  

def send_telegram_message(message):
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage"
    payload = {
        "chat_id": CHAT_ID,
        "text": message
    }
    try:
        requests.post(url, data=payload)
    except Exception as e:
        print(f"خطأ في إرسال الرسالة: {e}")

def check_volume():
    for symbol in SYMBOLS:
        try:
            url = f'https://api.binance.com/api/v3/ticker/24hr?symbol={symbol}'
            res = requests.get(url).json()
            quote_volume = float(res['quoteVolume'])  # بالدولار
            if quote_volume >= VOLUME_THRESHOLD:
                send_telegram_message(f"🚨 دخول سيولة على {symbol[:-4]}: {quote_volume:,.0f}$")
        except Exception as e:
            print(f"خطأ في فحص {symbol}: {e}")

while True:
    check_volume()


    time.sleep(60)  # تحقق كل دقيقة
