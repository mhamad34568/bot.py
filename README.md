import logging
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

# 1. إعدادات المراقبة والتحليل الحي للبوت في الطرفية
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO
)
logger = logging.getLogger(__name__)

# ==========================================
# 2. البيانات الأساسية الموحدة للمنصة
# ==========================================
TOKEN = "8808135399:AAHIyHXWvsoNV9HcLrAdVdsNdmeBBjLOV60"
WALLET_ADDRESS = "0x0411Dfb1CccD2ff25CFbfA8cb36B3013E29Ebba8"

# ==========================================
# 3. دالة بدء التشغيل وعرض الستة أزرار (3 صفوف)
# ==========================================
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        # الصف الأول
        [
            InlineKeyboardButton("💰 إيداع بينانس", callback_query_data='binance_deposit'),
            InlineKeyboardButton("💵 إيداع تيثر (USDT)", callback_query_data='usdt_deposit')
        ],
        # الصف الثاني
        [
            InlineKeyboardButton("💳 طلب سحب الأرباح", callback_query_data='withdraw_request'),
            InlineKeyboardButton("📊 تحليل السوق المباشر", callback_query_data='market_analysis')
        ],
        # الصف الثالث
        [
            InlineKeyboardButton("🛑 إشارة وقف الخسارة", callback_query_data='stop_loss'),
            InlineKeyboardButton("📞 الدعم الفني", callback_query_data='support')
        ]
    ]
    
    reply_markup = InlineKeyboardMarkup(keyboard)
